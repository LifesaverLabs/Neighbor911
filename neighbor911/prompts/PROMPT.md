# Neighbor 911 - AI Development Prompts

**Purpose:** Common prompts and context for AI tools (Claude, ChatGPT, Copilot, etc.) to help contributors align with project principles and architecture.

**Last Updated:** 2025-11-10
**Organization:** Lifesaver Labs Public Benefit Corporation

---

## ⚠️ CRITICAL: LIFE AND DEATH STAKES

**THIS IS NOT A TYPICAL SOFTWARE PROJECT. POOR EXECUTION KILLS PEOPLE.**

### The Responsibility We Bear

Every feature we build, every line of code we ship, every architectural decision we make has the potential to **save or end a human life**. This is not hyperbole—this is the reality of emergency response technology.

**We can kill someone by:**
- Launching critical alerts irresponsibly before they're validated
- Giving people false confidence that delays their 911 call
- Creating privacy vulnerabilities that expose abuse victims
- Building unreliable software that fails during emergencies
- Poor UX that confuses users in high-stress life-or-death moments

**We can also kill someone by:**
- **Failing to launch** when people need us (analysis paralysis kills)
- Over-cautious delays when Good Samaritan protections apply
- Letting perfect be the enemy of good in lifesaving features
- Waiting for "exhaustive validation" while people die from slow EMS response

### Launch Maturity: A Balanced Approach

**The Core Tension:**
- ⚠️ **Launch too early** → We might harm someone through buggy, unvalidated software
- ⚠️ **Launch too late** → We definitely harm people by withholding lifesaving tools

**Our Strategy:**

1. **For CRITICAL Alerts (CPR, AED, overdose, DV, consent emergency, wellness check):**
   - **Ideal path:** Validate with PSAP partnerships before launch
   - **Acceptable path:** Launch with Good Samaritan law protections if:
     - PSAPs are dragging feet unreasonably (giving them "industry veto" on innovation)
     - Software is battle-tested and reasonably stable
     - Legal review confirms Good Samaritan protections apply
     - We've gained community trust through non-critical alerts first
   - **Unacceptable path:** Cowboy launch without validation OR without legal protections

2. **For NON-CRITICAL Alerts (quit companion, sport dissuasion, loneliness):**
   - Launch faster to build trust and test infrastructure
   - Lower stakes = more room for iteration and learning

3. **PSAP Integration (911 Dispatch ↔ Neighbor 911):**
   - **Stage 1 (MVP - Months 1-3):** "Call 911 first" redirect for critical emergencies
   - **Stage 2 (Partnership - Months 3-6):** 3-way calling with 911 primary, test with pilot PSAPs
   - **Stage 3 (Full Integration - Months 6-12):** Seamless handoff in test communities, expand after validation

   **Note:** These timelines depend entirely on the **size and commitment of our open source community**. The limiting reagent is not just engineers—it's the collective strength of our **institutions, designers, project and product managers, healthcare experts, security professionals, leaders, advisors, writers, minutewomen and minutemen volunteers, grant writers, advocates, ethicists, proponents, community teachers, grandparents and parents, student/grandstudent calmunity, STEAM professionals, tradespeople, voters, and supporters**.

   But above all perhaps, **the limiting reagent is the rate and depth of our conversations with each other** about privacy, permissions, security, safety, liberty, freedom, features, tradeoffs, choice, acceptance, resistance, protocol, alternatives, standardization, community storytelling, architecture, the right balance between community nudging/noodging/nagging, and more. Finding service-market and product-market fit for lifesaving technology requires **thousands and tens of thousands of meaningful conversations**—with PSAPs, calmunity members, responders, legal experts, ethicists, and each other. These conversations cannot be rushed, yet they cannot be delayed. The more contributors who engage in these critical dialogues, the faster we can validate and deploy variations experimentally under conscientious quality review to calmunities (eg communities) locally, statewide, nationwide and globally.

### Decision Framework for Contributors

When deciding whether to launch or delay a feature:

