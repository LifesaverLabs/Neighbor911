# Neighbor 911 - Technical Architecture (PROPOSAL / DRAFT)

**Version:** 0.1 (Living Document)
**Last Updated:** 2025-11-16
**Status:** Open for community review and contributions
**Organization:** Lifesaver Labs Public Benefit Corporation

---

## ⚠️ IMPORTANT: WE NEED YOUR HELP

**This architecture is an AI-generated starting proposal and has NOT been battle-tested in production.**

### We Are Actively Seeking:
- ✅ **Volunteer Early CTO or Lead Architect** with deep mobile experience
- ✅ **Constructive criticism** from experienced mobile developers
- ✅ **Alternative architectural proposals** if you see better approaches
- ✅ **Warning about one-way door decisions** we should avoid before lock-in

### What We Need Help With:
- **Mobile architecture review** (Flutter best practices, scalability patterns)
- **Real-time communication design** (Firebase vs. alternatives, WebSocket patterns)
- **Geospatial query optimization** (GeoFlutterFire vs. custom solutions)
- **Privacy-preserving location architecture** (no long-term storage, real-time queries only)
- **Offline-first patterns** (what must work without network, what requires connectivity)
- **Security review** (authentication, encryption, vulnerability assessment)

**If you have more mobile/backend experience than our founder and can help us avoid architectural mistakes, please reach out:**
- GitHub Issues: [Link to repo]
- Email: team@lifesaverlabs.org
- We're a Public Benefit Corporation building open-source emergency response technology

---

## Table of Contents

