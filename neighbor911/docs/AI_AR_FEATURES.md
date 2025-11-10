# Neighbor 911 - AI, AR, and Advanced Features

**Version:** 0.2 (Draft)
**Last Updated:** 2025-01-09
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

[Content continues with PSAP monitoring, AR navigation, armed responder protocols, legal witness mode as before, but all examples now properly defer to 911 for critical emergencies]

---

**Would you like me to continue with the rest of the sections (AR, armed protocols, legal witness) with this corrected PSAP primacy framework, or would you like to refine the handoff protocols further first?**