**Ask yourself:**
1. ✅ **Is this reasonably safe?** (Not "perfectly safe"—that's impossible)
2. ✅ **Have we done due diligence?** (Legal review, PSAP consultation, testing)
3. ✅ **Are we protected?** (Good Samaritan laws, PSAP partnership, or low-risk category)
4. ✅ **Is delay causing MORE harm?** (People dying from slow EMS while we wait for perfection)

**Red flags for "launch too early":**
- No legal review of liability
- No testing with real users in stress scenarios
- Critical bugs in emergency dispatch logic
- Privacy vulnerabilities that expose victims
- No PSAP consultation AND no Good Samaritan protection

**Red flags for "launch too late":**
- Waiting for "exhaustive" validation that will never come
- PSAPs giving unreasonable runaround (industry veto)
- Perfectly working software sitting on shelf while people die
- Analysis paralysis driven by fear rather than evidence

### Supreme Care, Not Paralysis

**We must:**
- 🚨 **Be supremely careful** in our launch decisions (lives depend on it)
- 🚀 **Be supremely urgent** in our development speed (lives depend on it)
- ⚖️ **Balance both** through rigorous but rapid validation

**Guiding Philosophy:**
> "First, do no harm. Second, do not fail to help when you can."

When in doubt, consult with:
- Legal counsel (liability, Good Samaritan protections)
- PSAP partners (professional emergency responders)
- Community members (real users with real emergencies)
- Lifesaver Labs leadership (team@lifesaverlabs.org)

### Remember

**People are dying right now** from slow EMS response. We can help them. But we must do so **responsibly, carefully, and with urgency**—not recklessly, not timidly.

---

## How to Use This Document

When working on Neighbor 911 with AI assistance:

1. **Load this prompt** at the start of your AI session
2. **Reference specific sections** for context on architecture decisions
3. **Follow the principles** outlined here for consistency
4. **Update this document** if you discover better patterns

---

## Project Overview

**Neighbor 911** is a rapid-response mobile application that connects neighbors in emergencies, enabling 2-3 minute response times vs. 8-15 minute professional EMS response.

**Core Mission:** Save lives by coordinating trained neighbors with life-saving equipment (AEDs, naloxone, CPR skills) to arrive before professional EMS.

**Legal Entity:** Lifesaver Labs Public Benefit Corporation
**License:** MIT License (open source code) + Trademark protection (Neighbor 911™)
**Repository:** https://github.com/LifesaverLabs/Neighbor911

---

## Fundamental Principles

### 1. **Lives Saved > Features**
- Prioritize features that directly save lives (CPR/AED dispatch, overdose response)
- Defer nice-to-have features (analytics, gamification, social features)
- Every feature should answer: "Does this help someone faster or better?"

### 2. **PSAP Primacy (911 is Primary)**
- Neighbor 911 **supplements, not replaces** professional 911 services
- For life-threatening emergencies: **911 MUST be called first**
- We coordinate faster neighbor response WHILE EMS is en route
- Never claim to replace or compete with professional emergency services

### 3. **Privacy-First Architecture**
- **Never store long-term location data** (GPS queried in real-time only during emergencies)
- **Minimal data collection** (only what's needed for emergency response)
- **GDPR/CCPA compliant** from day one
- **Encryption at rest and in transit** for all sensitive data
- **User can delete all data** (right to erasure)

### 4. **Responder Burnout Prevention**
- **Weekly limits on non-critical alerts** (default: 5 per week for quit companion, sport dissuasion, etc.)
- **Users set their own limits** during capability selection
- **Critical alerts never count toward limits** (cardiac arrest, DV, consent emergency, wellness check)
- **Proactive check-ins** when responders receive too many alerts
- **No guilt for declining** alerts or taking breaks

### 5. **Inclusive & Accessible**
- **Ages 13+** can be responders (with parent consent for minors)
- **Ages 80+** can receive-only (opt out of inbound alerts but can still call for help)
- **Full internationalization** from day one (10+ languages at launch)
- **Accessibility compliance** (WCAG 2.1 AA)
- **Low-bandwidth friendly** (works on 3G)

### 6. **Community-Driven, Not Top-Down**
- **Volunteers, not employees** as responders
- **Peer-to-peer mutual aid** philosophy
- **No mandatory training for basic capabilities** (honor system for MVP)
- **Training encouraged but not gated** (except for advanced capabilities like firearms response)

### 7. **De-escalation First, Force Last**
- Armed response is **rare exception, not the rule**
- **Practice spray preferred** over pepper spray (inert deterrent)
- **Firearms response requires extensive training** and explicit authorization
- **Legal witness mode** for neutral documentation without confrontation

---

## Architecture Principles

### Technology Stack (Agreed Upon)

**Mobile:**
- Framework: **Flutter 3.19+** (cross-platform iOS/Android)
- State Management: **Riverpod** (recommended) or Bloc
- Navigation: **GoRouter**
- Target: iOS 14+, Android 9+ (API 28+)

**Backend:**
- **Firebase** (BaaS approach)
  - Authentication: Firebase Auth (phone verification)
  - Database: Cloud Firestore (real-time NoSQL)
  - Notifications: Firebase Cloud Messaging (FCM)
  - Storage: Cloud Storage for Firebase
  - Functions: Cloud Functions (Node.js/TypeScript)

**Geospatial:**
- **GeoFlutterFire Plus** for proximity queries
- Privacy-preserving: location queried only during active availability

**Maps:**
- Google Maps (MVP)
- OpenStreetMap fallback (future)

**Internationalization:**
- Flutter i18n with ARB files
- 10+ languages at launch: EN, ES, FR, ZH, AR, PT, RU, HI, DE, JA

### Key Architectural Decisions

#### 1. **Privacy-Preserving Location**

```typescript
// ❌ DON'T: Store exact home location in user profile
interface User {
  homeLocation: { lat: 40.748817, lng: -73.985428 }  // WRONG
}

// ✅ DO: Query location only when alert created
async function createAlert() {
  const position = await Geolocator.getCurrentPosition();

  // Store ONLY for this specific alert, NOT in user profile
  await firestore.collection('alerts').add({
    location: {
      lat: position.latitude,
      lng: position.longitude,
      geohash: calculateGeohash(position)
    }
  });
}

// ✅ DO: Store only city/state in user profile
interface User {
  address: {
    city: "San Francisco",
    state: "CA",
    zipCode: "94102"
    // NO lat/lng stored here
  }
}
```

#### 2. **Alert Categorization for Burnout Prevention**

```typescript
type CriticalityLevel =
  | 'critical'       // Life-threatening: NEVER count toward weekly limit
                     // (cardiac arrest, overdose, fire, drowning)

  | 'security'       // Safety/security: NEVER count toward limit
                     // (consent emergency, DV, SA, missing child abduction)

  | 'urgent'         // Time-sensitive critical: NEVER count toward limit
                     // (wellness check - potential suicide/medical)

  | 'non_critical';  // Support/preventive: COUNT toward 5/week limit
                     // (quit companion, companionship, sport dissuasion)
```

#### 3. **PSAP Handoff Maturity Stages**

```
Stage 1 (MVP - Weeks 1-16):
  - AI voice interface ONLY for non-critical emergencies
  - Critical emergencies → "Call 911 first, we'll coordinate neighbors"
  - No call handoff, no 3-way calling

Stage 2 (PSAP Partnership - Months 4-12):
  - 3-way calling: User + AI + 911
  - 911 takes primary control
  - AI coordinates neighbor dispatch in background
  - Silent data channel for location/responder status

Stage 3 (Full Integration - Year 2+):
  - E911 features work through Neighbor 911
  - Seamless handoff, lossless information relay
  - PSAP can preempt/takeover calls
  - Battle-tested and authorized by mobile OS
```

#### 4. **Real-Time Updates via Firestore Streams**

```dart
// ✅ Use Firestore snapshots for real-time status
Stream<Alert> watchAlertStatus(String alertId) {
  return FirebaseFirestore.instance
    .collection('alerts')
    .doc(alertId)
    .snapshots()
    .map((snapshot) => Alert.fromFirestore(snapshot));
}

// Originator's UI updates automatically when responder accepts
```

#### 5. **Geospatial Index Lifecycle (Privacy)**

```
User toggles "Available Now" ON:
  → Get current GPS location
  → Create document in /geospatial/{userId} with geohash
  → Document exists ONLY while user is available

User toggles "Available Now" OFF:
  → Delete document from /geospatial/{userId}
  → User's location now completely unknown to system

Cloud Function cleanup (daily):
  → Delete stale geospatial docs (lastUpdated > 24 hours ago)
```

---

## Code Style & Conventions

### Dart/Flutter Conventions

**File naming:**
```
snake_case.dart          ✅ Good
PascalCase.dart          ❌ Avoid
camelCase.dart           ❌ Avoid
```

**Class naming:**
```dart
class AlertModel { }     ✅ Good (PascalCase)
class alert_model { }    ❌ Bad
```

**Variable naming:**
```dart
final alertId = '...';   ✅ Good (camelCase)
final AlertId = '...';   ❌ Bad
final alert_id = '...';  ❌ Bad
```

**Folder structure:**
```
lib/
├── core/               # Constants, themes, utils
├── data/               # Models, repositories, services
├── domain/             # Entities, use cases, providers
├── presentation/       # Screens, widgets, navigation
└── l10n/              # Internationalization (ARB files)
```

### TypeScript (Cloud Functions) Conventions

**File naming:**
```
camelCase.ts            ✅ Good
PascalCase.ts           ❌ Avoid
snake_case.ts           ❌ Avoid
```

**Function naming:**
```typescript
export const onAlertCreated = ...    ✅ Good (camelCase)
export const OnAlertCreated = ...    ❌ Bad
export const on_alert_created = ...  ❌ Bad
```

### Internationalization (i18n) Patterns

**Always externalize user-facing strings:**

```dart
// ❌ Bad: Hardcoded string
Text('Cardiac Arrest')

// ✅ Good: Localized string
Text(AppLocalizations.of(context)!.alertCardiacArrest)
```

**ARB file structure:**
```json
{
  "@@locale": "en",

  "alertCardiacArrest": "Cardiac Arrest",
  "@alertCardiacArrest": {
    "description": "Emergency type: Cardiac arrest"
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
  }
}
```

---

## Common AI Prompts for Development Tasks

### Prompt 1: Creating a New Feature

```
I'm working on Neighbor 911, an emergency response app that coordinates
neighbors to arrive faster than EMS. Please help me implement [FEATURE].

Context:
- We use Flutter + Firebase
- State management: Riverpod
- Privacy-first: never store long-term location
- PSAP primacy: 911 is always primary for critical emergencies

Key principles to follow:
1. Lives saved > features
2. Privacy-preserving (minimal data collection)
3. Responder burnout prevention (weekly limits on non-critical alerts)
4. Full internationalization (externalize all strings to ARB files)
5. Accessibility (WCAG 2.1 AA)

Please ensure:
- All user-facing strings are externalized to l10n ARB files
- Location data is NOT stored in user profiles (query at alert creation only)
- Code follows Dart/Flutter conventions (snake_case files, camelCase vars)
- Real-time updates use Firestore snapshots
- Error handling for offline scenarios

Reference docs:
- Architecture: docs/ARCHITECTURE.md
- Data model: docs/DATA_MODEL.md
- User stories: docs/USER_STORIES.md
```

### Prompt 2: Reviewing Code for Privacy Compliance

```
Please review this code for privacy compliance in Neighbor 911.

Critical rules:
1. ❌ NEVER store exact lat/lng in user profile (only city/state/zip)
2. ❌ NEVER retain location data after alert resolved
3. ✅ Query location only when user toggles "Available Now" ON
4. ✅ Delete geospatial index when user toggles "Available Now" OFF
5. ✅ Encrypt sensitive data at rest
6. ✅ User must be able to delete all their data

Check for:
- Long-term location storage violations
- PII in logs or analytics
- Missing encryption for sensitive fields
- Lack of data deletion endpoints
- GDPR/CCPA compliance issues

Code to review:
[PASTE CODE]
```

### Prompt 3: Adding Internationalization

```
I need to add i18n support to this Neighbor 911 feature.

Requirements:
- Use Flutter's intl package + ARB files
- Support 10 languages: EN, ES, FR, ZH, AR, PT, RU, HI, DE, JA
- Externalize ALL user-facing strings
- Include date/time/number formatting
- Provide clear ARB descriptions for translators

Current code:
[PASTE CODE]

Please:
1. Extract hardcoded strings to ARB file (lib/l10n/app_en.arb)
2. Update code to use AppLocalizations
3. Show example for 2-3 languages (English + Spanish + one other)
4. Handle plurals and parameters correctly
```

### Prompt 4: Implementing Real-Time Updates

```
I need to implement real-time status updates for Neighbor 911 alerts.

Requirements:
- Use Firestore snapshots (not polling)
- Update originator UI when responder accepts/arrives/resolves
- Handle connection loss gracefully
- Minimize Firestore reads (use snapshots efficiently)
- Update Riverpod state when Firestore changes

Architecture:
- Repository pattern for data access
- Riverpod StreamProvider for reactive state
- UI consumes streams via ref.watch()

Please implement:
1. Repository method that returns Stream<Alert>
2. Riverpod StreamProvider
3. UI widget that displays real-time status
4. Error handling for offline/timeout

Context:
[DESCRIBE FEATURE]
```

### Prompt 5: Security Review

```
Please perform a security review of this Neighbor 911 code.

Focus areas:
1. Authentication: Are Firebase Auth tokens validated properly?
2. Authorization: Can users only access their own data?
3. Input validation: Are user inputs sanitized?
4. Rate limiting: Can users spam alerts?
5. Injection attacks: SQL/NoSQL injection risks?
6. API security: Are API keys properly protected?

Firestore Security Rules to validate against:
- Users can only read/write their own profile
- Alerts: Originators can create, responders can update
- Geospatial index: Users can only update their own location
- Training content: Read-only for all users

Code to review:
[PASTE CODE]
```

---

## Testing Guidelines

### Unit Tests (75% of tests)

Focus on:
- Data models (serialization/deserialization)
- Business logic (use cases, triage algorithms)
- Utility functions (validators, formatters)

```dart
// Example: Test alert model
test('AlertModel.fromJson creates valid model', () {
  final json = {
    'type': 'cardiac_arrest',
    'status': 'created',
    'location': {'lat': 40.7, 'lng': -74.0}
  };

  final alert = AlertModel.fromJson(json);

  expect(alert.type, AlertType.cardiacArrest);
  expect(alert.status, AlertStatus.created);
});
```

### Widget Tests (20% of tests)

Focus on:
- UI component behavior
- User interactions
- State changes

```dart
testWidgets('AlertCard displays correctly', (tester) async {
  final alert = AlertModel(
    type: AlertType.cardiacArrest,
    distance: 0.3
  );

  await tester.pumpWidget(
    MaterialApp(
      home: Scaffold(body: AlertCard(alert: alert))
    )
  );

  expect(find.text('Cardiac Arrest'), findsOneWidget);
  expect(find.text('0.3 miles away'), findsOneWidget);
});
```

### Integration Tests (5% of tests)

Focus on:
- Critical user flows end-to-end
- Create alert → Dispatch → Accept → Resolve
- Phone auth → Profile setup → Capability selection

---

## Documentation Standards

### Code Comments

**When to comment:**
- ✅ Why something is done (not what)
- ✅ Complex algorithms that aren't obvious
- ✅ Privacy/security considerations
- ✅ Workarounds for known issues

**When NOT to comment:**
- ❌ Obvious code that explains itself
- ❌ Commented-out code (delete it, use git history)

**Examples:**

```dart
// ❌ Bad: States the obvious
// Create a new alert
final alert = Alert(type: 'cardiac_arrest');

// ✅ Good: Explains why
// We must call 911 first for critical emergencies to ensure
// E911 location features work and professional EMS is dispatched
if (alert.isCritical) {
  redirectTo911First();
}

// ✅ Good: Privacy consideration
// Location is NOT stored in user profile to protect privacy.
// We only query GPS when user toggles "Available Now" ON.
final position = await Geolocator.getCurrentPosition();
```

### Documentation Files

All major features should have:
- User story in `docs/USER_STORIES.md`
- Architecture notes in `docs/ARCHITECTURE.md`
- API contracts in `docs/API_SPECIFICATION.md`
- Privacy impact in `docs/DATA_MODEL.md`

---

## Git Commit Message Format

```
[JIRA-TASK] Short summary (50 chars or less)

Longer description of what changed and why (wrap at 72 chars).

Key changes:
- Bullet point 1
- Bullet point 2
- Bullet point 3

Technical details:
- Implementation notes
- Performance impact
- Security considerations

🚨 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Example:**
```
[N9-4] Add technical architecture documentation

Created comprehensive technical architecture and design docs:
- ARCHITECTURE.md: Flutter stack, Firebase backend, i18n strategy
- DATA_MODEL.md: Privacy-preserving location, GDPR compliance
- API_SPECIFICATION.md: Safeword integration, 911 dispatch APIs

Key decisions:
- Flutter + Firebase for rapid MVP development
- Riverpod for state management
- GeoFlutterFire for proximity queries
- Privacy-first: no long-term location storage

🚨 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

---

## Common Pitfalls to Avoid

### ❌ Pitfall 1: Storing Long-Term Location
```dart
// ❌ WRONG: Storing exact home location
await firestore.collection('users').doc(userId).update({
  'homeLocation': GeoPoint(lat, lng)  // PRIVACY VIOLATION
});
```

### ❌ Pitfall 2: Not Externalizing Strings
```dart
// ❌ WRONG: Hardcoded English string
Text('Alert sent to 5 nearby responders')

// ✅ RIGHT: Localized string
Text(l10n.alertSentToResponders(5))
```

### ❌ Pitfall 3: Claiming to Replace 911
```dart
// ❌ WRONG: Marketing copy that competes with 911
"Skip the 911 wait - get help faster with Neighbor 911!"

// ✅ RIGHT: Supplement, don't replace
"Get help from neighbors while professional EMS is en route"
```

### ❌ Pitfall 4: No Burnout Prevention
```dart
// ❌ WRONG: Unlimited alerts to all responders
sendAlertToAllResponders(alert);

// ✅ RIGHT: Check weekly limits for non-critical alerts
if (alert.isCritical || userHasCapacity(responder, alert.type)) {
  sendAlert(responder, alert);
}
```

### ❌ Pitfall 5: Blocking UI During Firestore Calls
```dart
// ❌ WRONG: Blocking UI
final alert = await firestore.collection('alerts').doc(id).get();
setState(() { this.alert = Alert.fromFirestore(alert); });

// ✅ RIGHT: Reactive streams
final alertStream = firestore.collection('alerts').doc(id).snapshots();
// UI updates automatically via Riverpod StreamProvider
```

---

## Resources & References

### Documentation
- **Architecture:** [docs/ARCHITECTURE.md](../docs/ARCHITECTURE.md)
- **Data Model:** [docs/DATA_MODEL.md](../docs/DATA_MODEL.md)
- **API Spec:** [docs/API_SPECIFICATION.md](../docs/API_SPECIFICATION.md)
- **User Stories:** [docs/USER_STORIES.md](../docs/USER_STORIES.md)
- **AI/AR Features:** [docs/AI_AR_FEATURES.md](../docs/AI_AR_FEATURES.md)
- **PRD:** [docs/PRD.md](../docs/PRD.md)

### External Resources
- **Flutter Docs:** https://docs.flutter.dev/
- **Firebase Docs:** https://firebase.google.com/docs
- **Riverpod:** https://riverpod.dev/
- **GeoFlutterFire:** https://pub.dev/packages/geoflutterfire_plus
- **Flutter i18n:** https://docs.flutter.dev/ui/accessibility-and-localization/internationalization

### Community
- **GitHub:** https://github.com/LifesaverLabs/Neighbor911
- **Issues:** https://github.com/LifesaverLabs/Neighbor911/issues
- **Email:** team@lifesaverlabs.org

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-10 | Nobody Clayman | Initial prompt document for AI contributors |

---

## Contributing Updates to This Document

If you discover better patterns, improved prompts, or common mistakes to avoid:

1. **Open a PR** with your suggested changes
2. **Explain the improvement** in the PR description
3. **Reference examples** where the pattern helped (or the old pattern caused issues)
4. **Get review** from 1-2 other contributors
5. **Merge and announce** in team channels

This document should evolve as we learn what works and what doesn't.

---

**Remember:** The goal is to save lives through rapid neighbor response. Every line of code, every architectural decision, every feature should serve that mission. When in doubt, ask: "Does this help us save more lives, faster?"