1. [System Overview](#1-system-overview)
2. [Technology Stack](#2-technology-stack)
3. [Flutter Package Dependencies](#3-flutter-package-dependencies)
4. [High-Level Architecture](#4-high-level-architecture)
5. [Mobile Application Architecture](#5-mobile-application-architecture)
6. [Backend Architecture (Firebase)](#6-backend-architecture-firebase)
7. [Real-Time Communication](#7-real-time-communication)
8. [Geospatial Query Design](#8-geospatial-query-design)
9. [Privacy & Security Architecture](#9-privacy--security-architecture)
10. [Internationalization (i18n) Strategy](#10-internationalization-i18n-strategy)
11. [Offline-First Patterns](#11-offline-first-patterns)
12. [Performance & Scalability](#12-performance--scalability)
13. [Deployment Architecture](#13-deployment-architecture)
14. [Testing Strategy](#14-testing-strategy)
15. [Open Questions & Decisions Needed](#15-open-questions--decisions-needed)

---

## 1. System Overview

### 1.1 Core Mission
Neighbor 911 is a rapid-response mobile application that connects neighbors in emergencies, enabling 2-3 minute response times vs. 8-15 minute professional EMS response.

### 1.2 Key Technical Requirements
- **Sub-3-second alert delivery** to nearby responders
- **Privacy-first location handling** (no long-term storage, real-time queries only)
- **Cross-platform mobile** (iOS 14+, Android 9+)
- **Bluetooth proximity homing** (±10 feet accuracy)
- **Full internationalization** (i18n) support from day one
- **Offline-first for critical paths** (alert creation, responder acceptance)
- **Real-time coordination** between alert originators and responders
- **Scalable to millions of users** across geographic regions

### 1.3 Non-Functional Requirements
- **Reliability:** 99.9% uptime for alert dispatch
- **Latency:** <3s alert delivery, <500ms API response times
- **Privacy:** GDPR/CCPA compliant, end-to-end encryption for sensitive data
- **Accessibility:** WCAG 2.1 AA compliance for UI
- **Internationalization:** Support for 10+ languages at launch

---

## 2. Technology Stack

### 2.1 Mobile (Cross-Platform)
- **Framework:** Flutter 3.19+ (Dart)
- **Target Platforms:** iOS 14.0+, Android 9.0+ (API level 28+)
- **State Management:** Riverpod (recommended) or Bloc
- **Navigation:** GoRouter (or auto_route)

### 2.2 Backend (BaaS)
- **Primary Backend:** Firebase (Google Cloud)
  - **Authentication:** Firebase Auth (phone verification)
  - **Database:** Cloud Firestore (real-time NoSQL)
  - **Push Notifications:** Firebase Cloud Messaging (FCM)
  - **Storage:** Cloud Storage for Firebase (training videos, user photos)
  - **Functions:** Cloud Functions for Firebase (Node.js/TypeScript)
  - **Analytics:** Firebase Analytics (privacy-preserving)

### 2.3 Supporting Services
- **Maps & Navigation:**
  - Google Maps Platform (Android)
  - Apple MapKit (iOS)
  - Fallback: OpenStreetMap + OSRM (open-source routing)
- **SMS/Phone Verification:** Firebase Auth (Twilio fallback for SMS)
- **Video Hosting:** YouTube (unlisted embeds for training videos)
- **Geospatial Queries:** GeoFlutterFire or custom geohashing

### 2.4 External Integrations (Future)
- **911 Dispatch:** Email/SMS (MVP), CAD API integration (Phase 2)
- **Safeword App:** REST API webhooks (POST alerts to Neighbor 911)
- **Civic Data:** Vote.org API, Google Civic Information API (for suffrage features)

### 2.5 Development & DevOps
- **Version Control:** Git + GitHub
- **CI/CD:** GitHub Actions (or Codemagic for Flutter-specific builds)
- **App Distribution:**
  - iOS: TestFlight (beta), App Store (production)
  - Android: Internal testing (beta), Google Play (production)
- **Monitoring:** Firebase Crashlytics, Sentry (error tracking)
- **Logging:** Firebase Analytics + Cloud Logging

---

## 3. Flutter Package Dependencies

### 3.1 Core Framework & State Management
```yaml
dependencies:
  flutter:
    sdk: flutter

  # State Management
  flutter_riverpod: ^2.4.0  # Reactive state management (recommended)
  # OR bloc: ^8.1.0  # Alternative: BLoC pattern for state management

  # Navigation
  go_router: ^12.0.0  # Declarative routing with deep linking support

  # Internationalization
  intl: ^0.19.0  # Date/number formatting, message translation
  flutter_localizations:
    sdk: flutter  # Built-in Flutter i18n support
```

### 3.2 Firebase & Backend Services
```yaml
dependencies:
  # Firebase Core
  firebase_core: ^2.24.0  # Firebase initialization
  firebase_auth: ^4.15.0  # Phone authentication
  cloud_firestore: ^4.13.0  # Real-time database
  firebase_messaging: ^14.7.0  # Push notifications (FCM)
  firebase_storage: ^11.5.0  # File storage (photos, videos)
  firebase_analytics: ^10.7.0  # Analytics
  firebase_crashlytics: ^3.4.0  # Crash reporting

  # Cloud Functions Integration
  cloud_functions: ^4.5.0  # Call backend Cloud Functions
```

### 3.3 Geospatial & Location
```yaml
dependencies:
  # Location Services
  geolocator: ^10.1.0  # GPS location access (iOS/Android)
  permission_handler: ^11.1.0  # Permission requests (location, notifications)

  # Geospatial Queries
  geoflutterfire_plus: ^0.0.3  # Firestore geospatial queries (geohashing)
  # OR custom geohashing implementation for better control

  # Maps
  google_maps_flutter: ^2.5.0  # Google Maps (Android/iOS)
  # apple_maps_flutter: ^1.0.0  # Apple Maps (iOS native, optional)
```

### 3.4 Real-Time Communication & Bluetooth
```yaml
dependencies:
  # Bluetooth (for proximity homing)
  flutter_blue_plus: ^1.14.0  # Bluetooth Low Energy (BLE) communication

  # Real-time updates
  # (Cloud Firestore already provides real-time streams)
```

### 3.5 UI & UX
```yaml
dependencies:
  # Material Design / Cupertino
  # (Built into Flutter SDK)

  # Image Handling
  cached_network_image: ^3.3.0  # Cached image loading
  image_picker: ^1.0.0  # Camera/gallery access for profile photos

  # Video Player (for training videos)
  youtube_player_flutter: ^8.1.0  # YouTube embed player
  # OR video_player: ^2.8.0  # Generic video player

  # Loading & Feedback
  flutter_spinkit: ^5.2.0  # Loading indicators
  fluttertoast: ^8.2.0  # Toast notifications

  # Icons
  font_awesome_flutter: ^10.6.0  # Extended icon set
```

### 3.6 Security & Encryption
```yaml
dependencies:
  # Secure Storage
  flutter_secure_storage: ^9.0.0  # Encrypted local storage (tokens, sensitive data)

  # Encryption
  encrypt: ^5.0.0  # AES encryption for local data
  crypto: ^3.0.0  # Hashing (SHA-256, etc.)
```

### 3.7 Utilities & Helpers
```yaml
dependencies:
  # HTTP (for REST APIs - external integrations)
  http: ^1.1.0  # HTTP client (for Safeword webhooks, 911 dispatch APIs)
  dio: ^5.4.0  # Alternative: Advanced HTTP client with interceptors

  # JSON Serialization
  json_annotation: ^4.8.0  # Annotations for code generation
  freezed_annotation: ^2.4.0  # Immutable data classes

  # Logging
  logger: ^2.0.0  # Structured logging

  # Date/Time
  timeago: ^3.5.0  # Human-readable time differences ("2 minutes ago")

  # Phone Number Formatting
  intl_phone_number_input: ^0.7.0  # International phone number input
```

### 3.8 Development Dependencies (Dev Tools)
```yaml
dev_dependencies:
  flutter_test:
    sdk: flutter

  # Code Generation
  build_runner: ^2.4.0  # Code generation for json_serializable, freezed
  json_serializable: ^6.7.0  # JSON serialization codegen
  freezed: ^2.4.0  # Immutable data class codegen

  # Linting
  flutter_lints: ^3.0.0  # Official Flutter linting rules

  # Testing
  mockito: ^5.4.0  # Mocking for unit tests
  integration_test:
    sdk: flutter  # Integration testing
```

### 3.9 Package Selection Rationale

| Package | Why This Choice? | Alternatives Considered |
|---------|------------------|-------------------------|
| **Riverpod** | Modern, compile-safe state management; easier than Bloc for most cases | Bloc (more boilerplate), Provider (deprecated patterns) |
| **GoRouter** | Declarative routing, deep linking support, type-safe | auto_route (code generation overhead) |
| **GeoFlutterFire Plus** | Purpose-built for Firestore geospatial queries | Custom geohashing (more control but more work) |
| **Flutter Blue Plus** | Most maintained BLE package for Flutter | flutter_reactive_ble (less community support) |
| **Firebase Auth** | Built-in phone verification, integrates with Firebase ecosystem | Supabase Auth (requires different backend), custom auth (too much work) |
| **YouTube Player Flutter** | Simple YouTube embeds for training videos | video_player + cloud storage (more expensive, slower) |

---

## 4. High-Level Architecture

### 4.1 System Component Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     MOBILE APPS (Flutter)                   │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │ Alert        │  │ Responder    │  │ Navigation &    │  │
│  │ Originator   │  │ Dashboard    │  │ Bluetooth       │  │
│  │ Flow         │  │              │  │ Homing          │  │
│  └──────────────┘  └──────────────┘  └─────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │        Riverpod State Management Layer               │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ HTTPS / WebSocket
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    FIREBASE (Backend)                       │
│                                                             │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ Firebase    │  │ Cloud        │  │ Cloud Functions  │  │
│  │ Auth        │  │ Firestore    │  │ (Node.js)        │  │
│  │ (Phone)     │  │ (Real-time)  │  │                  │  │
│  └─────────────┘  └──────────────┘  └──────────────────┘  │
│                                                             │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ FCM         │  │ Cloud        │  │ Firebase         │  │
│  │ (Push       │  │ Storage      │  │ Analytics        │  │
│  │ Notifications)│ │ (Videos)     │  │                  │  │
│  └─────────────┘  └──────────────┘  └──────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┼─────────┐
                    │                   │
                    ▼                   ▼
          ┌──────────────────┐  ┌────────────────────┐
          │ Google Maps API  │  │ External APIs      │
          │ (Navigation)     │  │ - Safeword (POST)  │
          │                  │  │ - 911 Dispatch     │
          └──────────────────┘  │ - Vote.org         │
                                └────────────────────┘
```

### 4.2 Data Flow for Core Use Case (Cardiac Arrest Alert)

```
1. ALERT CREATION (Originator's Phone)
   ├─> User taps "Cardiac Arrest" emergency type
   ├─> App gets GPS location (geolocator package)
   ├─> App creates alert document in Firestore
   │   └─> Cloud Function triggers on new alert
   │
2. ALERT DISPATCH (Firebase Cloud Function)
   ├─> Geospatial query: Find responders within 0.5 mi with CPR/AED capability
   ├─> Filter by availability, schedule, equipment
   ├─> Send FCM push notifications to top 5 responders
   │
3. ALERT RECEIPT (Responder's Phone)
   ├─> FCM push notification arrives (<3s)
   ├─> App opens to alert detail screen
   ├─> Responder taps "ACCEPT - I'M ON MY WAY"
   ├─> Update alert document in Firestore (status: accepted)
   │   └─> Originator's app receives real-time update via Firestore stream
   │
4. NAVIGATION (Responder's Phone)
   ├─> Google Maps deep link opens with destination
   ├─> Responder drives/runs to location
   ├─> When within 50m, Bluetooth homing activates
   │   ├─> BLE beacon broadcast from originator's phone
   │   └─> Responder's phone calculates distance + direction
   │
5. ARRIVAL & RESOLUTION
   ├─> Proximity < 10 feet → "You've Arrived" screen
   ├─> On-scene tips displayed
   ├─> Responder performs CPR/AED
   ├─> Responder taps "Mark Emergency Resolved"
   ├─> Alert document updated → Originator notified
   └─> Post-emergency summary generated
```

---

## 5. Mobile Application Architecture

### 5.1 Folder Structure (Proposed)

```
lib/
├── main.dart                    # App entry point
├── firebase_options.dart        # Firebase config (generated)
│
├── core/                        # Core utilities
│   ├── constants/
│   │   ├── app_constants.dart   # App-wide constants
│   │   └── route_constants.dart # Route names
│   ├── theme/
│   │   ├── app_theme.dart       # Material/Cupertino themes
│   │   └── colors.dart          # Color palette
│   ├── utils/
│   │   ├── logger.dart          # Logging utility
│   │   ├── validators.dart      # Form validation
│   │   └── date_utils.dart      # Date formatting
│   └── errors/
│       └── exceptions.dart      # Custom exceptions
│
├── data/                        # Data layer
│   ├── models/                  # Data models (freezed classes)
│   │   ├── user_model.dart
│   │   ├── alert_model.dart
│   │   ├── capability_model.dart
│   │   └── location_model.dart
│   ├── repositories/            # Data access layer
│   │   ├── user_repository.dart
│   │   ├── alert_repository.dart
│   │   ├── location_repository.dart
│   │   └── auth_repository.dart
│   └── services/                # External services
│       ├── firebase_service.dart
│       ├── geolocation_service.dart
│       ├── bluetooth_service.dart
│       └── notification_service.dart
│
├── domain/                      # Business logic
│   ├── entities/                # Domain entities (pure Dart)
│   ├── usecases/                # Use cases (business operations)
│   │   ├── create_alert_usecase.dart
│   │   ├── accept_alert_usecase.dart
│   │   └── resolve_alert_usecase.dart
│   └── providers/               # Riverpod providers
│       ├── auth_provider.dart
│       ├── alert_provider.dart
│       └── location_provider.dart
│
├── presentation/                # UI layer
│   ├── screens/                 # Full-page screens
│   │   ├── onboarding/
│   │   │   ├── phone_verification_screen.dart
│   │   │   ├── profile_setup_screen.dart
│   │   │   └── capability_selection_screen.dart
│   │   ├── home/
│   │   │   └── home_screen.dart
│   │   ├── alert/
│   │   │   ├── create_alert_screen.dart
│   │   │   ├── alert_status_screen.dart
│   │   │   └── receive_alert_screen.dart
│   │   ├── navigation/
│   │   │   ├── navigation_screen.dart
│   │   │   └── bluetooth_homing_screen.dart
│   │   └── settings/
│   │       └── settings_screen.dart
│   ├── widgets/                 # Reusable widgets
│   │   ├── buttons/
│   │   │   └── primary_button.dart
│   │   ├── alerts/
│   │   │   └── alert_card.dart
│   │   └── common/
│   │       └── loading_indicator.dart
│   └── navigation/
│       └── app_router.dart      # GoRouter configuration
│
├── l10n/                        # Internationalization
│   ├── app_en.arb               # English strings
│   ├── app_es.arb               # Spanish strings
│   ├── app_fr.arb               # French strings
│   └── ...                      # Additional languages
│
└── generated/                   # Generated code (build_runner)
    └── intl/                    # Generated i18n code
```

### 5.2 State Management Architecture (Riverpod)

**Recommended Pattern: Repository + Provider + Consumer**

```dart
// Example: Alert Creation Flow

// 1. REPOSITORY (data/repositories/alert_repository.dart)
class AlertRepository {
  final FirebaseFirestore _firestore;

  Future<String> createAlert(AlertModel alert) async {
    final docRef = await _firestore.collection('alerts').add(alert.toJson());
    return docRef.id;
  }

  Stream<AlertModel> watchAlert(String alertId) {
    return _firestore.collection('alerts').doc(alertId)
      .snapshots()
      .map((snap) => AlertModel.fromJson(snap.data()!));
  }
}

// 2. PROVIDER (domain/providers/alert_provider.dart)
final alertRepositoryProvider = Provider<AlertRepository>((ref) {
  return AlertRepository();
});

final createAlertProvider = FutureProvider.family<String, AlertModel>(
  (ref, alert) async {
    final repo = ref.read(alertRepositoryProvider);
    return await repo.createAlert(alert);
  },
);

final watchAlertProvider = StreamProvider.family<AlertModel, String>(
  (ref, alertId) {
    final repo = ref.read(alertRepositoryProvider);
    return repo.watchAlert(alertId);
  },
);

// 3. CONSUMER (presentation/screens/alert/create_alert_screen.dart)
class CreateAlertScreen extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return Scaffold(
      body: Column(
        children: [
          ElevatedButton(
            onPressed: () async {
              final alert = AlertModel(/* ... */);
              final alertId = await ref.read(
                createAlertProvider(alert).future
              );
              // Navigate to alert status screen
              context.go('/alert/$alertId');
            },
            child: Text('Send Alert'),
          ),
        ],
      ),
    );
  }
}
```

---

## 6. Backend Architecture (Firebase)

### 6.1 Firebase Project Structure

**Proposed Firebase Collections:**

```
/users/{userId}
  ├─ profile: { name, phone, address, age, photoUrl }
  ├─ capabilities: [ { type, certified, equipment, weeklyLimit } ]
  ├─ availability: { isAvailable, lastUpdated }
  ├─ alertHistory: [ { alertId, timestamp, role, outcome } ]
  └─ settings: { language, notifications, schedule }

/alerts/{alertId}
  ├─ type: "cardiac_arrest" | "wellness_check" | ...
  ├─ status: "created" | "dispatching" | "accepted" | "resolved"
  ├─ originator: { userId, name, phone, location }
  ├─ location: { lat, lng, geohash, address }
  ├─ responders: [
  │   { userId, name, eta, status, acceptedAt }
  │ ]
  ├─ timestamps: { created, dispatched, accepted, resolved }
  └─ metadata: { description, criticalityLevel }

/geospatial/{geohashPrefix}
  └─ users: [ { userId, capabilities, lastSeen } ]
      # Index for fast proximity queries
      # Updated when users toggle availability

/training/{capabilityType}
  ├─ videos: [ { url, duration, language } ]
  ├─ quiz: { questions: [...], passingScore: 80 }
  └─ certification: { validityPeriod: "6 months" }
```

### 6.2 Cloud Functions (Server-Side Logic)

**Proposed Functions:**

```typescript
// functions/src/index.ts

// 1. Alert Dispatch Function
export const onAlertCreated = functions.firestore
  .document('alerts/{alertId}')
  .onCreate(async (snapshot, context) => {
    const alert = snapshot.data();

    // Geospatial query: Find nearby responders
    const responders = await findNearbyResponders(
      alert.location,
      alert.type,
      alert.criticalityLevel
    );

    // Send FCM push notifications
    await sendAlertNotifications(responders, alert);

    // Update alert status
    await snapshot.ref.update({ status: 'dispatching' });
  });

// 2. Geospatial Query Helper
async function findNearbyResponders(
  location: GeoPoint,
  alertType: string,
  criticalityLevel: string
): Promise<UserSnapshot[]> {
  // Use GeoFlutterFire for geohashing
  // Filter by:
  // - Proximity (0.5 mi initially, escalate to 1 mi, 2 mi)
  // - Capability (has selected this alert type)
  // - Availability (isAvailable === true)
  // - Schedule (if non-critical, respect time windows)
  // - Weekly limit (if non-critical, check quota)
}

// 3. Responder Acceptance Function
export const onResponderAccepted = functions.firestore
  .document('alerts/{alertId}')
  .onUpdate(async (change, context) => {
    const before = change.before.data();
    const after = change.after.data();

    // Check if responder count increased
    if (after.responders.length > before.responders.length) {
      // Send notification to alert originator
      await notifyOriginator(after.originator.userId, after);

      // Cancel alerts to other responders if target met
      if (after.responders.length >= getTargetResponderCount(after.type)) {
        await cancelRemainingAlerts(after);
      }
    }
  });

// 4. 911 Dispatch Notification
export const notify911Dispatch = functions.firestore
  .document('alerts/{alertId}')
  .onUpdate(async (change, context) => {
    const after = change.after.data();

    if (after.status === 'accepted' && requiresEMS(after.type)) {
      // Send email/SMS to 911 dispatch
      await send911Notification({
        address: after.location.address,
        type: after.type,
        responder: after.responders[0],
        eta: calculateETA(after.responders[0])
      });
    }
  });
```

### 6.3 Security Rules (Firestore)

```javascript
// firestore.rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users can only read/write their own profile
    match /users/{userId} {
      allow read: if request.auth.uid == userId;
      allow write: if request.auth.uid == userId;
    }

    // Alerts: Originator can create, responders can update
    match /alerts/{alertId} {
      allow create: if request.auth != null;
      allow read: if request.auth != null;
      allow update: if request.auth != null
        && (isOriginator() || isResponder());

      function isOriginator() {
        return request.auth.uid == resource.data.originator.userId;
      }

      function isResponder() {
        return request.auth.uid in resource.data.responders.map(r => r.userId);
      }
    }

    // Training content is public (read-only)
    match /training/{document=**} {
      allow read: if true;
      allow write: if false;
    }
  }
}
```

---

## 7. Real-Time Communication

### 7.1 Push Notifications (FCM)

**Alert Notification Payload:**
```json
{
  "notification": {
    "title": "🚨 CARDIAC ARREST - 0.3 miles away",
    "body": "Male, 60s, not breathing. Tap to respond."
  },
  "data": {
    "alertId": "abc123",
    "type": "cardiac_arrest",
    "distance": "0.3",
    "location": "{ lat: 40.7, lng: -74.0 }",
    "priority": "high"
  },
  "android": {
    "priority": "high",
    "notification": {
      "channel_id": "critical_alerts",
      "sound": "emergency_alert.mp3"
    }
  },
  "apns": {
    "payload": {
      "aps": {
        "sound": "emergency_alert.caf",
        "interruption-level": "critical"
      }
    }
  }
}
```

**Notification Channels (Android):**
- **critical_alerts**: Life-threatening emergencies (bypass DND)
- **security_alerts**: Safety/security emergencies
- **standard_alerts**: Non-critical requests

### 7.2 Real-Time Updates (Firestore Streams)

```dart
// Example: Watch alert status in real-time
Stream<AlertModel> watchAlertStatus(String alertId) {
  return FirebaseFirestore.instance
    .collection('alerts')
    .doc(alertId)
    .snapshots()
    .map((snapshot) => AlertModel.fromFirestore(snapshot));
}

// In UI:
final alertStream = ref.watch(watchAlertProvider(alertId));

alertStream.when(
  data: (alert) {
    if (alert.status == 'accepted') {
      // Show "Sarah is on the way - ETA 3 min"
    }
  },
  loading: () => CircularProgressIndicator(),
  error: (err, stack) => Text('Error: $err'),
);
```

---

## 8. Geospatial Query Design

### 8.1 Geohashing Strategy (GeoFlutterFire Plus)

**Why Geohashing?**
Firestore doesn't natively support geospatial radius queries. Geohashing converts lat/lng into a string prefix that enables proximity queries.

**Example:**
```dart
import 'package:geoflutterfire_plus/geoflutterfire_plus.dart';

// Store user location with geohash
Future<void> updateUserLocation(String userId, double lat, double lng) async {
  final geo = GeoCollectionReference(
    FirebaseFirestore.instance.collection('users')
  );

  await geo.add(
    userId,
    GeoPoint(lat, lng),
    extraData: {
      'capabilities': ['cpr', 'aed'],
      'isAvailable': true
    }
  );
}

// Query nearby responders
Stream<List<DocumentSnapshot>> findNearbyResponders(
  double centerLat,
  double centerLng,
  double radiusMiles
) {
  final geo = GeoCollectionReference(
    FirebaseFirestore.instance.collection('users')
  );

  return geo.subscribeWithin(
    center: GeoPoint(centerLat, centerLng),
    radiusInKm: radiusMiles * 1.60934,  // Convert to km
    field: 'location',
    strictMode: true
  );
}
```

### 8.2 Privacy-Preserving Location Queries

**Key Principle:** Never store exact long-term location. Only query in real-time during emergencies.

```dart
// GOOD: Query location only when alert created
Future<void> createAlert(AlertType type) async {
  // Get current location
  final position = await Geolocator.getCurrentPosition();

  // Create alert with location (stored ONLY for this alert)
  await FirebaseFirestore.instance.collection('alerts').add({
    'type': type,
    'location': GeoPoint(position.latitude, position.longitude),
    'timestamp': FieldValue.serverTimestamp()
  });

  // User's main profile does NOT store exact location
}

// BAD: Don't store home address as exact lat/lng in user profile
// Instead: Store only city/neighborhood for display purposes
```

---

## 9. Privacy & Security Architecture

### 9.1 Authentication Flow

```
1. User enters phone number
2. Firebase Auth sends SMS verification code
3. User enters code
4. Firebase Auth creates user account (uid)
5. App creates user profile document in Firestore
6. User proceeds to onboarding
```

**Security:**
- Phone verification prevents spam accounts
- Firebase Auth handles token refresh automatically
- Tokens stored in secure storage (flutter_secure_storage)

### 9.2 Data Encryption

**At Rest:**
- Firestore: Encrypted by default (Google-managed keys)
- Cloud Storage: Encrypted by default
- Sensitive local data: flutter_secure_storage (AES-256)

**In Transit:**
- All Firebase connections use HTTPS/TLS 1.3
- No unencrypted network traffic

### 9.3 Privacy Policies

**Location Data:**
- ✅ Query location in real-time during emergencies
- ❌ Never store long-term location history
- ✅ Store only city/neighborhood for display
- ✅ User can view/delete all their data (GDPR right to erasure)

**Personal Data Retention:**
- **Active users:** Data retained indefinitely while account active
- **Deleted accounts:** All data deleted within 30 days
- **Alert history:** Anonymized after 1 year (remove PII, keep aggregates)

---

## 10. Internationalization (i18n) Strategy

### 10.1 Flutter i18n Setup

**Enable i18n in `pubspec.yaml`:**
```yaml
flutter:
  generate: true  # Enable code generation

dependencies:
  flutter_localizations:
    sdk: flutter
  intl: ^0.19.0
```

**Create `l10n.yaml`:**
```yaml
arb-dir: lib/l10n
template-arb-file: app_en.arb
output-localization-file: app_localizations.dart
```

### 10.2 ARB Files (Application Resource Bundle)

**lib/l10n/app_en.arb (English):**
```json
{
  "@@locale": "en",

  "appTitle": "Neighbor 911",
  "@appTitle": {
    "description": "Application title"
  },

  "alertCardiacArrest": "Cardiac Arrest",
  "@alertCardiacArrest": {
    "description": "Emergency type: Cardiac arrest"
  },

  "acceptButtonText": "ACCEPT - I'M ON MY WAY",
  "@acceptButtonText": {
    "description": "Button to accept emergency alert"
  },

  "alertDistance": "{distance} miles away",
  "@alertDistance": {
    "description": "Distance to emergency",
    "placeholders": {
      "distance": {
        "type": "double",
        "format": "decimalPattern"
      }
    }
  },

  "etaMinutes": "ETA: {minutes} min",
  "@etaMinutes": {
    "placeholders": {
      "minutes": {
        "type": "int"
      }
    }
  }
}
```

**lib/l10n/app_es.arb (Spanish):**
```json
{
  "@@locale": "es",
  "appTitle": "Vecino 911",
  "alertCardiacArrest": "Paro Cardíaco",
  "acceptButtonText": "ACEPTAR - VOY EN CAMINO",
  "alertDistance": "A {distance} millas",
  "etaMinutes": "ETA: {minutes} min"
}
```

### 10.3 Using Localized Strings in Code

```dart
import 'package:flutter_gen/gen_l10n/app_localizations.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final l10n = AppLocalizations.of(context)!;

    return Text(
      l10n.alertDistance(0.3)  // "0.3 miles away" or "A 0.3 millas"
    );
  }
}
```

### 10.4 Supported Languages (MVP Target)

**Launch Languages:**
1. English (en)
2. Spanish (es)
3. French (fr)
4. Mandarin Chinese (zh)
5. Arabic (ar)
6. Portuguese (pt)
7. Russian (ru)
8. Hindi (hi)
9. German (de)
10. Japanese (ja)

Note: Chinese dialects would be super-high on this list if China didn't separate its app and technology stack from the rest of the world, in an effort to enforce ideological separation and enforcement separate from the rest of the human race who enjoy access to Google and open society services. The founder used to serve in China as a teacher; it sucks that this can't be launched in China, the second largest global population, rapidly and early on first launch. We hope Chinese engineers and designers fork, copy, and branch this repository aggressively to help Chinese residents with a local Neighbor 119. We wish the same for every other nation that needs this faster, especially those with authoritarian free speech and free discussion restrictions and technology barriers, like China or Russia.

**Post-MVP:** Community-contributed translations via Crowdin or similar

### 10.5 Locale Detection

```dart
// Detect system locale, fallback to English
MaterialApp(
  localizationsDelegates: AppLocalizations.localizationsDelegates,
  supportedLocales: AppLocalizations.supportedLocales,
  locale: _getPreferredLocale(),  // User setting or system default
);
```

---

## 11. Offline-First Patterns

### 11.1 What Must Work Offline?

**Critical Offline Capabilities:**
- ✅ View previously loaded alert history
- ✅ Accept alert (queue action, sync when online)
- ✅ View on-scene tips (cached locally)
- ✅ Navigate using cached map tiles (limited)

**Requires Connectivity:**
- ❌ Create new alert (needs real-time dispatch)
- ❌ Receive new alerts (needs FCM)
- ❌ Geospatial queries (needs Firestore)
- ❌ Bluetooth homing (local, but requires originator's phone to be online)

### 11.2 Firestore Offline Persistence

```dart
// Enable offline persistence (caches data locally)
await FirebaseFirestore.instance
  .settings = Settings(persistenceEnabled: true);

// Firestore automatically syncs when connection restored
```

### 11.3 Connectivity Detection

```dart
import 'package:connectivity_plus/connectivity_plus.dart';

final connectivityProvider = StreamProvider<ConnectivityResult>((ref) {
  return Connectivity().onConnectivityChanged;
});

// In UI:
final connectivity = ref.watch(connectivityProvider);
connectivity.when(
  data: (result) {
    if (result == ConnectivityResult.none) {
      // Show offline banner
    }
  },
  loading: () => null,
  error: (_, __) => null,
);
```

---

## 12. Performance & Scalability

### 12.1 Target Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Alert delivery latency | <3 seconds | Firebase Performance Monitoring |
| App cold start time | <2 seconds | Firebase Performance Monitoring |
| Firestore query time | <500ms | Cloud Firestore monitoring |
| Bluetooth homing accuracy | ±10 feet | User testing + GPS validation |
| App size (Android) | <50 MB | APK analyzer |
| App size (iOS) | <60 MB | App Store Connect |

### 12.2 Scalability Considerations

**Firestore Limits:**
- Max writes/second per document: 1 (not a concern for alerts)
- Max reads/second: 10,000+ (sufficient for MVP)
- Max document size: 1 MB (alerts are ~5-10 KB)

**FCM Limits:**
- Max message rate: 10,000 messages/second (sufficient)
- Max payload size: 4 KB (our alerts are ~1 KB)

**Scaling Strategy:**
- **Phase 1 (MVP):** Single Firebase project, single region (us-central1)
- **Phase 2 (Growth):** Multi-region Firestore, regional Cloud Functions
- **Phase 3 (Scale):** Sharding by geographic region (separate Firebase projects per region)

---

## 13. Deployment Architecture

### 13.1 Environments

```
Development:
  - Firebase Project: neighbor911-dev
  - iOS: TestFlight (internal)
  - Android: Internal testing
  - Domain: dev.neighbor911.app

Staging:
  - Firebase Project: neighbor911-staging
  - iOS: TestFlight (external)
  - Android: Open testing
  - Domain: staging.neighbor911.app

Production:
  - Firebase Project: neighbor911-prod
  - iOS: App Store
  - Android: Google Play
  - Domain: neighbor911.app
```

### 13.2 CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/flutter-ci.yml
name: Flutter CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.19.0'

      - name: Install dependencies
        run: flutter pub get

      - name: Run tests
        run: flutter test

      - name: Build APK
        run: flutter build apk --release

      - name: Build iOS (on macOS runner)
        run: flutter build ios --release --no-codesign
```

### 13.3 Release Process

1. **Version bump** in `pubspec.yaml`
2. **Run tests** (`flutter test`)
3. **Build artifacts**:
   - Android: `flutter build appbundle --release`
   - iOS: `flutter build ipa --release`
4. **Upload to stores**:
   - Android: Upload to Google Play Console
   - iOS: Upload to App Store Connect via Xcode
5. **Tag release** in Git: `git tag v1.0.0`
6. **Deploy Cloud Functions**: `firebase deploy --only functions`

---

## 14. Testing Strategy

### 14.1 Testing Pyramid

```
        ┌────────────┐
        │    E2E     │  (5% - Critical user flows)
        │Integration │
        └────────────┘
       ┌──────────────┐
       │     Widget   │  (20% - UI components)
       │    Tests     │
       └──────────────┘
      ┌────────────────┐
      │   Unit Tests   │  (75% - Business logic, models)
      └────────────────┘
```

### 14.2 Unit Tests

```dart
// test/data/models/alert_model_test.dart
void main() {
  group('AlertModel', () {
    test('fromJson creates valid model', () {
      final json = {
        'type': 'cardiac_arrest',
        'status': 'created',
        'location': {'lat': 40.7, 'lng': -74.0}
      };

      final alert = AlertModel.fromJson(json);

      expect(alert.type, AlertType.cardiacArrest);
      expect(alert.status, AlertStatus.created);
    });

    test('toJson serializes correctly', () {
      final alert = AlertModel(
        type: AlertType.cardiacArrest,
        status: AlertStatus.created
      );

      final json = alert.toJson();

      expect(json['type'], 'cardiac_arrest');
    });
  });
}
```

### 14.3 Widget Tests

```dart
// test/presentation/widgets/alert_card_test.dart
void main() {
  testWidgets('AlertCard displays correctly', (tester) async {
    final alert = AlertModel(
      type: AlertType.cardiacArrest,
      distance: 0.3
    );

    await tester.pumpWidget(
      MaterialApp(
        home: Scaffold(
          body: AlertCard(alert: alert)
        )
      )
    );

    expect(find.text('Cardiac Arrest'), findsOneWidget);
    expect(find.text('0.3 miles away'), findsOneWidget);
  });
}
```

### 14.4 Integration Tests

```dart
// integration_test/create_alert_flow_test.dart
void main() {
  testWidgets('Create alert flow - end to end', (tester) async {
    app.main();
    await tester.pumpAndSettle();

    // Login
    await tester.enterText(find.byKey(Key('phone_input')), '5551234567');
    await tester.tap(find.byKey(Key('send_code_button')));
    await tester.pumpAndSettle();

    // Create alert
    await tester.tap(find.byKey(Key('create_alert_button')));
    await tester.tap(find.text('Cardiac Arrest'));
    await tester.tap(find.byKey(Key('send_alert_button')));
    await tester.pumpAndSettle();

    // Verify alert created
    expect(find.text('Alert sent to 5 nearby responders'), findsOneWidget);
  });
}
```

---

## 15. Open Questions & Decisions Needed

### 15.1 High-Priority Decisions

| Question | Options | Recommendation | Status |
|----------|---------|----------------|--------|
| **State management** | Riverpod vs. Bloc | Riverpod (simpler, less boilerplate) | ⚠️ Needs review |
| **Geospatial library** | GeoFlutterFire Plus vs. custom | GeoFlutterFire Plus (MVP), custom later | ⚠️ Needs review |
| **Navigation** | GoRouter vs. auto_route | GoRouter (official, type-safe) | ⚠️ Needs review |
| **Maps** | Google Maps vs. Apple Maps vs. OSM | Google Maps (MVP), add OSM later | ⚠️ Needs review |
| **Video hosting** | YouTube vs. Cloud Storage | YouTube (cheaper, faster) | ⚠️ Needs review |
| **Bluetooth strategy** | BLE beacons vs. WebRTC | BLE (simpler, works offline) | ⚠️ Needs review |

### 15.2 Medium-Priority Decisions

- **Offline support level:** How much can/should work offline?
- **Multi-region strategy:** When to shard by geography?
- **Background location:** Do we need background location for auto-availability?
- **Analytics privacy:** What metrics to collect without compromising privacy?
- **Image optimization:** CDN for user photos, or rely on Firebase Storage?

### 15.3 Questions for Volunteer Architect/CTO

1. **Is Firebase the right backend choice for this use case?** (Alternatives: Supabase, AWS Amplify, custom Node.js backend)
2. **Is Riverpod the right state management for a team of volunteers?** (Learning curve vs. power)
3. **How should we handle multi-tenancy if we expand internationally?** (Separate Firebase projects per country?)
4. **What's the best Bluetooth proximity strategy?** (BLE beacons, iBeacon, Eddystone, custom?)
5. **Should we use Flutter Web for admin dashboard?** (Or separate React app?)
6. **What's our disaster recovery plan?** (Firestore backups, failover regions)

---

## Next Steps

### Before Sprint 1:
1. ✅ Get architectural review from experienced mobile developers
2. ✅ Finalize state management choice (Riverpod vs. Bloc)
3. ✅ Validate Firebase Security Rules with security expert
4. ✅ Review geospatial query performance at scale
5. ✅ Create detailed data model document (see [DATA_MODEL.md](DATA_MODEL.md))
6. ✅ Create API specification for external integrations (see [API_SPECIFICATION.md](API_SPECIFICATION.md))

### During Sprint 1:
1. Set up Firebase project (dev environment)
2. Create Flutter project with folder structure
3. Implement phone authentication flow
4. Set up CI/CD pipeline (GitHub Actions)

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | 2025-11-10 | AI-generated (Claude) | Initial architecture proposal |

---

## Contributors & Reviewers Needed

**We are actively seeking technical review from:**
- Mobile architects with Flutter experience
- Backend engineers familiar with Firebase at scale
- Security experts for privacy/encryption review
- DevOps engineers for CI/CD optimization
- UX engineers for accessibility review

**How to contribute:**
- Open issues on GitHub with architectural feedback
- Submit PRs with alternative proposals
- Email team@lifesaverlabs.org with questions

---

**This is a living document.** As we learn and iterate, this architecture will evolve. All suggestions and criticisms are welcome—this is open-source, community-driven emergency response technology. Let's build it right together.
