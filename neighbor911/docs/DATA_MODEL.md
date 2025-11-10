# Neighbor 911 - Data Model & Schema Design

**Version:** 0.1 (AI-Generated Initial Proposal)
**Last Updated:** 2025-01-09
**Status:** ⚠️ **DRAFT - SEEKING REVIEW**

---

## ⚠️ IMPORTANT: ARCHITECTURAL REVIEW NEEDED

This data model is an initial AI-generated proposal and requires review from:
- Database architects with Firestore/NoSQL experience
- Privacy engineers (GDPR/CCPA compliance review)
- Security experts (data access patterns, encryption strategy)

**If you have expertise in these areas, please review and provide feedback.**

---

## Table of Contents

1. [Data Modeling Principles](#1-data-modeling-principles)
2. [Core Entities](#2-core-entities)
3. [Firestore Collections Schema](#3-firestore-collections-schema)
4. [Privacy-Preserving Location Strategy](#4-privacy-preserving-location-strategy)
5. [Real-Time Data Patterns](#5-real-time-data-patterns)
6. [Data Retention & Deletion](#6-data-retention--deletion)
7. [Indexes & Query Optimization](#7-indexes--query-optimization)
8. [Data Migration Strategy](#8-data-migration-strategy)

---

## 1. Data Modeling Principles

### 1.1 Core Principles

**Privacy-First:**
- ❌ Never store long-term exact location (lat/lng) in user profiles
- ✅ Store only city/neighborhood for display purposes
- ✅ Query location in real-time only during active emergencies
- ✅ Encrypt sensitive personal data at rest
- ✅ User can delete all their data (GDPR right to erasure)

**Real-Time First:**
- ✅ Use Firestore real-time listeners for alert status updates
- ✅ Minimize round-trips (denormalize frequently accessed data)
- ✅ Use server timestamps for consistency

**Scalability:**
- ✅ Design for 1M+ users, 10K+ concurrent alerts
- ✅ Avoid hot spots (don't increment global counters)
- ✅ Use geohashing for spatial queries
- ✅ Shard by region if needed (future)

**Auditability:**
- ✅ Immutable alert history (for legal protection, post-incident review)
- ✅ Track critical state changes (alert created → dispatched → accepted → resolved)
- ✅ Preserve responder certifications for liability

---

## 2. Core Entities

### 2.1 Entity Relationship Diagram

```
┌──────────────┐         creates        ┌──────────────┐
│     USER     │ ──────────────────────> │    ALERT     │
│  (Responder/ │                         │              │
│  Originator) │ <────────────────────── │              │
└──────────────┘      responds to        └──────────────┘
       │                                         │
       │ has                                     │ references
       │                                         │
       ▼                                         ▼
┌──────────────┐                         ┌──────────────┐
│ CAPABILITIES │                         │   LOCATION   │
│              │                         │  (ephemeral) │
└──────────────┘                         └──────────────┘
       │
       │ completed
       ▼
┌──────────────┐
│   TRAINING   │
│ CERTIFICATION│
└──────────────┘
```

### 2.2 Key Entities

| Entity | Purpose | Lifetime |
|--------|---------|----------|
| **User** | Responder/originator profile, settings, preferences | Account lifetime |
| **Alert** | Emergency alert instance | Permanent (anonymized after 1 year) |
| **Capability** | User's response capabilities (CPR, AED, wellness check, etc.) | Account lifetime |
| **Training** | Video courses, quizzes, certifications | Permanent |
| **Location** | Geospatial data for proximity matching | Ephemeral (query-time only) |
| **NotificationToken** | FCM device tokens for push notifications | Device lifetime |

---

## 3. Firestore Collections Schema

### 3.1 `/users/{userId}`

**Purpose:** User profile, settings, capabilities

```typescript
interface User {
  // Profile
  userId: string;                    // Firebase Auth UID
  phoneNumber: string;               // E.164 format: "+15551234567"
  name: {
    first: string;
    last: string;
  };
  age?: number;                      // Optional, for age-based alert preferences
  photoUrl?: string;                 // Cloud Storage URL

  // Address (PRIVACY: Store only city/state, NOT exact location)
  address: {
    street?: string;                 // Optional, for display only
    city: string;                    // "San Francisco"
    state: string;                   // "CA"
    zipCode: string;                 // "94102"
    country: string;                 // "US"
    // NO lat/lng stored here!
  };

  // Capabilities (what types of emergencies user can respond to)
  capabilities: Capability[];        // See section 3.2

  // Availability
  availability: {
    isAvailable: boolean;            // Currently available to respond
    lastUpdated: Timestamp;          // When availability was last changed
    suspensionReason?: 'hospitalized' | 'immobilized' | 'in_custody' | 'temporary';
    suspendedUntil?: Timestamp;      // Auto-resume date
  };

  // Alert Schedule (for non-critical alerts)
  alertSchedule: {
    [day: string]: {                 // "monday", "tuesday", etc.
      enabled: boolean;
      startTime: string;             // "18:00" (6 PM)
      endTime: string;               // "23:00" (11 PM)
    }
  };

  // Trusted Responders (for sensitive alerts - ping them first)
  trustedResponders?: string[];      // Array of userIds

  // Settings
  settings: {
    language: string;                // "en", "es", "fr", etc.
    notificationsEnabled: boolean;
    gentleHours?: {                  // For age 70+
      wakeTime: string;              // "07:00"
      bedTime: string;               // "22:00"
    };
  };

  // Metadata
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

### 3.2 `/users/{userId}/capabilities` (Subcollection)

**Purpose:** User's response capabilities and weekly limits

```typescript
interface Capability {
  type: AlertType;                   // "cardiac_arrest", "quit_companion", etc.
  tier: 1 | 2 | 3;                   // Tier 1 (basic), Tier 2 (intermediate), Tier 3 (advanced)

  // Certification
  certified: boolean;
  certifiedAt?: Timestamp;
  certificationExpires?: Timestamp;  // e.g., 6 months for CPR
  trainingCompleted?: {
    videoWatched: boolean;
    quizPassed: boolean;
    score?: number;
  };

  // Equipment (for equipment-based responses like AED, naloxone)
  hasEquipment?: boolean;
  equipmentVerified?: {
    photoUrl?: string;               // Photo of AED/naloxone
    serialNumber?: string;
    expirationDate?: Timestamp;      // For naloxone
  };

  // Weekly Limits (for non-critical alerts)
  weeklyLimit?: number;              // Default 5 for non-critical, null for critical
  alertsThisWeek?: number;           // Counter, resets every Monday
  lastWeekReset?: Timestamp;

  // Metadata
  addedAt: Timestamp;
  updatedAt: Timestamp;
}
```

**Example Capability Document:**
```json
{
  "type": "quit_companion",
  "tier": 1,
  "certified": true,
  "certifiedAt": "2025-01-09T10:00:00Z",
  "weeklyLimit": 3,
  "alertsThisWeek": 2,
  "lastWeekReset": "2025-01-06T00:00:00Z"
}
```

### 3.3 `/alerts/{alertId}`

**Purpose:** Emergency alert instance

```typescript
interface Alert {
  // Identifiers
  alertId: string;                   // Firestore doc ID

  // Alert Type & Status
  type: AlertType;                   // "cardiac_arrest", "wellness_check", etc.
  status: AlertStatus;               // "created" | "dispatching" | "accepted" | "in_progress" | "resolved" | "cancelled"
  criticalityLevel: CriticalityLevel; // "critical" | "security" | "urgent" | "non_critical"

  // Originator (person requesting help)
  originator: {
    userId: string;
    name: string;                    // For display to responders
    phoneNumber: string;             // For responder to call
    photoUrl?: string;
  };

  // Location (ONLY for this specific alert, not stored long-term in user profile)
  location: {
    lat: number;
    lng: number;
    geohash: string;                 // For geospatial queries
    address?: string;                // Human-readable address
    description?: string;            // "Apartment 4B", "Red house on corner"
  };

  // Alert Details
  description?: string;              // Optional details from originator
  personInNeed?: {                   // Optional info about person needing help
    age?: number;
    gender?: string;
    condition?: string;              // "unconscious", "not breathing", etc.
  };

  // Responders
  responders: Responder[];           // See Responder interface below
  targetResponderCount: number;      // How many responders needed (1-5 depending on alert type)

  // Dispatch Waves
  dispatchWaves: DispatchWave[];     // Track wave-based dispatch

  // Timestamps
  timestamps: {
    created: Timestamp;
    dispatched?: Timestamp;
    firstAccepted?: Timestamp;
    arrived?: Timestamp;
    resolved?: Timestamp;
    cancelled?: Timestamp;
  };

  // EMS Coordination
  emsNotified: boolean;              // Whether 911 dispatch was notified
  emsArrivedAt?: Timestamp;

  // Outcome Summary (filled after resolution)
  outcome?: {
    resolved: boolean;
    responderResponseTime?: number;  // Seconds from alert to first responder arrival
    emsResponseTime?: number;        // Seconds from alert to EMS arrival
    timeAdvantage?: number;          // Neighbor time advantage over EMS
    lifesSaved?: boolean;            // Self-reported or confirmed
  };

  // Privacy: Alert history is anonymized after 1 year
  anonymizedAt?: Timestamp;
}

interface Responder {
  userId: string;
  name: string;
  phoneNumber?: string;              // Optional: for originator to call
  status: 'invited' | 'accepted' | 'declined' | 'timeout';

  // Response details
  acceptedAt?: Timestamp;
  declinedAt?: Timestamp;
  arrivedAt?: Timestamp;

  // Transportation & ETA
  transportMethod?: 'driving' | 'running' | 'walking' | 'biking';
  eta?: number;                      // Estimated time to arrival (seconds)

  // Capabilities
  capabilities: string[];            // e.g., ["cpr", "aed"]
  hasEquipment?: string[];           // e.g., ["aed", "naloxone"]
}

interface DispatchWave {
  waveNumber: number;                // 1, 2, 3...
  type: 'trusted' | 'nearby' | 'extended';
  sentAt: Timestamp;
  recipientUserIds: string[];
  acceptedCount: number;
}

type AlertType =
  | 'cardiac_arrest'
  | 'overdose'
  | 'fire'
  | 'drowning'
  | 'wellness_check'
  | 'quit_companion'
  | 'bedroom_consent_emergency'
  | 'domestic_violence'
  | 'sexual_assault'
  | 'active_bystander'
  | 'missing_child'
  | 'missing_pet'
  | 'brain_damage_sport_dissuasion'
  | 'companionship'
  | 'mental_health_crisis';

type AlertStatus =
  | 'created'
  | 'dispatching'
  | 'accepted'
  | 'in_progress'
  | 'resolved'
  | 'cancelled';

type CriticalityLevel =
  | 'critical'       // Life-threatening: CPR, AED, overdose, fire, drowning
  | 'security'       // Safety: consent emergency, DV, SA, missing child (abduction)
  | 'urgent'         // Time-sensitive: wellness check, missing child (lost)
  | 'non_critical';  // Support: quit companion, companionship, brain damage dissuasion
```

### 3.4 `/geospatial/{geohashPrefix}` (For Fast Proximity Queries)

**Purpose:** Spatial index for finding nearby available responders

**⚠️ PRIVACY NOTE:** This collection stores geohashes (approximate location), not exact lat/lng.

```typescript
interface GeospatialIndex {
  userId: string;
  geohash: string;                   // 7-character geohash (~150m precision)
  capabilities: string[];            // ["cpr", "aed", "wellness_check"]
  isAvailable: boolean;
  lastUpdated: Timestamp;

  // This document is EPHEMERAL:
  // - Created when user toggles availability ON
  // - Deleted when user toggles availability OFF
  // - Auto-deleted if lastUpdated > 24 hours ago (Cloud Function cleanup)
}
```

**Update Strategy:**
```dart
// When user toggles availability ON:
await FirebaseFirestore.instance
  .collection('geospatial')
  .doc(userId)
  .set({
    'userId': userId,
    'geohash': Geoflutterfire.geoHash(lat, lng),
    'capabilities': userCapabilities,
    'isAvailable': true,
    'lastUpdated': FieldValue.serverTimestamp()
  });

// When user toggles availability OFF:
await FirebaseFirestore.instance
  .collection('geospatial')
  .doc(userId)
  .delete();
```

### 3.5 `/training/{capabilityType}`

**Purpose:** Training videos, quizzes, certifications

```typescript
interface Training {
  capabilityType: AlertType;
  tier: 1 | 2 | 3;

  // Training Content
  videos: TrainingVideo[];
  quiz: Quiz;

  // Certification Requirements
  certification: {
    requiresQuiz: boolean;
    passingScore: number;            // e.g., 80
    validityPeriod?: string;         // "6 months", "12 months", or null (no expiration)
    requiresEquipment?: boolean;
  };
}

interface TrainingVideo {
  language: string;                  // "en", "es", etc.
  url: string;                       // YouTube URL or Cloud Storage URL
  duration: number;                  // Seconds
  thumbnailUrl?: string;
}

interface Quiz {
  questions: QuizQuestion[];
  passingScore: number;              // 80%
}

interface QuizQuestion {
  question: string;
  options: string[];                 // Multiple choice
  correctAnswerIndex: number;
  explanation?: string;              // Shown after answer
}
```

### 3.6 `/notificationTokens/{userId}`

**Purpose:** Store FCM device tokens for push notifications

```typescript
interface NotificationTokens {
  userId: string;
  tokens: {
    [tokenId: string]: {
      token: string;                 // FCM token
      platform: 'ios' | 'android';
      createdAt: Timestamp;
      lastUsed: Timestamp;
    }
  };
}
```

**Why separate collection?**
- User can have multiple devices (phone + tablet)
- Tokens expire and need to be refreshed
- Clean separation from user profile

---

## 4. Privacy-Preserving Location Strategy

### 4.1 The Problem

**Bad Approach (Privacy Violation):**
```typescript
// ❌ DON'T DO THIS: Storing exact home location in user profile
interface User {
  homeLocation: {
    lat: 40.748817,
    lng: -73.985428
  }
}
```

**Why this is bad:**
- Long-term location tracking
- Could be subpoenaed or leaked
- GDPR/CCPA compliance issues
- User can't truly "delete" their location history

### 4.2 The Solution: Query-Time Location Only

**Good Approach:**
```typescript
// ✅ DO THIS: Query location ONLY when alert is created

// 1. User profile stores only city/state (not exact location)
interface User {
  address: {
    city: "San Francisco",
    state: "CA",
    zipCode: "94102"
    // NO lat/lng
  }
}

// 2. When alert is created, get GPS location at that moment
async function createAlert() {
  // Get location NOW (not from stored profile)
  const position = await Geolocator.getCurrentPosition();

  // Store location ONLY for this alert (not in user profile)
  await firestore.collection('alerts').add({
    type: 'cardiac_arrest',
    location: {
      lat: position.latitude,
      lng: position.longitude,
      geohash: Geoflutterfire.geoHash(position.latitude, position.longitude)
    },
    // ... rest of alert data
  });
}

// 3. Responder location is queried similarly (real-time only)
async function findNearbyResponders(alertLocation) {
  // Query responders who are CURRENTLY available
  // Their location is calculated from geospatial index (which only exists when they're available)
  const responders = await queryGeospatialIndex(alertLocation, radius);

  // No historical location data is accessed
}
```

### 4.3 Geospatial Index Lifecycle

```
User toggles "Available Now" ON
  └─> Get current GPS location
      └─> Create document in /geospatial/{userId} with geohash
          └─> Document exists while user is available

User toggles "Available Now" OFF
  └─> Delete document from /geospatial/{userId}
      └─> User's location is now completely unknown to the system

Cloud Function cleanup (runs daily)
  └─> Delete geospatial documents where lastUpdated > 24 hours ago
      └─> Prevents stale location data from users who force-quit app
```

**Key Benefits:**
- ✅ No long-term location tracking
- ✅ User can delete all location data by deleting account
- ✅ Location only exists during active availability
- ✅ GDPR/CCPA compliant (minimal data retention)

---

## 5. Real-Time Data Patterns

### 5.1 Alert Status Updates (Firestore Streams)

**Pattern:** Originator watches alert document for real-time status changes

```dart
// Originator's phone listens for updates
Stream<Alert> watchAlert(String alertId) {
  return FirebaseFirestore.instance
    .collection('alerts')
    .doc(alertId)
    .snapshots()
    .map((snapshot) => Alert.fromFirestore(snapshot));
}

// When responder accepts, Firestore updates alert document
await alertRef.update({
  'status': 'accepted',
  'responders': FieldValue.arrayUnion([{
    'userId': responderId,
    'name': responderName,
    'acceptedAt': FieldValue.serverTimestamp()
  }])
});

// Originator's UI automatically updates via stream
// "Alert sent" → "Sarah is on the way - ETA 3 min"
```

### 5.2 Denormalization for Performance

**Trade-off:** Duplicate data to avoid extra reads

```typescript
// Example: Alert document includes responder name (denormalized)
interface Alert {
  responders: [
    {
      userId: "abc123",
      name: "Sarah Smith",        // ← Denormalized from /users/{userId}
      phoneNumber: "+15551234567" // ← Denormalized
    }
  ]
}
```

**Why?**
- Originator can see "Sarah is on the way" without additional Firestore read
- Trade-off: If Sarah changes her name, old alerts still show old name (acceptable for historical records)

---

## 6. Data Retention & Deletion

### 6.1 GDPR/CCPA Right to Erasure

**When user deletes account:**
```typescript
async function deleteUserAccount(userId: string) {
  // 1. Delete user profile
  await firestore.collection('users').doc(userId).delete();

  // 2. Delete capabilities subcollection
  await deleteSubcollection(`users/${userId}/capabilities`);

  // 3. Anonymize user in all alerts (don't delete alerts - legal/audit trail)
  const alerts = await firestore.collection('alerts')
    .where('originator.userId', '==', userId)
    .get();

  for (const alert of alerts.docs) {
    await alert.ref.update({
      'originator.name': '[Deleted User]',
      'originator.phoneNumber': '[Deleted]',
      'originator.photoUrl': null
    });
  }

  // 4. Delete notification tokens
  await firestore.collection('notificationTokens').doc(userId).delete();

  // 5. Delete geospatial index entry (if exists)
  await firestore.collection('geospatial').doc(userId).delete();
}
```

### 6.2 Alert History Anonymization

**After 1 year, anonymize non-critical alert details:**

```typescript
// Cloud Function: Runs daily, anonymizes alerts older than 1 year
async function anonymizeOldAlerts() {
  const oneYearAgo = new Date();
  oneYearAgo.setFullYear(oneYearAgo.getFullYear() - 1);

  const oldAlerts = await firestore.collection('alerts')
    .where('timestamps.created', '<', oneYearAgo)
    .where('anonymizedAt', '==', null)
    .get();

  for (const alert of oldAlerts.docs) {
    await alert.ref.update({
      'originator.name': '[Anonymized]',
      'originator.phoneNumber': '[Anonymized]',
      'responders': alert.data().responders.map(r => ({
        ...r,
        name: '[Anonymized]',
        phoneNumber: '[Anonymized]'
      })),
      'location.address': '[Anonymized]',
      'anonymizedAt': FieldValue.serverTimestamp()
    });
  }
}
```

**What's preserved for statistics:**
- Alert type (cardiac arrest, wellness check, etc.)
- Response time metrics
- Geographic region (city/state, not exact address)
- Outcome (resolved, cancelled, etc.)

**What's deleted:**
- Names
- Phone numbers
- Exact addresses
- Photos

---

## 7. Indexes & Query Optimization

### 7.1 Required Firestore Indexes

**For Alert Queries:**
```
Collection: alerts
Fields: status (Ascending), timestamps.created (Descending)
Purpose: Query recent active alerts
```

```
Collection: alerts
Fields: originator.userId (Ascending), timestamps.created (Descending)
Purpose: User's alert history
```

**For Geospatial Queries:**
```
Collection: geospatial
Fields: geohash (Ascending), isAvailable (Ascending)
Purpose: Find nearby available responders
```

**For Training Content:**
```
Collection: training
Fields: capabilityType (Ascending), tier (Ascending)
Purpose: Fetch training for specific capability
```

### 7.2 Composite Index Configuration

**firestore.indexes.json:**
```json
{
  "indexes": [
    {
      "collectionGroup": "alerts",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "status", "order": "ASCENDING" },
        { "fieldPath": "timestamps.created", "order": "DESCENDING" }
      ]
    },
    {
      "collectionGroup": "geospatial",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "geohash", "order": "ASCENDING" },
        { "fieldPath": "isAvailable", "order": "ASCENDING" }
      ]
    }
  ]
}
```

---

## 8. Data Migration Strategy

### 8.1 Schema Versioning

**Add version field to all documents:**
```typescript
interface User {
  _schemaVersion: number;  // Current: 1
  // ... rest of fields
}
```

**When schema changes:**
```typescript
// Cloud Function: Migrate users from v1 to v2
async function migrateUsers() {
  const v1Users = await firestore.collection('users')
    .where('_schemaVersion', '==', 1)
    .get();

  for (const user of v1Users.docs) {
    await user.ref.update({
      '_schemaVersion': 2,
      'newField': defaultValue
    });
  }
}
```

### 8.2 Backward Compatibility

**Client code should handle missing fields gracefully:**
```dart
class UserModel {
  final String userId;
  final int? age;  // Nullable: not all users will have age

  UserModel.fromFirestore(DocumentSnapshot doc) {
    final data = doc.data() as Map<String, dynamic>;
    userId = data['userId'];
    age = data['age'] as int?;  // Safely handle missing field
  }
}
```

---

## Next Steps

1. ✅ Review this data model with database architect
2. ✅ Validate privacy patterns with legal/compliance team
3. ✅ Test geospatial query performance at scale (simulate 10K users)
4. ✅ Create Firestore Security Rules (see ARCHITECTURE.md)
5. ✅ Implement data model in Flutter (freezed classes)
6. ✅ Write unit tests for data serialization/deserialization

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | 2025-01-09 | AI-generated (Claude) | Initial data model proposal |

---

**This data model is a living document.** Feedback and improvements are welcome. Open issues on GitHub or email team@lifesaverlabs.org.
