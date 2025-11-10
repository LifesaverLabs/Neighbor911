# Neighbor 911 - API Specification

**Version:** 0.1 (AI-Generated Initial Proposal)
**Last Updated:** 2025-01-09
**Status:** ⚠️ **DRAFT - SEEKING REVIEW**

---

## ⚠️ IMPORTANT: API DESIGN REVIEW NEEDED

This API specification is an initial AI-generated proposal and requires review from:
- Backend engineers with REST API design experience
- Security engineers (authentication, rate limiting, abuse prevention)
- Integration engineers familiar with 911 CAD systems
- External partners (Safeword, dispatch centers, civic data providers)

**If you have API design expertise, please review and provide feedback.**

---

## Table of Contents

1. [API Overview](#1-api-overview)
2. [Authentication](#2-authentication)
3. [External App → Neighbor 911 (Safeword Integration)](#3-external-app--neighbor-911-safeword-integration)
4. [911 Dispatch Centers → Neighbor 911 (Dispatcher-Initiated Alerts)](#4-911-dispatch-centers--neighbor-911-dispatcher-initiated-alerts)
5. [Neighbor 911 → 911 Dispatch Systems](#5-neighbor-911--911-dispatch-systems)
6. [Neighbor 911 → Civic Data APIs](#6-neighbor-911--civic-data-apis)
7. [Internal Mobile ↔ Firebase API](#7-internal-mobile--firebase-api)
8. [Webhooks & Callbacks](#8-webhooks--callbacks)
9. [Rate Limiting & Abuse Prevention](#9-rate-limiting--abuse-prevention)
10. [Error Handling](#10-error-handling)
11. [API Versioning Strategy](#11-api-versioning-strategy)

---

## 1. API Overview

### 1.1 API Categories

Neighbor 911 has **three primary API integration points:**

1. **Inbound: External Apps → Neighbor 911**
   - Example: Safeword app triggers a bedroom consent emergency alert
   - Method: REST API webhooks (POST to Neighbor 911)

2. **Inbound: 911 Dispatch Centers → Neighbor 911**
   - Example: 911 dispatcher requests nearby CPR responders for active cardiac arrest call
   - Method: REST API (POST to Neighbor 911)

3. **Outbound: Neighbor 911 → 911 Dispatch Systems**
   - Example: Notify 911 dispatch that a neighbor responder is en route with AED
   - Method: Email/SMS (MVP), REST API to CAD systems (Phase 2)

4. **Outbound: Neighbor 911 → Civic Data APIs**
   - Example: Fetch voter registration status, polling locations
   - Method: REST API calls to Vote.org, Google Civic Information API

### 1.2 Technology Stack

**API Framework:**
- Cloud Functions for Firebase (Node.js/TypeScript)
- Express.js for REST endpoints
- HTTPS only (TLS 1.3)

**Authentication:**
- API Keys (for trusted partners like Safeword)
- JWT tokens (for future public API)
- Firebase Auth (internal mobile app ↔ backend)

**Rate Limiting:**
- Cloud Functions built-in concurrency limits
- Redis (or Firestore) for custom rate limiting

---

## 2. Authentication

### 2.1 API Key Authentication (For Partners like Safeword)

**Request Header:**
```http
POST /api/v1/alerts
Authorization: Bearer sk_live_abc123...
Content-Type: application/json
```

**API Key Format:**
- `sk_test_*` - Test environment keys
- `sk_live_*` - Production environment keys

**Key Generation:**
```bash
# Admin generates API key for Safeword
firebase functions:config:set safeword.api_key="sk_live_xyz789..."
```

**Validation (Server-Side):**
```typescript
// Cloud Function middleware
function validateApiKey(req, res, next) {
  const authHeader = req.headers['authorization'];
  const apiKey = authHeader?.split('Bearer ')[1];

  if (!apiKey || !isValidApiKey(apiKey)) {
    return res.status(401).json({ error: 'Invalid API key' });
  }

  next();
}
```

### 2.2 Firebase Auth (Internal Mobile App)

**All mobile app requests use Firebase ID tokens:**
```dart
// Client-side (Flutter)
final user = FirebaseAuth.instance.currentUser;
final idToken = await user?.getIdToken();

final response = await http.post(
  Uri.parse('https://us-central1-neighbor911.cloudfunctions.net/createAlert'),
  headers: {
    'Authorization': 'Bearer $idToken',
    'Content-Type': 'application/json'
  },
  body: jsonEncode({ /* alert data */ })
);
```

**Server-side validation:**
```typescript
import * as admin from 'firebase-admin';

async function verifyFirebaseToken(req, res, next) {
  const idToken = req.headers['authorization']?.split('Bearer ')[1];

  try {
    const decodedToken = await admin.auth().verifyIdToken(idToken);
    req.user = decodedToken;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Unauthorized' });
  }
}
```

---

## 3. External App → Neighbor 911 (Safeword Integration)

### 3.1 Create Alert from Safeword (POST /api/v1/alerts)

**Endpoint:**
```
POST https://us-central1-neighbor911.cloudfunctions.net/api/v1/alerts
```

**Authentication:**
```
Authorization: Bearer sk_live_safeword_abc123
```

**Request Body:**
```json
{
  "externalSource": "safeword",
  "externalAlertId": "safeword_alert_789",
  "type": "bedroom_consent_emergency",
  "originator": {
    "externalUserId": "safeword_user_456",
    "name": "Jamie Smith",
    "phoneNumber": "+15551234567"
  },
  "location": {
    "lat": 40.748817,
    "lng": -73.985428,
    "address": "123 Main St, Apt 7C, New York, NY 10001"
  },
  "description": "Safeword called - need bystander witness",
  "metadata": {
    "safewordTriggerMethod": "voice",
    "safewordTriggeredAt": "2025-01-09T14:30:00Z"
  },
  "alertTrustedRespondersFirst": true
}
```

**Response (Success):**
```json
{
  "success": true,
  "alertId": "neighbor911_alert_abc123",
  "status": "dispatching",
  "estimatedResponders": 3,
  "webhookUrl": "https://us-central1-neighbor911.cloudfunctions.net/webhooks/safeword/alert_abc123"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": {
    "code": "INVALID_LOCATION",
    "message": "Location coordinates are required",
    "field": "location.lat"
  }
}
```

### 3.2 Webhook Callbacks (Neighbor 911 → Safeword)

**When alert status changes, Neighbor 911 sends webhook to Safeword:**

**POST to Safeword's webhook URL:**
```json
{
  "event": "alert.accepted",
  "alertId": "neighbor911_alert_abc123",
  "externalAlertId": "safeword_alert_789",
  "timestamp": "2025-01-09T14:32:15Z",
  "data": {
    "status": "accepted",
    "responder": {
      "name": "Alex Johnson",
      "phoneNumber": "+15559876543",
      "eta": 120
    }
  }
}
```

**Webhook Events:**
- `alert.created` - Alert dispatched to responders
- `alert.accepted` - Responder accepted
- `alert.arrived` - Responder arrived on scene
- `alert.resolved` - Emergency resolved
- `alert.cancelled` - Alert cancelled

### 3.3 API Contract (TypeScript Interface)

```typescript
// API request from Safeword to Neighbor 911
interface CreateAlertRequest {
  externalSource: 'safeword' | 'other';
  externalAlertId: string;
  type: AlertType;
  originator: {
    externalUserId: string;
    name: string;
    phoneNumber: string;
  };
  location: {
    lat: number;
    lng: number;
    address?: string;
  };
  description?: string;
  metadata?: Record<string, any>;
  alertTrustedRespondersFirst?: boolean;
}

// API response from Neighbor 911 to Safeword
interface CreateAlertResponse {
  success: boolean;
  alertId?: string;
  status?: 'dispatching' | 'no_responders_available';
  estimatedResponders?: number;
  webhookUrl?: string;
  error?: {
    code: string;
    message: string;
    field?: string;
  };
}
```

---

## 4. 911 Dispatch Centers → Neighbor 911 (Dispatcher-Initiated Alerts)

### 4.1 Use Case: 911 Dispatcher Requests Neighbor Response

**Scenario:**
1. 911 dispatcher receives cardiac arrest call at 123 Main St
2. Dispatcher sees EMS is 12 minutes away
3. Dispatcher uses CAD system to trigger Neighbor 911 alert for nearby CPR/AED responders
4. Neighbor 911 dispatches to nearby responders
5. Neighbor responder arrives in 2 minutes, starts CPR
6. EMS arrives 10 minutes later, takes over

**Why This Matters:**
- Doubles cardiac arrest survival rates (from ~10% to ~20%+)
- Dispatchers gain additional response capability
- Coordinates professional and neighbor response

### 4.2 Create Alert from 911 Dispatch Center (POST /api/v1/dispatch/alerts)

**Endpoint:**
```
POST https://us-central1-neighbor911.cloudfunctions.net/api/v1/dispatch/alerts
```

**Authentication:**
```
Authorization: Bearer dispatch_api_key_xyz123
X-Dispatch-Center-ID: sfpd-911-center
```

**Request Body:**
```json
{
  "dispatchSource": "911_dispatch",
  "cad_incident_id": "CAD-2025-001234",
  "dispatch_center": {
    "id": "sfpd-911-center",
    "name": "San Francisco Police Department - 911 Dispatch",
    "phone": "+14155531234",
    "dispatcher_name": "Officer Johnson"
  },
  "emergency": {
    "type": "cardiac_arrest",
    "priority": "critical",
    "caller_reported": "Male, 60s, unconscious, not breathing",
    "units_dispatched": ["Medic 12", "Engine 7"],
    "ems_eta": 720
  },
  "location": {
    "address": "123 Main St, Apt 4B, San Francisco, CA 94102",
    "lat": 37.7749,
    "lng": -122.4194,
    "cross_streets": "Main St & 1st Ave",
    "apartment_unit": "4B",
    "access_notes": "Front entrance, elevator to 4th floor"
  },
  "requested_capabilities": ["cpr", "aed"],
  "requested_responder_count": 2,
  "special_instructions": "Caller will unlock apartment door. Do not knock, just enter.",
  "notify_dispatcher_on": ["responder_accepted", "responder_arrived", "alert_resolved"]
}
```

**Response (Success):**
```json
{
  "success": true,
  "alertId": "neighbor911_dispatch_abc123",
  "status": "dispatching",
  "responders_found": 5,
  "estimated_first_arrival": 150,
  "dispatch_notification": {
    "sms": "+14155531234",
    "email": "dispatch@sfpd.gov",
    "webhook_url": "https://us-central1-neighbor911.cloudfunctions.net/webhooks/dispatch/abc123"
  },
  "message": "Alert sent to 5 nearby CPR/AED responders. Estimated first arrival: 2.5 minutes."
}
```

**Response (No Responders Available):**
```json
{
  "success": true,
  "alertId": "neighbor911_dispatch_abc123",
  "status": "no_responders_available",
  "responders_found": 0,
  "search_radius_miles": 1.0,
  "message": "No nearby responders with CPR/AED capability are currently available. Alert not dispatched.",
  "recommendation": "Continue with standard EMS response only."
}
```

### 4.3 Dispatcher Webhook Callbacks (Neighbor 911 → 911 Dispatch)

**When responder accepts:**
```json
{
  "event": "responder_accepted",
  "alertId": "neighbor911_dispatch_abc123",
  "cad_incident_id": "CAD-2025-001234",
  "timestamp": "2025-01-09T14:32:15Z",
  "responder": {
    "name": "Mike Smith",
    "phoneNumber": "+15551234567",
    "capabilities": ["cpr", "aed"],
    "equipment": ["AED"],
    "certification": {
      "cpr_certified_date": "2024-11-01",
      "aed_certified_date": "2024-11-01"
    },
    "eta_seconds": 120,
    "transport_method": "driving"
  },
  "location": {
    "responder_current_distance_miles": 0.3,
    "responder_current_lat": 37.7740,
    "responder_current_lng": -122.4200
  },
  "professional_ems": {
    "time_advantage_seconds": 600,
    "message": "Neighbor responder arriving ~10 minutes before EMS"
  }
}
```

**When responder arrives:**
```json
{
  "event": "responder_arrived",
  "alertId": "neighbor911_dispatch_abc123",
  "cad_incident_id": "CAD-2025-001234",
  "timestamp": "2025-01-09T14:34:30Z",
  "responder": {
    "name": "Mike Smith",
    "phoneNumber": "+15551234567"
  },
  "actual_response_time_seconds": 135,
  "on_scene_status": "Performing CPR with AED",
  "message": "Neighbor responder on scene. Professional EMS still needed."
}
```

**When emergency resolved:**
```json
{
  "event": "alert_resolved",
  "alertId": "neighbor911_dispatch_abc123",
  "cad_incident_id": "CAD-2025-001234",
  "timestamp": "2025-01-09T14:47:00Z",
  "resolution": {
    "resolved_by": "professional_ems",
    "outcome": "patient_transported",
    "neighbor_response_time": 135,
    "ems_arrival_time": 720,
    "time_advantage": 585,
    "neighbor_actions": "CPR + AED shock delivered, pulse restored"
  },
  "message": "Emergency resolved. EMS transported patient to hospital. Neighbor response provided critical life-saving intervention."
}
```

### 4.4 Dispatcher Query Available Responders (GET /api/v1/dispatch/responders)

**Use Case:** Dispatcher wants to check responder availability before creating alert

**Endpoint:**
```
GET https://us-central1-neighbor911.cloudfunctions.net/api/v1/dispatch/responders
```

**Query Parameters:**
```
?lat=37.7749
&lng=-122.4194
&capabilities=cpr,aed
&radius_miles=0.5
```

**Response:**
```json
{
  "success": true,
  "location": {
    "lat": 37.7749,
    "lng": -122.4194,
    "search_radius_miles": 0.5
  },
  "responders_available": 5,
  "responders": [
    {
      "distance_miles": 0.2,
      "eta_seconds": 120,
      "capabilities": ["cpr", "aed"],
      "equipment": ["AED"],
      "last_response": "2024-12-15",
      "certification_status": "valid"
    },
    {
      "distance_miles": 0.3,
      "eta_seconds": 180,
      "capabilities": ["cpr", "aed", "naloxone"],
      "equipment": ["AED", "Naloxone"],
      "last_response": "2025-01-02",
      "certification_status": "valid"
    }
  ],
  "message": "5 CPR/AED responders available within 0.5 miles",
  "estimated_first_arrival": 120
}
```

**Note:** No personally identifiable information (names, phone numbers) returned until alert is dispatched

### 4.5 API Authentication for Dispatch Centers

**API Key Management:**
```typescript
// Admin creates API key for dispatch center
const dispatchApiKey = await createDispatchApiKey({
  dispatchCenterId: 'sfpd-911-center',
  name: 'San Francisco Police Department - 911 Dispatch',
  permissions: ['create_alerts', 'query_responders', 'receive_webhooks'],
  ipWhitelist: ['192.168.1.1', '203.0.113.0/24'],
  expiresAt: null  // No expiration for government agencies
});

// Returns: dispatch_api_key_abc123...
```

**Security Considerations:**
- ✅ API keys are **IP-whitelisted** (only dispatch center IPs can use them)
- ✅ Keys are **scoped** to specific dispatch centers (can only create alerts in their jurisdiction)
- ✅ All requests **logged** for audit trail
- ✅ **Rate limiting** applied per dispatch center (1000 alerts/hour max)

### 4.6 CAD System Integration (Phase 2+)

**Direct integration with major CAD vendors:**
- **Hexagon** (Intergraph CAD)
- **Motorola** (PremierOne CAD)
- **CentralSquare** (TriTech Inform CAD)
- **Tyler Technologies** (New World CAD)

**Integration Method:**
- Neighbor 911 provides **CAD plugin/module**
- Dispatcher clicks button in CAD interface: "Request Neighbor Response"
- CAD system sends API call to Neighbor 911
- Real-time status updates sent back to CAD

**Example CAD Integration:**
```javascript
// CAD system plugin (JavaScript)
function requestNeighborResponse(incident) {
  const response = await fetch(
    'https://neighbor911.app/api/v1/dispatch/alerts',
    {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${CAD_API_KEY}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        cad_incident_id: incident.id,
        emergency: {
          type: mapIncidentTypeToNeighbor911(incident.type),
          priority: incident.priority
        },
        location: {
          address: incident.address,
          lat: incident.lat,
          lng: incident.lng
        },
        requested_capabilities: determineRequiredCapabilities(incident.type),
        ems_eta: calculateEMSEta(incident)
      })
    }
  );

  const data = await response.json();

  // Update CAD incident with neighbor response status
  incident.addNote(`Neighbor 911: ${data.responders_found} responders dispatched. ETA: ${data.estimated_first_arrival}s`);
}
```

### 4.7 Dispatcher Training & Coordination

**When to use Neighbor 911 (Dispatcher Guidelines):**

✅ **DO request neighbor response for:**
- Cardiac arrest (CPR/AED needed)
- Overdose (naloxone available nearby)
- Fire (neighbors can evacuate building, use extinguishers)
- Wellness check (officer is 20+ min away)
- Missing child (local search party)

❌ **DON'T request neighbor response for:**
- Active shooter / violent crime
- Hazmat / unsafe scenes
- Anything requiring law enforcement authority
- Situations where bystanders would be at risk

**Dispatcher Script:**
```
"I'm dispatching professional EMS to your location.
They will arrive in approximately 12 minutes.

I'm also sending a Neighbor 911 alert to nearby trained
CPR responders who may arrive sooner. If someone knocks
on your door in the next few minutes, they're a neighbor
here to help until EMS arrives."
```

---

## 5. Neighbor 911 → 911 Dispatch Systems

### 5.1 MVP: Email/SMS Notification

**When responder accepts life-threatening alert, send email to dispatch:**

**Email Template:**
```
Subject: [NEIGHBOR RESPONSE] Cardiac Arrest - 123 Main St

NEIGHBOR RESPONSE IN PROGRESS

Emergency Type: Cardiac Arrest
Address: 123 Main St, Apt 4B, San Francisco, CA 94102
Coordinates: 37.7749° N, 122.4194° W

Responder: Mike Smith
Phone: (555) 123-4567
Equipment: AED, CPR certified
Certification Date: 2024-11-01
ETA: 2 minutes

Professional EMS still needed.
Responder will provide aid until EMS arrives.

Alert ID: neighbor911_abc123
Timestamp: 2025-01-09 14:30:00 PST
```

**SMS Template (Shortened):**
```
[NEIGHBOR911] Cardiac arrest at 123 Main St Apt 4B.
Mike Smith (555-123-4567) en route with AED, ETA 2 min.
EMS still needed. ID: abc123
```

**Implementation:**
```typescript
import * as nodemailer from 'nodemailer';
import * as twilio from 'twilio';

async function notify911Dispatch(alert: Alert, responder: Responder) {
  const dispatchEmail = getDispatchEmailForZipCode(alert.location.zipCode);
  const dispatchSMS = getDispatchSMSForZipCode(alert.location.zipCode);

  // Send email
  await sendEmail({
    to: dispatchEmail,
    subject: `[NEIGHBOR RESPONSE] ${alert.type} - ${alert.location.address}`,
    body: generateEmailBody(alert, responder)
  });

  // Send SMS
  await sendSMS({
    to: dispatchSMS,
    body: generateSMSBody(alert, responder)
  });
}
```

### 5.2 Phase 2: CAD System API Integration

**POST to 911 dispatch CAD system:**

```
POST https://dispatch.example.gov/api/v1/neighbor-response
Authorization: Bearer govt_api_key_xyz
Content-Type: application/json
```

**Request Body:**
```json
{
  "incidentType": "cardiac_arrest",
  "location": {
    "address": "123 Main St, Apt 4B",
    "city": "San Francisco",
    "state": "CA",
    "zipCode": "94102",
    "coordinates": {
      "lat": 37.7749,
      "lng": -122.4194
    }
  },
  "neighborResponder": {
    "name": "Mike Smith",
    "phoneNumber": "+15551234567",
    "equipment": ["AED", "CPR"],
    "certificationDate": "2024-11-01",
    "eta": 120
  },
  "alertId": "neighbor911_abc123",
  "timestamp": "2025-01-09T14:30:00Z",
  "emsStillRequired": true
}
```

**Standard:** NENA i3 format (future compatibility)

---

## 5. Neighbor 911 → Civic Data APIs

### 5.1 Vote.org API (Voter Registration)

**Check voter registration status:**

```typescript
import axios from 'axios';

async function checkVoterRegistration(
  firstName: string,
  lastName: string,
  zipCode: string,
  dateOfBirth: string
): Promise<VoterRegistrationStatus> {
  const response = await axios.get('https://api.vote.org/v1/voter-registration/status', {
    headers: {
      'Authorization': `Bearer ${VOTE_ORG_API_KEY}`
    },
    params: {
      first_name: firstName,
      last_name: lastName,
      zip_code: zipCode,
      date_of_birth: dateOfBirth
    }
  });

  return {
    isRegistered: response.data.is_registered,
    state: response.data.state,
    registrationUrl: response.data.registration_url
  };
}
```

### 5.2 Google Civic Information API (Polling Locations)

**Get polling place for user's address:**

```typescript
async function getPollingPlace(address: string): Promise<PollingLocation> {
  const response = await axios.get(
    'https://www.googleapis.com/civicinfo/v2/voterinfo',
    {
      params: {
        key: GOOGLE_CIVIC_API_KEY,
        address: address,
        electionId: '2000'  // Get latest election ID dynamically
      }
    }
  );

  return {
    name: response.data.pollingLocations[0].address.locationName,
    address: response.data.pollingLocations[0].address.line1,
    city: response.data.pollingLocations[0].address.city,
    hours: response.data.pollingLocations[0].pollingHours
  };
}
```

---

## 6. Internal Mobile ↔ Firebase API

### 6.1 Firebase SDK (Primary Method)

**Most internal communication uses Firebase SDKs directly:**

```dart
// Create alert (mobile app → Firestore)
final alertRef = await FirebaseFirestore.instance.collection('alerts').add({
  'type': 'cardiac_arrest',
  'status': 'created',
  'originator': {
    'userId': currentUser.uid,
    'name': currentUser.displayName
  },
  'location': {
    'lat': position.latitude,
    'lng': position.longitude
  },
  'timestamps': {
    'created': FieldValue.serverTimestamp()
  }
});

// Watch alert status (Firestore → mobile app)
FirebaseFirestore.instance
  .collection('alerts')
  .doc(alertRef.id)
  .snapshots()
  .listen((snapshot) {
    final alert = Alert.fromFirestore(snapshot);
    // Update UI with real-time status
  });
```

### 6.2 Cloud Functions (For Complex Logic)

**When direct Firestore access isn't sufficient:**

```dart
// Call Cloud Function to calculate ETA
final callable = FirebaseFunctions.instance.httpsCallable('calculateETA');
final result = await callable.call({
  'responderLocation': {'lat': 40.7, 'lng': -74.0},
  'alertLocation': {'lat': 40.75, 'lng': -74.01},
  'transportMethod': 'driving'
});

final eta = result.data['eta'];  // ETA in seconds
```

**Cloud Function:**
```typescript
export const calculateETA = functions.https.onCall(async (data, context) => {
  const { responderLocation, alertLocation, transportMethod } = data;

  // Use Google Maps Directions API
  const eta = await getETA(responderLocation, alertLocation, transportMethod);

  return { eta };
});
```

---

## 7. Webhooks & Callbacks

### 7.1 Webhook Security

**All webhooks include HMAC signature for verification:**

```typescript
import * as crypto from 'crypto';

function generateWebhookSignature(payload: any, secret: string): string {
  const payloadString = JSON.stringify(payload);
  return crypto
    .createHmac('sha256', secret)
    .update(payloadString)
    .digest('hex');
}

// Include signature in webhook request
const signature = generateWebhookSignature(payload, SAFEWORD_WEBHOOK_SECRET);

await axios.post(webhookUrl, payload, {
  headers: {
    'X-Neighbor911-Signature': signature,
    'Content-Type': 'application/json'
  }
});
```

**Safeword verifies signature:**
```typescript
function verifyWebhookSignature(
  payload: any,
  signature: string,
  secret: string
): boolean {
  const expectedSignature = generateWebhookSignature(payload, secret);
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}
```

### 7.2 Webhook Retry Logic

**If webhook fails, retry with exponential backoff:**

```typescript
async function sendWebhookWithRetry(
  url: string,
  payload: any,
  maxRetries: number = 3
) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      const response = await axios.post(url, payload, { timeout: 5000 });
      if (response.status === 200) return;
    } catch (error) {
      if (attempt === maxRetries - 1) {
        // Log failure, alert admin
        console.error(`Webhook failed after ${maxRetries} attempts`);
        await logWebhookFailure(url, payload, error);
      }
      // Exponential backoff: 1s, 2s, 4s
      await sleep(Math.pow(2, attempt) * 1000);
    }
  }
}
```

---

## 8. Rate Limiting & Abuse Prevention

### 8.1 API Rate Limits

**Per API Key (e.g., Safeword):**
- **Alerts:** 100 alerts per minute, 10,000 per day
- **Webhooks:** 1,000 requests per minute

**Per User (Mobile App):**
- **Alert creation:** 3 alerts per day (default), 1 alert per hour
- **Capability updates:** 10 per hour

### 8.2 Implementation (Firestore-based Rate Limiting)

```typescript
async function checkRateLimit(userId: string, action: string): Promise<boolean> {
  const rateLimitRef = firestore.collection('rateLimits').doc(`${userId}_${action}`);
  const doc = await rateLimitRef.get();

  const now = Date.now();
  const windowMs = 60 * 60 * 1000;  // 1 hour
  const maxRequests = 3;

  if (!doc.exists) {
    await rateLimitRef.set({
      count: 1,
      windowStart: now
    });
    return true;
  }

  const data = doc.data()!;
  const windowStart = data.windowStart;

  if (now - windowStart > windowMs) {
    // Reset window
    await rateLimitRef.set({
      count: 1,
      windowStart: now
    });
    return true;
  }

  if (data.count >= maxRequests) {
    return false;  // Rate limit exceeded
  }

  await rateLimitRef.update({
    count: admin.firestore.FieldValue.increment(1)
  });
  return true;
}
```

---

## 9. Error Handling

### 9.1 Standard Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "INVALID_LOCATION",
    "message": "Location coordinates are required",
    "field": "location.lat",
    "details": {
      "provided": null,
      "expected": "number (latitude)"
    }
  },
  "timestamp": "2025-01-09T14:30:00Z",
  "requestId": "req_abc123"
}
```

### 9.2 Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_API_KEY` | 401 | Invalid or missing API key |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `INVALID_LOCATION` | 400 | Missing or invalid location data |
| `NO_RESPONDERS_AVAILABLE` | 200 | Alert created but no responders found (not an error) |
| `INTERNAL_ERROR` | 500 | Server error |
| `UNSUPPORTED_ALERT_TYPE` | 400 | Alert type not recognized |
| `INVALID_PHONE_NUMBER` | 400 | Phone number format invalid |

---

## 10. API Versioning Strategy

### 10.1 URL Versioning

**All API endpoints include version in URL:**
```
https://us-central1-neighbor911.cloudfunctions.net/api/v1/alerts
                                                      ^^^
                                                    version
```

**Version Policy:**
- `v1` - Current stable version (MVP)
- `v2` - Future breaking changes (when needed)
- Old versions deprecated with 6-month notice

### 10.2 Backward Compatibility

**Non-breaking changes (safe to add to v1):**
- ✅ Add new optional fields
- ✅ Add new endpoints
- ✅ Add new enum values

**Breaking changes (require v2):**
- ❌ Remove fields
- ❌ Rename fields
- ❌ Change field types
- ❌ Change authentication method

---

## Next Steps

1. ✅ Review this API spec with backend engineer
2. ✅ Coordinate with Safeword team on webhook contract
3. ✅ Contact local 911 dispatch centers for email/SMS preferences
4. ✅ Implement API endpoints in Cloud Functions
5. ✅ Write integration tests for all API flows
6. ✅ Create API documentation site (Postman or Swagger)

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | 2025-01-09 | AI-generated (Claude) | Initial API specification |

---

**This API specification is a living document.** Feedback from integration partners and security reviews will shape the final implementation. Open issues on GitHub or email team@lifesaverlabs.org.
