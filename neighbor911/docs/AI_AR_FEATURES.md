# Neighbor 911 - AI, AR, and Advanced Features

**Version:** 0.2 (Draft)
**Last Updated:** 2025-11-10
**Status:** ⚠️ **DRAFT - FUTURE FEATURES**
**Organization:** Lifesaver Labs Public Benefit Corporation

---

## ⚠️ CRITICAL: PSAP Primacy & Safety Limitations

**Neighbor 911 is NOT a replacement for 911.**

For life-threatening emergencies requiring professional EMS, **users must call 911 FIRST.**

**Why:**
- E911 location accuracy (tower triangulation, device GPS)
- Medical history relay to dispatchers
- Direct connection to professional responders
- Legal framework and liability protections
- Battle-tested infrastructure

**Neighbor 911's role:** Supplement, not replace. We coordinate faster neighbor response WHILE professional help is en route.

---

## Table of Contents

1. [Conversational AI Alert Reporting](#1-conversational-ai-alert-reporting)
2. [PSAP Primacy & Handoff Protocols](#2-psap-primacy--handoff-protocols)
3. [AI-Assisted PSAP Audio Monitoring](#3-ai-assisted-psap-audio-monitoring)
4. [Augmented Reality (AR) Navigation](#4-augmented-reality-ar-navigation)
5. [Armed Responder Protocols](#5-armed-responder-protocols)
6. [Legal Witness Mode](#6-legal-witness-mode)
7. [AI Triage & Human Escalation](#7-ai-triage--human-escalation)

---

## 1. Conversational AI Alert Reporting

### 1.1 Problem Statement

**Current limitation:** Touch-typing and navigation tapping are too slow for some emergencies.

**Solution:** Voice-first conversational AI that accepts natural language reports.

**CRITICAL LIMITATION:** For life-threatening medical emergencies, AI MUST redirect to 911 first, then coordinate neighbor response in background.

### 1.2 Conversational AI Maturity Stages

We recognize **three maturity stages** based on technical capability and PSAP partnership:

**Stage 1: MVP (Minimal Viable Product - Weeks 1-16)**
- ❌ No AI conversational interface for critical emergencies
- ✅ AI voice interface ONLY for non-critical (wellness check, quit companion)
- ✅ Critical emergencies → "Call 911 first, I'll coordinate neighbors"
- ❌ No PSAP integration
- ❌ No call bouncing/handoff

**Stage 2: PSAP Partnership (Months 4-12)**
- ✅ AI can accept report but IMMEDIATELY transfers to 911
- ✅ AI stays on line to coordinate neighbor dispatch in background
- ✅ Passive PSAP audio monitoring (with consent)
- ⚠️ Call handoff tested but not relied upon for critical info

**Stage 3: Full Integration (Year 2+)**
- ✅ Seamless AI ↔ PSAP handoff
- ✅ Lossless information relay (address, medical history, context)
- ✅ E911 location features work through Neighbor 911
- ✅ PSAP can preempt/takeover Neighbor 911 calls at will
- ✅ Neighbor 911 can shotgun-seat on PSAP calls
- ✅ Mobile OS authorization for emergency calling

---

## 2. PSAP Primacy & Handoff Protocols

### 2.1 Stage 1 (MVP): "Call 911 First" Protocol

**User Experience: Someone mistakenly calls Neighbor 911 for cardiac arrest**

```
User: "Hey Neighbor 911, my dad just collapsed! He's not breathing!"

AI: "🚨 STOP - CALL 911 RIGHT NOW 🚨

     This is a life-threatening emergency.
     You MUST call 911 first for professional help.

     I'm going to help you:
     1. HANG UP and dial 911 immediately
     2. Tell 911: 'Cardiac arrest, not breathing'
     3. Give 911 your address
     4. STAY ON THE LINE with 911

     While you're on with 911, I'll send neighbor
     responders with CPR/AED to your location
     in the background.

     📞 HANG UP NOW AND CALL 911
     ⏱️  Professional help is dispatched by 911
     🏃 Neighbor help is being sent by us

     [TAP TO CALL 911 NOW]"

[If user doesn't hang up within 5 seconds:]

AI: "Are you still there? I need you to call 911.
     Can you dial 911 right now?"

User: "I don't know the address!"

AI: "That's okay. Call 911 and they can locate you
     from your phone. Just dial 911 now.

     I'm recording your location as:
     [123 Main Street, based on GPS]

     Tell 911 that address. But call them NOW.

     I'm dispatching neighbors to that location
     right now with CPR equipment.

     CALL 911. GO."
```

**What happens in the background:**
```typescript
// AI Flow for Mistaken Critical Emergency Call

1. User says: "collapsed", "not breathing", "cardiac arrest"
   → AI recognizes: CRITICAL MEDICAL EMERGENCY

2. AI IMMEDIATELY responds:
   - Display: "🚨 CALL 911 NOW 🚨"
   - Audio: "Stop - call 911 right now"
   - Button: [TAP TO CALL 911]

3. WHILE user is calling 911 (or after they hang up):
   - AI extracts location from GPS
   - AI extracts emergency type: "cardiac_arrest"
   - AI creates alert silently in background
   - AI dispatches to nearby CPR/AED responders
   - AI does NOT wait for user confirmation

4. If user gives address to AI before hanging up:
   - Store address for neighbor dispatch
   - But still require 911 call first

5. After neighbor dispatched:
   - Send confirmation SMS: "Neighbor responders dispatched
     to [address]. Mike with AED arriving in 2 min. 911
     professionals also en route."
```

**Key Principles (Stage 1 MVP):**
- ❌ Never let AI "handle" critical emergency alone
- ✅ Always redirect to 911 FIRST
- ✅ Extract minimal info (location, emergency type) while redirecting
- ✅ Dispatch neighbors in background automatically
- ✅ Confirm via SMS what we did
- ❌ Don't ask follow-up questions - get user to 911 faster

### 2.2 Stage 2: PSAP Partnership with Background Coordination

**Prerequisites for Stage 2:**
- ✅ Signed partnership agreement with local PSAP
- ✅ Tested call transfer protocol
- ✅ PSAP can see Neighbor 911 caller ID
- ✅ PSAP trained on "Neighbor 911 transfer" protocol
- ✅ Legal framework for shared liability

**User Experience: Cardiac arrest call with PSAP handoff**

```
User: "Hey Neighbor 911, my dad collapsed and isn't breathing!"

AI: "Cardiac arrest - connecting you to 911 NOW.
     Stay on the line.

     [3-way call initiated: User + AI + 911]"

911 Dispatcher: "911, what's your emergency?"

User: "My dad collapsed! He's not breathing!"

911: "What's your location?"

User: "Um, I'm at... uh..."

AI: [Provides to 911 dispatcher via data channel]
    "Location: 123 Main St, Apt 4B, San Francisco"

911: "Thank you. I'm dispatching paramedics now.
      They'll arrive in approximately 12 minutes.
      Can you start CPR?"

User: "I don't know how!"

911: "I'll guide you through it..."

AI: [Silent background action - not interrupting 911]
    → Dispatches CPR/AED neighbor responders
    → 5 responders alerted within 0.5 miles
    → Mike accepts alert, ETA 2 minutes

AI: [To 911 dispatcher via data channel]
    "Neighbor responder Mike Smith with AED accepted.
     ETA 2 minutes. Phone: 555-123-4567."

911: [To user] "Help is on the way. A trained neighbor
      with an AED will arrive in about 2 minutes to
      help until paramedics get there. Continue CPR."

[Mike arrives 2 minutes later]

911: "Is someone at the door?"

User: "Yes!"

911: "Let them in. They have an AED."

[Mike takes over CPR + AED while user stays on with 911]

911: [To Mike] "Are you the Neighbor 911 responder?"

Mike: "Yes, I have AED applied. Analyzing rhythm now."

[AED delivers shock]

Mike: "Shock delivered. Patient has pulse. Breathing resumed."

911: "Excellent. Paramedics are 8 minutes out. Stay with
      the patient and monitor breathing."

[Paramedics arrive, take over]

911: "Paramedics are on scene?"

Mike: "Yes, handing off now."

911: "Thank you for your help. You can disconnect."

Mike: [Marks alert as resolved in Neighbor 911 app]
```

**Technical Implementation (Stage 2):**
```typescript
// 3-Way Call Architecture

interface ThreeWayCall {
  participants: {
    user: { phoneNumber: string, location: GPSCoords };
    ai: { role: 'coordinator', speaksTo: ['user', 'psap'] };
    psap: { phoneNumber: '911', dispatcherId?: string };
  };

  dataChannels: {
    // AI ↔ PSAP data exchange (not voice)
    aiToPSAP: {
      location: { address: string, lat: number, lng: number };
      emergencyType: 'cardiac_arrest';
      neighborResponderStatus: {
        dispatched: 5,
        accepted: 1,
        responderName: 'Mike Smith',
        responderPhone: '+15551234567',
        eta: 120
      };
    };

    // PSAP ↔ AI data exchange
    psapToAI: {
      emsDispatched: true;
      emsETA: 720;
      instructions: 'guide_cpr';
      acknowledgeNeighborResponse: true;
    };
  };

  audioFlow: {
    user_hears: ['ai', 'psap'];  // User hears both AI and 911 dispatcher
    psap_hears: ['user'];        // 911 hears ONLY user (AI muted from voice)
    ai_hears: ['user', 'psap'];  // AI monitors conversation for context

    // AI speaks only when:
    aiSpeaksWhen: [
      'initial_connection',       // "Connecting to 911 now"
      'psap_requests_info',       // If 911 asks AI for data
      'critical_update'           // "Neighbor arrived with AED"
    ];
  };

  handoffProtocol: {
    // User can't accidentally hang up on 911
    userHangsUp: 'disconnect_ai_only_keep_psap_connected';
    psapHangsUp: 'end_call_for_all';
    aiDisconnects: 'keep_user_psap_connected';
  };
}
```

**Key Principles (Stage 2):**
- ✅ AI initiates 3-way call: User + AI + 911
- ✅ 911 dispatcher takes primary control of conversation
- ✅ AI provides data (location, neighbor status) via silent data channel
- ✅ AI coordinates neighbor dispatch in background
- ✅ AI only speaks when necessary (minimal interruption)
- ✅ User stays connected to 911 throughout
- ✅ If call drops, 911 connection preserved

### 2.3 Stage 3: Full Integration with Lossless Relay

**Prerequisites for Stage 3:**
- ✅ Mobile OS (iOS/Android) grants emergency calling permissions
- ✅ E911 location features work through Neighbor 911
- ✅ Medical ID / emergency contacts relay to PSAP
- ✅ PSAP can preempt Neighbor 911 calls
- ✅ Neighbor 911 can monitor PSAP calls (with consent)
- ✅ CAD system bidirectional integration
- ✅ Multi-year track record of safe handoffs

**User Experience: Seamless integration**

```
User: "Hey Neighbor 911, my dad collapsed!"

AI: "Cardiac arrest. Connecting to 911 and dispatching
     neighbors now."

[Automatic actions in <2 seconds:]
→ E911 location transmitted to PSAP (tower + GPS)
→ Medical ID relayed: "Patient: John Doe, age 67,
   history of heart disease, takes blood thinners"
→ 911 call connected
→ Neighbor dispatch initiated
→ AI monitors call for context

911: "911, I see cardiac arrest at 123 Main St,
      patient John Doe, age 67. Paramedics dispatched,
      ETA 11 minutes. Neighbor responders with AED
      also en route, ETA 2 minutes. Stay on the line."

User: "Yes, he's not breathing!"

911: "Can you start CPR or wait for the neighbor?"

User: "I'll try..."

[AI detects uncertainty in voice]

AI: [To 911 via data] "User sounds panicked, may need
    CPR coaching."

911: "I'll guide you. Place your hands center of chest..."

[Mike arrives with AED]

AI: [To 911 via data] "Neighbor responder on scene."

911: "Let the neighbor in. They have an AED."

[Seamless handoff to professional EMS when they arrive]
```

**PSAP Preemption (Stage 3):**
If PSAP detects Neighbor 911 user needs immediate help:

```
[User calls Neighbor 911 for wellness check]

User: "Can someone check on my neighbor? Haven't seen her in days."

AI: "I can send someone for a wellness check. What's the address?"

[PSAP monitoring system flags this - neighbor might be deceased]

PSAP: [Preempts call]
      "This is 911 dispatch. We're going to send an officer
       for this wellness check. What's the address?"

User: "Oh, okay. 456 Oak Street."

PSAP: "Officer dispatched. ETA 15 minutes. I'm also sending
       a Neighbor 911 responder to arrive sooner. Is that okay?"

User: "Yes, thank you."

[PSAP + Neighbor 911 coordinate response]
```

**Technical Requirements for Stage 3:**
- Mobile OS emergency calling API access
- E911 Phase II location accuracy
- Health app data relay (with user consent)
- Real-time CAD ↔ Neighbor 911 sync
- PSAP operator training on hybrid dispatch
- Legal framework for shared dispatch authority

**Timeline:** Stage 3 is **Year 2+ minimum**. We must prove reliability in Stages 1-2 first.

---

## 2.4 Non-Critical Emergencies: AI Handles Autonomously (All Stages)

**What AI CAN handle without 911 redirect:**

✅ **Wellness Check**
```
User: "Can you send someone to check on my elderly neighbor?"
AI: "I can do that. What's the address?"
User: "456 Oak Street, Apartment 2C."
AI: "Sending wellness check responder now. Alex will be there in 4 minutes."
```

✅ **Quit Companion**
```
User: "I'm having a really bad nicotine craving."
AI: "I can send a quit companion to help. Where are you?"
User: "At home, 789 Pine Street."
AI: "Jamie, a former smoker, is 2 minutes away. They'll come walk with you."
```

✅ **Companionship / Loneliness**
```
User: "I'm feeling really lonely tonight."
AI: "I'm sorry to hear that. Would you like me to find a neighbor
     who can chat with you or go for a walk?"
User: "Yes, that would help."
AI: "Sam is available and 0.5 miles away. They'll message you shortly."
```

✅ **Brain Damage Sport Dissuasion**
```
User: "I'm a cheerleader organizing a boycott of Friday's tackle football game."
AI: "I can help coordinate adult support for your advocacy.
     When and where is the game?"
User: "Lincoln High, Friday at 7pm."
AI: "Sending alert to 5 nearby adults who support youth brain safety advocacy."
```

**Key Principle:** If it's not life-threatening and doesn't require professional EMS, AI can handle it.

---

## 2.5 The "Address Problem" - Why PSAP Primacy Matters

**Problem:** User often doesn't know exact address in emergency.

**Why 911 is better:**
- E911 location from cell towers (±50m)
- Device GPS location
- Address database lookup
- Dispatcher can talk through landmarks
- Battle-tested location verification

**Why Neighbor 911 struggles:**
- User might not give address before hanging up
- GPS can be inaccurate indoors (±10-50m)
- Apartment/unit number often missing
- Rural areas especially difficult

**Example of what can go wrong (Stage 1 MVP):**

```
User: "My neighbor collapsed!"

AI: "Call 911 first. Where are you located?"

User: [Panicking] "I don't know! Just send help!"

AI: "I see your GPS location as near 123 Main Street.
     Is that correct?"

User: "I think so? I don't know the address!"

[User hangs up to call 911]

AI: [Dispatches to 123 Main Street based on GPS]

[Responder arrives at 123 Main Street - nobody there]

[Actual location: 125 Main Street - GPS was off by 50 feet]
```

**This is why 911 MUST be primary for life-threatening emergencies.**

### 2.6 Minimum Information Requirements

**Before dispatching neighbor responders, AI MUST have:**

✅ **Confirmed Address** (not just GPS coordinates)
- Street number
- Street name
- Apartment/unit (if applicable)
- City/ZIP

✅ **Emergency Type** (CPR, overdose, wellness check, etc.)

✅ **Safety Confirmation** (scene is safe for responders)

**If missing any of these:**
```
AI: "I need to confirm the address before sending help.
     What's the full address including apartment number?"

User: "I don't know!"

AI: "That's okay. Call 911 - they can locate you from your phone.
     I'll try to send neighbors based on GPS, but 911 is more accurate.

     📞 CALL 911 NOW"
```

---

## 2.7 Summary: PSAP Primacy Rules

| Emergency Type | Stage 1 (MVP) | Stage 2 (Partnership) | Stage 3 (Full Integration) |
|----------------|---------------|----------------------|----------------------------|
| **Cardiac Arrest** | "Call 911 first" redirect | 3-way call, 911 primary | Seamless E911 + neighbor dispatch |
| **Overdose** | "Call 911 first" redirect | 3-way call, 911 primary | Seamless E911 + neighbor dispatch |
| **Fire** | "Call 911 first" redirect | 3-way call, 911 primary | Seamless E911 + neighbor dispatch |
| **Choking** | "Call 911 first" redirect | 3-way call, 911 primary | Seamless E911 + neighbor dispatch |
| **Home Invasion** | "Call 911 first" redirect | 3-way call, 911 primary | Seamless E911 + neighbor dispatch |
| **Wellness Check** | AI handles | AI handles | AI handles (PSAP can preempt) |
| **Quit Companion** | AI handles | AI handles | AI handles |
| **Companionship** | AI handles | AI handles | AI handles |

**Golden Rule:** If professional EMS is needed, 911 is primary. Always.

---

## 3. AI-Assisted PSAP Audio Monitoring

### 3.1 Overview

**Purpose:** AI monitors 911 calls (with user consent) to extract actionable information for neighbor dispatch coordination.

**Key Principle:** AI is a **passive listener and coordinator**, never interrupting or replacing PSAP dispatcher authority.

### 3.2 What AI Monitors For

When user is on a 911 call and has opted into Neighbor 911 audio monitoring:

✅ **Location confirmation** - Extract address, apartment number, cross streets
✅ **Emergency type** - Cardiac arrest, overdose, fire, etc.
✅ **Needed capabilities** - CPR, AED, naloxone, fire extinguisher
✅ **EMS ETA** - "Paramedics are 12 minutes out"
✅ **Scene safety** - "Is the attacker still there?" → Don't dispatch if unsafe
✅ **Updates from dispatcher** - "Start CPR now" → Send CPR-capable responders priority

### 3.3 Privacy & Consent

**User must explicitly opt-in:**
```
During onboarding:
□ Allow Neighbor 911 AI to monitor my 911 calls
  to coordinate neighbor response faster

What this means:
• AI listens to your 911 calls to extract address, emergency type
• AI uses this to dispatch nearby neighbors with right skills
• Audio is NOT recorded or stored
• 911 dispatcher remains in full control
• You can disable this anytime in Settings
```

**Technical safeguards:**
- Audio stream processed in real-time, never stored
- AI only extracts structured data (address, emergency type, ETA)
- No audio recordings retained
- HIPAA-compliant processing
- User can revoke consent anytime

### 3.4 Example: AI Passive Monitoring

```
[User calls 911 directly, AI monitors with consent]

911: "911, what's your emergency?"

User: "My neighbor is having a seizure!"

AI: [Detects: emergency_type = "seizure", location unknown yet]

911: "What's the address?"

User: "Um, 742 Evergreen Terrace, Apartment 3."

AI: [Extracts: address = "742 Evergreen Terrace, Apt 3"]
    [Action: Query geospatial index for nearby responders]

911: "Is the person breathing?"

User: "Yes, but they're shaking violently!"

AI: [Detects: needed_capabilities = ["seizure_first_aid", "medical_assistance"]]

911: "Paramedics are on the way, about 10 minutes out.
     Clear the area around them so they don't hurt themselves."

AI: [Extracts: ems_eta = 600 seconds]
    [Action: Dispatch 3 nearby responders with medical training]
    [Action: Send silent notification: "Sarah (RN) accepted, ETA 3 min"]

911: "Stay with them and time the seizure. I'll stay on the line."

AI: [Status update to 911 via data channel]
    "Neighbor responder (registered nurse) en route, ETA 3 min"

911: [To user] "A neighbor who's a nurse is also coming to help
     until paramedics arrive. That okay?"

User: "Yes, thank you!"

[Sarah arrives, assists until EMS takes over]
```

**Key Points:**
- AI never spoke during the call
- 911 dispatcher remained in full control
- AI extracted info passively and dispatched appropriately
- 911 was informed of neighbor response via data channel

---

## 4. Augmented Reality (AR) Navigation

### 4.1 Overview

**Purpose:** Use AR to help responders navigate quickly and accurately to emergency locations, especially in complex environments (apartment buildings, office complexes, outdoor areas).

**Key Insight:** In an emergency, every second counts. AR can shave 30-60 seconds off arrival time by eliminating navigation confusion.

### 4.2 AR Use Cases for Neighbor 911

#### 4.2.1 AR Turn-by-Turn Navigation to Emergency

**Problem:** Traditional map navigation shows 2D overhead view. In stress, responders may:
- Miss turns
- Not recognize landmarks
- Struggle with apartment building entrances
- Lose time at parking/entry points

**AR Solution:** Camera-based navigation with real-world overlay

**User Experience:**
```
[Responder accepts alert]

App: "CPR needed - 0.3 miles away"
     [Start Navigation] [Call Originator]

[Responder taps Start Navigation]

[AR View activates - camera shows real-world view]

→ Big green arrow on ground: "→ Turn right on Elm St"
→ Distance overlay: "450 feet to emergency"
→ Estimated time: "2 minutes walking / 1 minute running"

[Responder turns corner]

→ Arrow points to building: "↑ Enter building ahead"
→ Building highlight: Green outline around correct entrance
→ "123 Main St - Apartment 4B"

[Responder enters building]

→ Arrow points to elevator: "↑ Take elevator to Floor 4"
→ Alternative: "Stairs on right (faster if <3 floors)"

[Responder exits elevator on Floor 4]

→ Arrow points down hallway: "→ 40 feet - Apartment 4B on left"
→ Door highlight: Green outline around correct door
→ "You've arrived - Ring doorbell"

[Arrival notification sent to originator]
```

**Technical Implementation:**
- ARCore (Android) / ARKit (iOS) via Flutter plugins
  - `arcore_flutter_plugin` or `ar_flutter_plugin`
- Google Maps API for route calculation
- Real-time GPS + compass for orientation
- Image recognition for building/door number identification (optional ML)
- Fallback to 2D map if AR unavailable

#### 4.2.2 AR Equipment Locator

**Problem:** Emergency equipment (AEDs, fire extinguishers, first aid kits) often hidden in cabinets, behind signs, or in unmarked locations.

**AR Solution:** Virtual markers showing where equipment is located

**User Experience - Finding AED in Office Building:**
```
[Alert: "Cardiac arrest at TechCorp Office, Floor 3"]

Responder: [Accepts alert, has AED access capability]

App: "Nearest AED: TechCorp lobby, 0.2 miles away"
     [Navigate to AED] [Go Directly to Emergency]

[Responder taps Navigate to AED]

[AR View activates]

→ Arrow on ground: "↗ AED 0.2 miles northeast"
→ Marker in distance: Floating AED icon above building

[Responder arrives at building entrance]

→ AR highlights AED cabinet: Green glow around cabinet on wall
→ Text overlay: "AED inside - press to open"
→ Distance: "15 feet to your right"

[Responder retrieves AED]

App: "AED retrieved. Navigate to emergency?"
     [Yes - Start Navigation]

[AR navigation to emergency location with AED in hand]
```

**Equipment Database Requirements:**
- Crowdsourced AED locations (user submissions)
- Verified locations (trained volunteers confirm)
- Indoor mapping (for large buildings)
- Access codes (if equipment is locked)
- Equipment status (last inspection date, battery level if IoT-enabled)

#### 4.2.3 AR Scene Assessment & Safety

**Problem:** Responders arriving at chaotic scenes may not know:
- Where is safe to enter?
- Where are other responders?
- Where is the patient?
- What hazards exist?

**AR Solution:** Real-time scene overlay with safety information

**User Experience - Multi-Responder Coordination:**
```
[3 responders en route to same cardiac arrest]

[AR View shows:]
→ Patient location: Red pulsing marker at apartment door
→ Other responders: Blue avatars with names and ETAs
  - "Mike (AED) - 30 seconds away, approaching from north"
  - "Sarah (CPR certified) - 45 seconds away, approaching from south"
→ Your role: "You: First responder - prepare to start CPR"

[Mike arrives first]

App: "Mike arrived with AED. Coordinate entry together."

[Both responders at door]

→ AR highlights door: "Ring doorbell - originator will let you in"
→ Safety check: "Scene reported as safe. Patient inside, conscious person present."

[Inside apartment]

→ AR shows roles:
  - Mike: "Apply AED pads"
  - Sarah: "Prepare to start CPR if needed"
  - You: "Call out instructions from AED"
```

**Safety Overlay Features:**
- Scene safety status: ✅ Safe / ⚠️ Caution / 🚫 Unsafe
- Hazard markers: Fire, smoke, weapon, aggressive person
- Safe zones: Green highlighted areas
- Exit routes: Yellow arrows to nearest exit
- Other responder locations: Real-time positions

#### 4.2.4 AR CPR & First Aid Guidance

**Problem:** Even trained responders may forget:
- Correct hand placement
- Compression depth (2-2.4 inches)
- Compression rate (100-120 per minute)
- When to use AED

**AR Solution:** Visual overlay during CPR

**User Experience:**
```
[Responder arrives, patient unconscious]

App: "Start CPR. AR guidance available."
     [Enable AR Guidance]

[AR View activates, camera pointed at patient]

→ Hand placement marker: Green circles on patient's chest
  "Place hands here - center of chest"

[Responder places hands]

→ Depth gauge: Vertical bar showing compression depth
  "Push 2-2.4 inches deep"
  [Visual feedback: Green when correct depth, red if too shallow/deep]

→ Metronome: Visual beat indicator
  "100 compressions per minute"
  [Pulsing circle on screen matching rhythm]

→ Counter: "30 compressions, then 2 breaths"

[After 30 compressions]

App: "Give 2 rescue breaths. Tilt head back, lift chin."
     [AR shows head tilt angle]

[After 2 breaths]

App: "Resume compressions. AED arriving in 30 seconds."

[AED arrives]

App: "Stop CPR. Apply AED pads."
     [AR shows pad placement: Right upper chest, left lower ribs]

[AED analyzes rhythm]

AED: "Shock advised. Stand clear."

[AR highlights safe zone: Red outline around patient, "STAND BACK"]

[Shock delivered, CPR resumed with AR guidance]
```

**Technical Features:**
- Computer vision for body position detection (optional ML)
- Accelerometer for compression depth measurement
- Audio metronome (accessible fallback)
- Integration with smart AED devices (if available)

#### 4.2.5 AR for Low-Light & Nighttime Response

**Problem:** Emergencies at night or in dark areas make navigation difficult:
- Can't see house numbers
- Dark alleys/paths are confusing
- Indoor stairwells poorly lit

**AR Solution:** Enhanced vision mode

**User Experience:**
```
[2am cardiac arrest alert, dark residential street]

App: "Emergency nearby - Enable AR Night Mode?"
     [Enable]

[AR View with enhanced features:]
→ Flashlight mode: Camera activates flash for illumination
→ Address recognition: ML detects house numbers even in dark
  "123 visible ahead - target is 127, continue 40 feet"
→ Pathway highlighting: Green line on ground showing safe walking path
→ Obstacle detection: Yellow markers on curbs, steps, hazards
→ Building outline: Highlights correct building structure

[Approaching dark apartment building]

→ AR highlights entrance: "Main entrance - door on left"
→ Light indicator: "Doorbell will alert resident"
```

**Technical Implementation:**
- Flash/torch activation
- Image enhancement (brightness, contrast)
- ML-based number recognition (TensorFlow Lite)
- Depth sensing (LiDAR on supported devices)

#### 4.2.6 AR Advocacy Support (Optional Assistance, Not Borg Mode)

**Philosophy: Human-First, Machine-Assisted**

⚠️ **IMPORTANT:** This feature is **entirely optional** and designed for people who want backup support. Many students and adults are naturally excellent at advocacy conversations—empathetic, persuasive, balanced—and need zero help. **This is for those who want it, not for everyone.**

**We don't want to turn people into the Borg.** Real conversations are human, natural, spontaneous, and authentic. AR advocacy support is a **quiet assistant in your pocket**, not a script you read robotically.

**Think of it like:**
- 📚 Having cheat notes for a presentation (glance when you need them, ignore when you don't)
- 🎯 A debate team coach whispering reminders from the sidelines
- 📊 A fact-checker helping you remember statistics accurately
- 🧠 Training wheels you can remove once you're confident

**The goal:** Help nonprofessionals who want extra support **outperform even further** on balanced discussion and persuasion, while staying natural and human-led.

---

**Problem:** Advocates responding to brain-damaging sport dissuasion, quit companion support, or other educational encounters may want help recalling:
- Key statistics (to avoid misquoting)
- Evidence-based talking points (when conversation stalls)
- De-escalation techniques (if tension rises)
- Alternative suggestions (to offer constructive solutions)

**AR Solution:** Discreet, glanceable reference material—use it if you want, ignore it if you don't.

**User Experience - Brain-Damaging Sport Dissuasion (Optional Mode):**
```
[High school football game, parent advocate responding to alert]

Advocate: [Can choose to enable AR Advocacy Support—or not]

App: "Would you like advocacy talking points available?
      Many advocates prefer to speak naturally without them."
      [Enable Support] [No Thanks, I'm Good]

[If advocate enables:]

[DISCREET sidebar appears - small, minimized, glanceable]

📊 Quick Facts (tap to expand):
• 1 in 5 HS players: concussion/season (CDC)
• 99% deceased NFL: CTE (BU study)
• Flag football: same skills, zero head impacts

💡 If stuck (tap for suggestions):
• Deflecting "builds character" → Flag does too
• Deflecting "overreacting" → 99% is data, not opinion
• Alternatives: Flag leagues, 7v7, other sports

[Advocate speaks naturally, glances at phone only when needed]

Advocate: "Hey, I wanted to share some research with you all..."

[Conversation flows naturally - advocate in control]

Parent: "But doesn't football build character?"

[Advocate glances at phone for reminder]

Advocate: "Absolutely—and flag football builds the same character
          without the concussions. Leadership, teamwork, all there."

[Conversation continues - phone mostly ignored]

Parent: "Where would they even play flag football?"

[Advocate checks local resources]

Advocate: "Actually, Lincoln High just started a Saturday league.
          It's competitive, and colleges are recruiting from flag now."

[AR support is backup, not driving the conversation]
```

**Key Principles for AR Advocacy Support:**

1. **Human-led, not script-led**
   - You talk naturally, glance at phone if you forget a stat
   - Not: Reading from screen like a robot

2. **Opt-in, not default**
   - Many people don't want or need this
   - Choice to enable per encounter

3. **Discreet, not intrusive**
   - Small sidebar, not full-screen takeover
   - Easy to minimize or disable mid-conversation

4. **Reference, not script**
   - Facts, stats, alternatives (things you might forget)
   - Not: "Say this exact phrase" (that's weird)

5. **Supplement expertise, don't replace it**
   - Great advocates become even better with backup
   - Not: Turn novices into automatons

**What AR Advocacy Support Provides:**

✅ **Fact-checking on demand**
```
"Was it 1 in 5 or 1 in 10? Let me check... 1 in 5, CDC 2023"
```

✅ **Memory aids for alternatives**
```
"What were those flag football leagues nearby again?
 Oh right—Lincoln HS Saturdays, Riverside League Sundays"
```

✅ **Counters to common pushback**
```
[Parent says: "Scouts only look at tackle players"]
[You think: "I know that's wrong but can't remember why"]
[Glance at phone: "15% of NFL never played tackle in HS"]
[You say: "Actually, a lot of pros came from flag or other sports"]
```

✅ **De-escalation reminders when tense**
```
[Conversation getting heated]
[Phone vibrates: "Validate emotion, then reframe with data"]
[You pivot: "I hear you—change is hard. But we have info now
 that we didn't 10 years ago..."]
```

✅ **Resources to share afterward**
```
[Conversation went well]
[You: "Want me to text you those studies?"]
[Phone: One-tap to send fact sheet, local resources]
```

**What AR Advocacy Support Does NOT Do:**

❌ Tell you what to say word-for-word (robotic, inauthentic)
❌ Take over the conversation (you're in control)
❌ Make you dependent on it (learn and internalize over time)
❌ Replace genuine human connection (empathy, rapport, authenticity)
❌ Turn you into a walking billboard (persuasion is relationship-first)

**User Experience - Quit Companion Support (Human-First Example):**

```
[Companion arrives to help person with nicotine craving]

Companion: [Genuinely present, phone in pocket]

Person: "I'm really struggling. I don't think I can do this."

Companion: [Makes eye contact, listens] "I hear you. That's so hard."

[Natural silence - just walking together]

Person: "How long until this gets easier?"

Companion: [Glances at phone briefly] "From what I've read,
           cravings peak around week 2, then it gets better.
           You're in the hardest part right now."

[Phone back in pocket, focus on person]

Person: "I just want one cigarette."

Companion: "What if we walk for 5 more minutes? Cravings usually
           pass in 3-5 minutes. Let's see if this one does too."

[Companion might have gotten the "3-5 minutes" reminder from AR,
 but person never knows—conversation feels natural]
```

**The Balance:**
- **With AR support:** "I have backup if I need it" (confidence boost)
- **Without AR support:** "I got this on my own" (natural flow)
- **Both are valid.** Use what works for you.

**Technical Design for Non-Borg Experience:**

```dart
// AR Advocacy mode is MINIMAL by default
class AdvocacySupportUI {
  // Small, collapsible sidebar (not full-screen overlay)
  Widget buildMinimalMode() {
    return Positioned(
      bottom: 100,
      right: 20,
      child: CollapsibleCard(
        collapsed: true, // Starts minimized
        width: 80, // Tiny when collapsed
        child: Text("📊 Tap for facts"),
        onTap: () => expandToShowTalkingPoints(),
      ),
    );
  }

  // Vibration alerts are GENTLE reminders, not commands
  void sendGentleReminder(String context) {
    if (userEnabledReminders && conversationTense) {
      HapticFeedback.lightImpact(); // Subtle vibration
      showSnackbar("Tip: Validate emotion first"); // Brief, dismissible
    }
  }

  // Easy bailout - disable mid-conversation
  Widget buildQuickDisable() {
    return FloatingActionButton(
      mini: true,
      child: Icon(Icons.close),
      onPressed: () {
        disableAdvocacyMode();
        showToast("Advocacy support disabled. You got this!");
      },
    );
  }
}
```

**Accessibility & Learning:**

**For beginners:** AR support is training wheels
- Use heavily at first (build confidence)
- Gradually use less (internalize talking points)
- Eventually disable entirely (you've learned it)

**For experts:** AR support is reference material
- Rarely needed
- Glance for specific stats if asked
- Mostly ignored

**For everyone:** Choice and control
- Enable/disable per encounter
- Customize what you see (hide categories you don't need)
- Share what worked (improve database for others)

### 4.7 AR Advocacy Database - Call for Contributors

**We need subject matter experts to build advocacy support databases!**

**Content philosophy:**
- **Reference material, not scripts** (facts, stats, alternatives)
- **Conversational suggestions, not robotic phrases** ("Try mentioning..." not "Say exactly...")
- **Backup for those who want it, invisible for those who don't**

**Content needed for each advocacy scenario:**
- **Brain-damaging sports:** CTE researchers, neurologists, former athletes, coaches
- **Quit companions:** Addiction counselors, psychologists, former smokers/vapers
- **Harm reduction:** Public health experts, needle exchange workers, overdose prevention specialists
- **Consent education:** Sex educators, Title IX coordinators, survivors' advocates

**How to contribute:**
1. Submit talking points (as suggestions, not mandates)
2. Add statistics (with sources for fact-checking)
3. Share de-escalation techniques (that real humans actually use)
4. Suggest alternatives (constructive solutions)
5. Review for natural language (avoid corporate/robotic phrasing)

**Example contribution format:**
```yaml
# docs/advocacy_databases/brain_damage_sports.yaml

scenario: brain_damage_sport_dissuasion

# These are REFERENCE facts, not scripts to read aloud
statistics:
  - headline: "1 in 5 high school football players suffer concussion per season"
    source: "CDC, 2023"
    how_to_use: "Mention if asked about frequency, but don't lead with scary stats"

  - headline: "99% of deceased NFL players showed CTE"
    source: "Boston University CTE Center, 2024"
    how_to_use: "Save for pushback like 'you're overreacting'—this is hard data"

# These are CONVERSATION STARTERS, not word-for-word scripts
talking_points:
  - category: deflecting_pushback
    trigger: "builds character"
    suggestion: "Agree football builds character, then pivot to flag football doing the same"
    example_phrasing: "Totally agree—and flag football builds the same leadership and teamwork"
    tone: conversational
    note: "Don't be defensive, validate their point first"

# These are ALTERNATIVES to offer, not ultimatums
alternatives:
  - suggestion: "Flag football leagues"
    why_it_works: "Keeps what kids love (strategy, competition) without head impacts"
    local_resources: "Lincoln HS flag league, Saturdays 9am"
    how_to_introduce: "Ask if they've heard of the new flag leagues, don't demand they switch"
```

**Join the advocacy content team:**
- GitHub: Tag issues with `#advocacy-database`
- Email: advocacy-content@lifesaverlabs.org

**Remember:** The goal is to help humans be better humans, not turn them into machines.

### 4.3 AR Technical Architecture

#### 4.3.1 Flutter AR Plugins

**Recommended packages:**
```yaml
dependencies:
  ar_flutter_plugin: ^0.7.3
    # Cross-platform AR (ARCore + ARKit)

  arcore_flutter_plugin: ^0.1.0
    # Android-specific (if deeper ARCore integration needed)

  arkit_plugin: ^1.0.7
    # iOS-specific (if deeper ARKit integration needed)

  geolocator: ^10.1.0
    # GPS positioning for AR anchors

  sensors_plus: ^4.0.0
    # Compass, accelerometer for orientation

  google_maps_flutter: ^2.5.0
    # Route calculation, geocoding
```

#### 4.3.2 AR Anchor System

**Concept:** Place virtual markers at real-world GPS coordinates

```dart
// Place AR marker at emergency location
class EmergencyARMarker {
  final LatLng location;
  final String type; // 'patient', 'equipment', 'responder'
  final String label;
  final Color color;

  ARNode createARNode(ARSessionManager arSessionManager) {
    return ARNode(
      type: NodeType.localGLTF2,
      uri: "assets/ar/emergency_marker.gltf",
      scale: Vector3(0.5, 0.5, 0.5),
      position: calculateARPosition(location),
      rotation: Vector4(0, 1, 0, 0),
    );
  }

  Vector3 calculateARPosition(LatLng target) {
    // Convert GPS coordinates to AR world coordinates
    // relative to user's current position
    final userLocation = getCurrentLocation();
    final bearing = calculateBearing(userLocation, target);
    final distance = calculateDistance(userLocation, target);

    return Vector3(
      distance * sin(bearing), // X
      0, // Y (ground level)
      distance * cos(bearing), // Z
    );
  }
}
```

#### 4.3.3 AR Plane Detection

**Use case:** Detect ground plane for placing directional arrows

```dart
arSessionManager.onPlaneOrPointTap = (List<ARHitTestResult> hits) {
  if (hits.isNotEmpty) {
    final hit = hits.first;

    // Place arrow on detected ground plane
    final arrowNode = ARNode(
      type: NodeType.localGLTF2,
      uri: "assets/ar/navigation_arrow.gltf",
      position: hit.worldTransform.getColumn(3),
      rotation: Vector4(0, 1, 0, calculateBearing()),
    );

    arSessionManager.addARNode(arrowNode);
  }
};
```

#### 4.3.4 Performance Considerations

**AR is resource-intensive:**
- Camera stream processing
- Real-time position tracking
- 3D rendering
- ML inference (for object detection)

**Optimization strategies:**
1. **Battery management:** Offer "AR mode" toggle, fallback to 2D map
2. **Thermal throttling:** Reduce AR fidelity if device overheating
3. **Network usage:** Download AR assets once, cache locally
4. **Accessibility:** Always provide non-AR alternative (for older devices)

**Fallback for devices without AR:**
```dart
Future<bool> checkARAvailability() async {
  if (Platform.isAndroid) {
    return await ArCoreController.checkArCoreAvailability();
  } else if (Platform.isIOS) {
    return await ArKitController.checkArKitAvailability();
  }
  return false;
}

// In navigation screen
final arAvailable = await checkARAvailability();

if (arAvailable && userPrefersAR) {
  navigateWithAR();
} else {
  navigateWith2DMap(); // Standard Google Maps
}
```

### 4.4 AR Privacy & Safety Considerations

#### 4.4.1 Camera Privacy

**Concerns:**
- Responders using AR may inadvertently record sensitive scenes
- Camera pointed at patient during medical emergency
- Bystanders captured in AR footage

**Mitigations:**
- ❌ **Never record AR camera feed** (process in real-time only)
- ✅ **Display "AR Active - Camera On" indicator** prominently
- ✅ **Auto-disable AR when emergency resolved**
- ✅ **Privacy mode:** Blur background except navigation markers
- ✅ **User control:** Easy toggle to disable AR mid-response

```dart
// Privacy-preserving AR mode
ARSession(
  recordingEnabled: false, // Never record
  backgroundBlur: true, // Blur everything except markers
  showCameraIndicator: true, // Prominent "Camera Active" icon
);
```

#### 4.4.2 Cognitive Load

**Concern:** AR may overwhelm responders with information during high-stress emergency

**Mitigations:**
- Start with **minimal AR** (just directional arrow)
- **Progressive disclosure:** Show more info as responder approaches
- **Distraction-free mode:** Hide all UI except critical navigation
- **Audio cues:** Voice guidance alongside AR (accessible alternative)

**Example: Minimal AR mode**
```
Stage 1 (far away): Single arrow + distance
Stage 2 (approaching): Building highlight + address
Stage 3 (at entrance): Door highlight + apartment number
Stage 4 (inside): Role assignment + patient location
```

#### 4.4.3 Accessibility

**AR may not work for:**
- Visually impaired users
- Users with older devices (no ARCore/ARKit)
- Users in very dark environments (camera can't see)
- Users with shaky hands (AR anchors unstable)

**Always provide accessible alternatives:**
- Voice-guided navigation (turn-by-turn audio)
- High-contrast 2D map
- Text-based directions
- Vibration cues (haptic feedback for turns)

### 4.5 AR Feature Roadmap

**MVP (Stage 1):**
- ✅ Basic AR turn-by-turn navigation to emergency
- ✅ Distance markers and directional arrows
- ✅ 2D map fallback for devices without AR

**Stage 2 (Months 3-6):**
- ✅ Multi-responder coordination (show other responders in AR)
- ✅ Equipment locator (AEDs, fire extinguishers)
- ✅ Building entrance/door highlighting

**Stage 3 (Months 6-12):**
- ✅ CPR guidance overlay with depth/rate feedback
- ✅ Scene safety assessment (hazard markers)
- ✅ Night mode with enhanced vision

**Stage 4 (Year 2+):**
- ✅ ML-based object detection (AED recognition, house numbers)
- ✅ Indoor mapping for large buildings (malls, offices, hospitals)
- ✅ AR collaboration (responders see each other's view)
- ✅ Integration with smart AED devices (real-time feedback)

### 4.6 Call to Action for AR Developers

**We need AR/VR specialists to help design and build these features!**

**Skills needed:**
- ARCore / ARKit experience
- Flutter AR plugin development
- 3D modeling (GLTF/GLB assets for markers)
- Computer vision / ML (object detection)
- UX design for high-stress environments
- Accessibility expertise (non-visual AR alternatives)

**Open questions for AR community:**
1. What's the best way to handle AR in low-light conditions?
2. How do we balance information density vs. cognitive load?
3. What AR features would YOU want as a first responder?
4. How can we make AR accessible to visually impaired users?
5. What's the best fallback for devices without AR support?

**Join the discussion:**
- GitHub Issues: Tag `#AR` or `#augmented-reality`
- Email: ar-team@lifesaverlabs.org
- Discord: #ar-features channel (coming soon)

---

## 5. Armed Responder Protocols
