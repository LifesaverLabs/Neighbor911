# Neighbor911 - User Stories & Use Cases

**Version:** 1.0
**Last Updated:** 2025-11-07
**Status:** Draft
**Organization:** Lifesaver Labs

---

## Overview

This document contains user stories and use cases for the Neighbor911 Minute Response MVP. Stories are organized by user role and prioritized for the 8-week development timeline.

**Priority Levels:**
- 🔴 **P0 (Critical):** Must have for 8-week MVP
- 🟡 **P1 (High):** Should have for launch
- 🟢 **P2 (Medium):** Nice to have, post-MVP
- ⚪ **P3 (Low):** Future consideration

---

## Table of Contents
1. [Minute Responder Stories](#1-minute-responder-stories)
2. [Alert Originator Stories](#2-alert-originator-stories)
3. [Government/Admin Stories](#3-governmentadmin-stories)
4. [System Stories](#4-system-stories)
5. [Detailed Use Cases](#5-detailed-use-cases)

---

## 1. Minute Responder Stories

### 1.1 Registration & Onboarding

#### 🔴 US-R1: Sign Up as a Responder
**As a** neighbor who wants to help
**I want to** sign up for Neighbor911 with my phone number
**So that** I can respond to emergencies in my community

**Acceptance Criteria:**
- User can enter phone number
- User receives SMS verification code
- User enters code to verify phone number
- User is taken to profile setup on successful verification
- Error messages shown for invalid phone or code

**Notes:** Use Firebase Phone Auth

---

#### 🔴 US-R2: Create Basic Profile
**As a** new responder
**I want to** create a profile with my name, photo, and address
**So that** the system knows who I am and where I live

**Acceptance Criteria:**
- User can enter first and last name
- User can upload or take a profile photo (optional for MVP)
- User can enter home address (with autocomplete)
- User can save profile
- Profile data is stored securely

**Notes:** Address used for proximity matching, not displayed to others

---

#### 🔴 US-R3: Select Response Capabilities
**As a** responder
**I want to** select which types of emergencies I'm comfortable responding to
**So that** I only get alerts I can handle

**Acceptance Criteria:**
- User sees list of emergency types with brief descriptions
- User can check/uncheck capabilities
- User can select multiple capabilities
- User can save selections
- User can change selections later in settings
- No training requirement for MVP (honor system)

**Emergency Types (MVP):**
- Wellness Check / Door Knock
- Active Bystander Witness
- Addiction Support / Quit Companion (nicotine, tobacco, substances)
- 911 Coordination
- CPR / Cardiac Arrest
- AED Delivery
- Naloxone / Overdose Response

**Notes:** For MVP, defer training videos and quizzes

---

#### 🟡 US-R4: Watch Training Video
**As a** responder
**I want to** watch a brief training video for my selected capabilities
**So that** I know what to do when I respond

**Acceptance Criteria:**
- User can watch embedded video (YouTube)
- Video plays within app
- User can mark video as completed
- Completion tracked in profile

**Notes:** MVP includes 1-2 videos max (orientation + wellness check)

---

#### 🟡 US-R5: Pass Capability Quiz
**As a** responder
**I want to** take a short quiz after the training video
**So that** I can confirm my understanding

**Acceptance Criteria:**
- Quiz appears after video completion
- 3-5 multiple choice questions
- User must score 80%+ to pass
- User can retake quiz if failed
- Passing quiz certifies capability

**Notes:** Defer to Week 6 if time constrained

---

### 1.2 Availability Management

#### 🔴 US-R6: Set Availability Status
**As a** responder
**I want to** toggle my availability on/off
**So that** I only receive alerts when I'm home and ready to help

**Acceptance Criteria:**
- User sees clear "Available Now" toggle on home screen
- Toggle is OFF by default
- When ON, user can receive emergency alerts
- When OFF, user does not receive alerts
- Status saved and persisted across app restarts

**Notes:** Simple boolean flag for MVP

---

#### 🔴 US-R7: Set Alert Schedule for Non-Life-Threatening Emergencies
**As a** responder
**I want to** set specific hours/days when I'm willing to receive non-life-threatening alerts
**So that** I don't get woken up at 3am for wellness checks or bothered at work for quit companion requests

**Acceptance Criteria:**
- User can set time windows for each day of week (e.g., Mon-Fri 6pm-11pm)
- User can toggle days on/off
- Life-threatening alerts (CPR, AED, naloxone, fire, drowning) ALWAYS bypass schedule
- Non-life-threatening alerts (wellness check, quit companion, bystander, companionship) respect schedule
- System clearly labels which alert types are life-threatening vs non-life-threatening
- Default schedule: Weeknights 6pm-11pm, Weekends All Day
- User can disable non-life-threatening alerts entirely (opt out)

**Interface Example:**
```
ALERT SCHEDULE

Life-threatening (CPR, AED, overdose, fire):
  ☑ Always notify me 24/7

Non-life-threatening (wellness, quit support):
  Monday:    [6:00 PM] - [11:00 PM] ☑
  Tuesday:   [6:00 PM] - [11:00 PM] ☑
  Wednesday: [6:00 PM] - [11:00 PM] ☑
  Thursday:  [6:00 PM] - [11:00 PM] ☑
  Friday:    [6:00 PM] - [1:00 AM]  ☑
  Saturday:  [All Day]                ☑
  Sunday:    [All Day]                ☑

  ☐ Disable all non-life-threatening alerts
```

**Why This Matters:**
- Prevents responder burnout and app abandonment
- Respects work schedules and sleep
- Still ensures true emergencies get through 24/7
- Increases long-term retention

**Notes:** PROMOTED TO MVP - Critical for retention and preventing burnout

---

#### 🟢 US-R8: Enable Auto-Detection
**As a** responder
**I want to** automatically set availability based on my location
**So that** I don't have to remember to toggle

**Acceptance Criteria:**
- User can enable location-based auto-availability
- When user is at home address, availability = ON
- When user leaves home, availability = OFF
- Background location tracking with privacy controls

**Notes:** Post-MVP, requires background location permissions

---

### 1.3 Receiving & Accepting Alerts

#### 🔴 US-R9: Receive Emergency Alert
**As an** available responder
**I want to** receive a push notification when there's a nearby emergency I can help with
**So that** I can decide whether to respond

**Acceptance Criteria:**
- Notification received within 3 seconds of emergency trigger
- Notification shows: emergency type, distance, brief description
- Notification bypasses Do Not Disturb mode
- Notification plays loud alert sound
- Tapping notification opens app to alert detail screen

**Notes:** Use Firebase Cloud Messaging (FCM)

---

#### 🔴 US-R10: View Alert Details
**As a** responder who received an alert
**I want to** see full details of the emergency
**So that** I can make an informed decision about responding

**Acceptance Criteria:**
- Alert screen shows:
  - Emergency type
  - Distance from my location
  - Estimated time to reach (walking/driving)
  - Basic info about person in need (age, gender - if provided)
  - Optional description/notes from alert originator
- 30-second countdown timer displayed prominently
- Accept and Decline buttons clearly visible

**Notes:** Full-screen takeover for urgency

---

#### 🔴 US-R11: Accept Emergency Alert
**As a** responder
**I want to** accept an emergency alert within 30 seconds
**So that** I can commit to helping

**Acceptance Criteria:**
- User taps "ACCEPT - I'M ON MY WAY" button
- System immediately notifies alert originator
- System prevents other responders from accepting (or limits to 2-3)
- User is taken to navigation/guidance screen
- Status updated to "Responding"

**Notes:** For MVP, allow first responder to claim alert

---

#### 🔴 US-R12: Decline Emergency Alert
**As a** responder
**I want to** decline an alert if I can't respond
**So that** it can be sent to someone else

**Acceptance Criteria:**
- User taps "DECLINE - Find Someone Else" button
- Alert dismissed from user's screen
- System immediately dispatches to next available responder
- User receives no further notifications for this emergency

**Notes:** No penalty for declining in MVP

---

#### 🔴 US-R13: Handle Timeout
**As a** responder who doesn't respond within 30 seconds
**I want to** have the alert auto-dismissed and sent to others
**So that** time isn't wasted waiting for me

**Acceptance Criteria:**
- If no action taken within 30 seconds, alert auto-dismissed
- System dispatches to next available responders
- User may receive re-alert if no one else available

**Notes:** Countdown timer visible to user

---

#### 🟡 US-R14: Re-Alert as Last Resort
**As a** responder who declined
**I want to** be re-alerted if no one else is available
**So that** I can reconsider helping even if not my first choice

**Acceptance Criteria:**
- If all responders decline or timeout, system re-alerts declined users
- Re-alert message: "NO OTHER RESPONDERS AVAILABLE - Please reconsider"
- User can accept or final decline
- Limit to 1 re-alert per responder

**Notes:** Escalation logic

---

### 1.4 Navigation & Response

#### 🔴 US-R15: Get Directions to Emergency
**As a** responder who accepted an alert
**I want to** get turn-by-turn directions to the emergency location
**So that** I can get there quickly

**Acceptance Criteria:**
- User sees "Start Navigation" button
- Tapping button opens Google Maps (Android) or Apple Maps (iOS) with destination
- Destination is emergency location
- Navigation starts immediately

**Notes:** Deep link to native maps app for MVP, don't build custom navigation

---

#### 🔴 US-R16: Use Bluetooth Homing
**As a** responder getting close to the location
**I want to** see distance and direction to the exact emergency location
**So that** I can find them in a building or crowded area

**Acceptance Criteria:**
- When within 50 meters, Bluetooth homing activates
- Screen shows:
  - Distance to target (in feet/meters)
  - Directional arrow pointing toward target
  - Simple "warmer/colder" visual indicator
- Updates in real-time as responder moves
- Works via Bluetooth connection to originator's phone

**Notes:** Simple implementation - distance calculation + arrow

---

#### 🔴 US-R17: View On-Scene Tips
**As a** responder who arrived at the emergency
**I want to** see brief tips for how to help
**So that** I can respond effectively and safely

**Acceptance Criteria:**
- "You've Arrived" screen shows when Bluetooth proximity < 10 feet
- Screen displays 3-5 key tips specific to emergency type
- Tips are text-based, easy to scan
- "View Full Instructions" button for detailed protocol (expandable)
- "Mark Emergency Resolved" button

**Example Tips for Wellness Check:**
- Knock firmly on door
- Identify yourself as a neighbor checking in
- Ask "Are you okay? Do you need help?"
- Call 911 if person is injured or unresponsive
- Stay with them until help arrives

**Notes:** Text only for MVP, no fancy graphics

---

#### 🟡 US-R18: See Equipment Reminders
**As a** responder who accepted an alert
**I want to** be reminded what equipment to bring
**So that** I don't forget critical items (AED, naloxone, etc.)

**Acceptance Criteria:**
- Immediately after accepting, show equipment reminder popup
- List items relevant to emergency type
- User can dismiss reminder
- Reminder appears before navigation starts

**Example:**
```
✓ Alert Accepted

Remember to grab:
  🏥 Your AED
  📱 Your phone
  🔦 Flashlight (if nighttime)

[START NAVIGATION]
```

**Notes:** Phase 1.5, post-MVP

---

### 1.5 Communication & Coordination

#### 🔴 US-R19: Call 911 Manually
**As a** responder on scene
**I want to** call 911 with one tap
**So that** I can get professional help quickly

**Acceptance Criteria:**
- "Call 911" button prominently displayed on response screens
- Tapping button opens phone dialer with 911 pre-dialed
- User can proceed with call or cancel

**Notes:** Deep link to phone dialer, no automation for MVP

---

#### 🔴 US-R20: Update Response Status
**As a** responder
**I want to** update my status as I respond
**So that** the person in need knows help is coming

**Acceptance Criteria:**
- User can tap status buttons: "On My Way", "Arrived", "Emergency Resolved"
- Status sent to alert originator in real-time
- Status visible to originator and system

**Notes:** Simple button taps, no custom messages for MVP

---

#### 🟢 US-R21: Message Alert Originator
**As a** responder
**I want to** send a quick message to the person in need
**So that** I can coordinate or ask questions

**Acceptance Criteria:**
- In-app messaging interface
- Pre-written quick messages (e.g., "On my way", "What's your apartment number?")
- Free-form text messaging
- Messages delivered in real-time

**Notes:** Defer to post-MVP, use SMS/phone for now

---

#### 🟢 US-R22: See Other Responders
**As a** responder en route
**I want to** see if other neighbors are also responding
**So that** we can coordinate

**Acceptance Criteria:**
- List of other responders who accepted alert
- Show their names and ETA
- Show their capabilities

**Notes:** Multi-responder coordination, post-MVP

---

### 1.6 Profile & Settings

#### 🔴 US-R23: Edit Profile
**As a** responder
**I want to** update my profile information
**So that** my details stay current

**Acceptance Criteria:**
- User can edit name, photo, address
- User can change phone number (requires re-verification)
- User can save changes
- Changes reflected immediately

**Notes:** Basic CRUD for profile

---

#### 🔴 US-R24: Manage Capabilities
**As a** responder
**I want to** add or remove capabilities
**So that** I can respond to more or fewer emergency types

**Acceptance Criteria:**
- User can view current capabilities
- User can check/uncheck additional capabilities
- For capabilities requiring training (post-MVP), user must complete training
- User can save changes

**Notes:** Checkbox interface, no training gates for MVP

---

#### 🟡 US-R25: View Response History
**As a** responder
**I want to** see my past responses
**So that** I can track how I've helped

**Acceptance Criteria:**
- List of emergencies user responded to
- Show date, emergency type, outcome
- Show response time stats

**Notes:** Post-MVP analytics

---

#### 🟢 US-R26: Manage Notifications
**As a** responder
**I want to** control notification preferences
**So that** I can customize how I'm alerted

**Acceptance Criteria:**
- Toggle for push notifications
- Toggle for SMS fallback
- Toggle for bypassing Do Not Disturb
- Sound/vibration settings

**Notes:** Defer to post-MVP, default settings OK for MVP

---

## 2. Alert Originator Stories

### 2.1 Creating Alerts

#### 🔴 US-O1: Sign Up to Request Help
**As a** community member in need
**I want to** sign up for Neighbor911
**So that** I can request help from neighbors

**Acceptance Criteria:**
- Same registration flow as responders (US-R1, US-R2)
- Optional: Mark account as "alert originator only" (not a responder)

**Notes:** Same app, different role/usage

---

#### 🔴 US-O2: Create Emergency Alert
**As a** person in an emergency
**I want to** create an alert to request help from neighbors
**So that** someone can come assist me

**Acceptance Criteria:**
- User sees large "REQUEST HELP" button on home screen
- Tapping button opens emergency type selector
- User selects emergency type from list
- User sees location (auto-detected via GPS)
- User can adjust location on map if needed
- User can add optional description (text field)
- User can tap "SEND ALERT" to dispatch

**Notes:** Quick, simple interface for emergency situations

---

#### 🔴 US-O3: Add Emergency Description
**As an** alert originator
**I want to** add details about my emergency
**So that** responders know what to expect

**Acceptance Criteria:**
- Optional text field for description (max 200 chars)
- Pre-filled suggestions based on emergency type
- User can skip description if urgent

**Examples:**
- "60s male, unconscious, not breathing"
- "Apartment 4B, door is unlocked"
- "Heard a safeword called, please knock"

**Notes:** Keep brief and optional

---

#### 🔴 US-O4: Confirm Alert Location
**As an** alert originator
**I want to** verify my location is accurate
**So that** responders can find me

**Acceptance Criteria:**
- Location auto-detected via GPS
- User sees location on map with pin
- User can drag pin to adjust location
- User can enter address manually
- User confirms location before sending

**Notes:** GPS + manual override

---

#### 🟡 US-O5: Trigger Alert from Safeword App
**As a** Safeword app user
**I want to** trigger a Neighbor911 alert from within Safeword
**So that** I can get help during a consent emergency

**Acceptance Criteria:**
- Safeword app has integration with Neighbor911
- User can trigger "Bedroom Consent Emergency" alert from Safeword
- Alert includes context from Safeword (if permitted)
- Responders receive alert via Neighbor911

**Notes:** Phase 2, requires Safeword integration

---

### 2.2 Managing Alerts

#### 🔴 US-O6: See Alert Status
**As an** alert originator
**I want to** see the status of my alert
**So that** I know if help is coming

**Acceptance Criteria:**
- After sending alert, user sees status screen
- Status shows:
  - "Alert sent to X nearby responders"
  - "Waiting for response..." (if no one accepted yet)
  - "Sarah is on the way - ETA 3 minutes" (when accepted)
  - Responder name, photo, ETA
- Real-time updates as status changes

**Notes:** Reassuring feedback loop

---

#### 🔴 US-O7: See Responder Arrival Updates
**As an** alert originator
**I want to** be notified when a responder is getting close
**So that** I can prepare or go to the door

**Acceptance Criteria:**
- Push notification when responder is 1 minute away
- Push notification when responder has arrived
- Status updates visible in-app

**Notes:** Automatic status updates from responder

---

#### 🔴 US-O8: Cancel Alert
**As an** alert originator
**I want to** cancel my alert if the emergency resolves
**So that** responders don't come unnecessarily

**Acceptance Criteria:**
- "Cancel Alert" button visible on status screen
- Confirmation dialog: "Are you sure? Emergency resolved?"
- Upon confirmation, alert cancelled
- All responding neighbors notified immediately
- Alert marked as "Cancelled" in system

**Notes:** Prevent false dispatches

---

#### 🟡 US-O9: Contact Responding Neighbor
**As an** alert originator
**I want to** message the responding neighbor
**So that** I can provide additional info or ask questions

**Acceptance Criteria:**
- "Message" button appears once responder accepts
- In-app messaging interface
- Real-time delivery

**Notes:** Defer to post-MVP, use phone call for now

---

#### 🔴 US-O10: Call Responding Neighbor
**As an** alert originator
**I want to** call the responding neighbor
**So that** I can speak to them directly

**Acceptance Criteria:**
- "Call Responder" button appears once responder accepts
- Tapping button initiates phone call
- Responder's phone number used

**Notes:** Simple phone call deep link

---

### 2.3 Post-Emergency

#### 🔴 US-O11: Mark Emergency Resolved
**As an** alert originator
**I want to** mark the emergency as resolved
**So that** the system knows help is no longer needed

**Acceptance Criteria:**
- "Mark Resolved" button visible on status screen
- User can tap when emergency is over
- Alert closed in system
- Responder notified

**Notes:** Can also be marked resolved by responder

---

#### 🟡 US-O12: Rate Responder
**As an** alert originator
**I want to** rate the neighbor who helped me
**So that** I can provide feedback

**Acceptance Criteria:**
- After emergency resolved, rating prompt appears
- 1-5 star rating
- Optional text feedback
- Ratings aggregated per responder

**Notes:** Post-MVP, trust-based for now

---

#### 🟡 US-O13: Thank Responder
**As an** alert originator
**I want to** send a thank you message
**So that** I can express gratitude

**Acceptance Criteria:**
- "Send Thanks" button after resolution
- Pre-written thank you options or custom message
- Message delivered to responder

**Notes:** Post-MVP, nice-to-have

---

#### 🟡 US-O14: Request Support for Cheerleading Boycott (Brain-Damaging Sport Dissuasion)
**As a** high school or college cheerleader
**I want to** call for community support to organize a cheerleading boycott at tackle football games
**So that** I can refuse to cheer for brain-damaging sports and persuade my classmates to join me

**Acceptance Criteria:**
- User can trigger "Brain-Damaging Youth Sport Dissuasion" alert
- Alert specifies: "Cheerleader organizing boycott at [School Name] tackle football game"
- Alert indicates number of cheerleaders willing to participate (1, few, many)
- Location shows school/stadium where boycott will occur
- Optional description: reason for boycott, concerns about classmates' brain safety
- 3-5 responders dispatched to provide:
  - Moral support and adult backing
  - Educational materials about CTE and brain injury
  - Help communicating with coaches, administration, parents
  - Media coordination if desired (optional)
  - Safety support if facing pressure from school/peers

**Example Alert:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🟡 BRAIN-DAMAGING SPORT DISSUASION

Type: Cheerleading Boycott
Location: Lincoln High School Stadium
Time: Friday 7pm (game time)

Requester: Sarah (age 16, varsity cheerleader)

"I'm refusing to cheer at Friday's tackle
football game. I can't support my classmates
damaging their brains. I want to organize
other cheerleaders to join me, but I need
adult support to face the coaches and admin.

Can anyone help me stand up for my friends'
brain safety?"

Target responders: 3-5 adults
- Educational background preferred
- Parents, teachers, medical professionals
- Experience with school advocacy helpful

[ACCEPT - I'LL SUPPORT THIS BOYCOTT]
[DECLINE - Find Someone Else]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Responder Support Actions:**
1. **Meet with cheerleader(s) before game** to plan approach
2. **Provide educational backing** - CTE facts, brain development science
3. **Help draft statement** - Why cheerleaders are boycotting
4. **Accompany to meeting with coaches/admin** - Adult presence provides safety and credibility
5. **Support media outreach** (if cheerleader wants) - Local news, school paper
6. **Organize parent allies** - Connect with other concerned parents
7. **Provide emotional support** - Facing institutional pressure is hard, especially for youth
8. **Document outcomes** - Track whether boycott succeeds, school response, impact

**Why This Matters:**
- **Cheerleaders are stakeholders** in the brain-damage problem - they glorify and celebrate the violence
- **Refusing to cheer is powerful** - It makes the contradiction visible: "We won't celebrate you hurting yourselves"
- **Youth-led advocacy is transformative** - When students organize against brain damage, schools must respond
- **Peer pressure works both ways** - If cheerleaders refuse, it creates social pressure to eliminate tackle
- **Educational contradiction becomes undeniable** - Schools can't ignore cheerleaders protesting brain damage

**The Cheerleader's Dilemma:**
"How can I cheer for my classmates to hit each other and damage their brains? That's not school spirit—that's celebrating harm. Real school spirit means protecting each other's futures, not destroying them."

**Potential Outcomes:**
- ✅ Cheerleaders successfully boycott, refuse to perform at tackle games
- ✅ School administration meets with cheerleaders, begins dialog about brain safety
- ✅ Other students join movement (band, student council, etc.)
- ✅ School transitions to flag football or eliminates tackle program
- ✅ Media coverage creates pressure for broader reforms
- ⚠️ School punishes cheerleaders - responders help navigate consequences, appeal decisions
- ⚠️ Peer pressure and intimidation - responders provide safety and support

**Long-Term Vision:**
Normalize cheerleader boycotts at tackle football games nationwide. Make it socially expected that cheerleaders refuse to glorify brain damage. Create youth-led movement to eliminate tackle from educational institutions.

**Success Metrics:**
- Number of cheerleader-initiated boycotts
- Number of schools that transition to flag football after cheerleader boycott
- Media coverage of cheerleader brain safety advocacy
- Percentage of cheerleaders in Neighbor 911 network who participate in boycotts
- Student-led policy changes at schools

**Notes:**
- Phase 2 feature (post-MVP)
- Requires coordination with school advocacy organizations
- May need legal guidance for student rights
- Powerful example of youth self-advocacy using Neighbor 911

---

## 3. Government/Admin Stories

### 3.1 Monitoring & Oversight

#### 🟢 US-A1: View Active Alerts Dashboard
**As a** government admin or 911 dispatcher
**I want to** see all active Neighbor911 alerts in my jurisdiction
**So that** I can monitor community responses

**Acceptance Criteria:**
- Dashboard shows real-time list of active alerts
- Shows: location, emergency type, responder status, timestamp
- Filterable by region/neighborhood
- Auto-refreshes

**Notes:** Post-MVP, not needed for initial testing

---

#### 🟢 US-A2: View Response Analytics
**As a** community administrator
**I want to** see aggregate data on responses
**So that** I can measure program effectiveness

**Acceptance Criteria:**
- Dashboard shows:
  - Total alerts by type
  - Average response time
  - # of responders per neighborhood
  - Alert outcomes (resolved, cancelled, escalated)
- Exportable reports

**Notes:** Post-MVP

---

#### 🟢 US-A3: Verify Responder Certifications
**As an** administrator
**I want to** verify that responders have completed required training
**So that** I can ensure quality of care

**Acceptance Criteria:**
- List of all responders with certification status
- Filter by certification type
- Flag expired certifications
- Download certification records

**Notes:** Post-MVP, honor system for MVP

---

#### 🟢 US-A4: Moderate Users
**As an** administrator
**I want to** suspend or remove problematic users
**So that** I can maintain community safety

**Acceptance Criteria:**
- Admin can view user reports/flags
- Admin can suspend user account
- Admin can permanently ban user
- User notified of action

**Notes:** Post-MVP

---

### 3.2 Receiving 911 Coordination

#### 🟡 US-A5: Receive 911 Dispatch Notification
**As a** 911 dispatcher
**I want to** be notified when a Neighbor911 responder is en route
**So that** I can coordinate professional EMS

**Acceptance Criteria:**
- Notification sent when responder accepts alert
- Notification includes: emergency type, location, responder name/phone, ETA
- Notification medium: SMS or email (MVP), API integration (future)

**Notes:** Phase 1.5, manual integration for MVP

---

#### 🟢 US-A6: Track Neighbor Response via CAD
**As a** 911 dispatcher
**I want to** see Neighbor911 responses in my CAD (Computer-Aided Dispatch) system
**So that** I have full situational awareness

**Acceptance Criteria:**
- API integration between Neighbor911 and CAD system
- Neighbor responses appear as "Community Assist" incidents
- Real-time status updates

**Notes:** Long-term integration, post-MVP

---

## 4. System Stories

### 4.1 Intelligent Dispatch Logic

#### 🔴 US-S1: Query Available Responders
**As the** system
**I want to** query all responders in the area when an alert is created
**So that** I know who can potentially respond

**Acceptance Criteria:**
- When alert created, system queries all users within 0.5 mile radius
- Query checks:
  - User is marked "Available Now"
  - User has selected the relevant capability
  - User has not been recently spammed with alerts
- Return list of eligible responders sorted by proximity

**Notes:** Real-time location query, no long-term storage

---

#### 🔴 US-S2: Send Alerts to Top Matches
**As the** system
**I want to** send push notifications to the best-matched responders
**So that** the alert reaches the most capable neighbors first

**Acceptance Criteria:**
- Send push notification to top 5 closest eligible responders simultaneously
- Include: emergency type, distance, description
- Track who was notified and when

**Notes:** Parallel dispatch to 5 users for MVP

---

#### 🔴 US-S3: Handle First Acceptance
**As the** system
**I want to** stop dispatching when a responder accepts
**So that** we don't over-dispatch

**Acceptance Criteria:**
- When first responder accepts, mark alert as "Claimed"
- Stop sending to new responders
- Dismiss alert from other notified responders' screens
- Notify alert originator

**Notes:** Single responder for MVP (can expand to 2-3 later)

---

#### 🔴 US-S4: Escalate on No Response
**As the** system
**I want to** escalate to more responders if no one accepts
**So that** the emergency doesn't go unanswered

**Acceptance Criteria:**
- If all initial 5 responders decline or timeout, send to next 5
- Repeat escalation up to 3 tiers (15 total responders)
- If still no response, re-alert all declined responders with urgent message

**Notes:** Tiered escalation

---

#### 🟡 US-S5: Filter by Equipment Availability
**As the** system
**I want to** only dispatch equipment-based alerts to responders who have the equipment
**So that** we don't send AED alerts to people without AEDs

**Acceptance Criteria:**
- For AED alerts, only dispatch to responders who verified AED ownership
- For naloxone alerts, only dispatch to responders with naloxone
- For general alerts (wellness check), dispatch to all

**Notes:** Post-MVP when equipment verification added

---

### 4.2 Privacy & Security

#### 🔴 US-S6: Store Minimal Location Data
**As the** system
**I want to** avoid storing long-term location data
**So that** user privacy is protected

**Acceptance Criteria:**
- Home address stored only as city/neighborhood, not exact coordinates (or encrypted)
- Real-time location queried only during active alerts
- Location data not logged or retained after alert resolved
- Comply with privacy regulations

**Notes:** Privacy-first architecture

---

#### 🔴 US-S7: Encrypt Sensitive Data
**As the** system
**I want to** encrypt user data at rest and in transit
**So that** personal information is protected

**Acceptance Criteria:**
- All API calls use HTTPS
- User data encrypted in database
- Messages encrypted end-to-end (if implemented)

**Notes:** Firebase handles most of this automatically

---

#### 🟡 US-S8: Implement Rate Limiting
**As the** system
**I want to** prevent users from spamming alerts
**So that** the system isn't abused

**Acceptance Criteria:**
- Max 3 alerts per user per day
- Max 1 alert per user per hour
- Admin can override limits
- User notified if limit reached

**Notes:** Anti-spam measure, post-MVP

---

### 4.3 Notifications & Reliability

#### 🔴 US-S9: Deliver Push Notifications Reliably
**As the** system
**I want to** ensure push notifications are delivered within 3 seconds
**So that** responders are alerted quickly

**Acceptance Criteria:**
- Push notifications sent via Firebase Cloud Messaging (FCM)
- 95%+ delivery rate within 3 seconds
- Fallback to SMS if push fails (post-MVP)
- Retry logic for failed deliveries

**Notes:** Use FCM high-priority messages

---

#### 🟡 US-S10: Send SMS Fallback
**As the** system
**I want to** send SMS if push notification fails
**So that** responders don't miss critical alerts

**Acceptance Criteria:**
- If push delivery fails or not opened within 10 seconds, send SMS
- SMS includes: emergency type, distance, link to open app
- Track SMS delivery status

**Notes:** Post-MVP, requires Twilio integration

---

---

## 1.7 Critical Value Proposition Stories

### 1.7.1 Demonstrating the Core Problem

#### 🔴 US-O15: See EMS Response Time Warning
**As an** alert originator in a cardiac arrest emergency
**I want to** see estimated professional EMS arrival time
**So that** I understand why neighbor response is critical

**Acceptance Criteria:**
- After sending cardiac arrest alert, system shows:
  ```
  Professional EMS estimated arrival: 12-15 minutes
  Survival without intervention: 3-5 minutes

  Neighbor responders can arrive in 2-3 minutes.
  They are your best chance.
  ```
- System pulls real-time EMS availability data (future) or uses regional averages (MVP)
- User sees both neighbor ETA and professional EMS ETA side-by-side
- Message emphasizes time gap between professional help and survival window
- Reinforces that neighbors complement (not replace) professional EMS

**Notes:** This story explicitly captures the **time-is-life gap** that justifies the entire app

---

#### 🔴 US-O16: Experience No Responder Availability
**As an** alert originator in a life-threatening emergency
**I want to** know if no neighbors are available
**So that** I can immediately call 911 myself or take other action

**Acceptance Criteria:**
- **For life-threatening emergencies requiring EMS (cardiac arrest, overdose, fire, severe medical, drowning):**
  - System shows **immediately upon alert creation** (before even dispatching to neighbors):
    ```
    Alert sent to nearby responders.

    ⚠️ CALL 911 NOW ⚠️

    Professional EMS is essential for this emergency.
    Neighbors will help until they arrive.
    ```
  - If first wave (5 responders) all decline/timeout within 30 seconds, originator sees:
    ```
    No nearby responders available yet.
    Searching wider area...

    Have you called 911?
    Professional help is critical.

    [TAP HERE TO CALL 911]
    ```
  - If second wave also fails, system shows:
    ```
    Still searching for responders.

    If you haven't called 911, DO IT NOW.

    While waiting:
    - If you know CPR, start now
    - If you have an AED nearby, retrieve it
    - Stay on the line with 911 dispatcher

    [CALL 911]
    ```
- **For non-life-threatening emergencies (wellness check, quit companion, bystander witness, companionship):**
  - System does NOT prompt to call 911
  - If no responders available, shows:
    ```
    No nearby responders available right now.
    Searching wider area...

    We're doing our best to find someone to help.
    ```
  - If still no responders after extended search:
    ```
    We couldn't find a neighbor responder.

    Other options:
    - Call a friend or family member
    - Contact a crisis hotline (if applicable)
    - Try again later when more neighbors are available
    ```
- **For preventive/advocacy emergencies (brain-damaging sport dissuasion):**
  - System does NOT prompt to call 911 (not a socially conventional emergency response situation yet)
  - If no responders available:
    ```
    No nearby responders available right now.

    This type of intervention works best with
    community support. Try again during peak hours
    (evenings/weekends) when more neighbors are
    available.

    In the meantime, consider:
    - Researching talking points about CTE/brain injury
    - Connecting with advocacy organizations
    - Planning your approach for when responders are available
    ```
- System never leaves originator in silence - constant status updates every 15 seconds
- Fallback instructions appear based on emergency type and severity
- Clear hierarchy: 911 for life-threatening, neighbor support for non-life-threatening, advocacy coordination for preventive

**Notes:** Demonstrates the app **doesn't replace 911**, it supplements it. **911 should always be called immediately and first for life-threatening emergencies where EMS is needed.** Shows worst-case scenario handling. Recognizes that not all alert types have conventional emergency response protocols yet (e.g., youth sports brain damage prevention is advocacy work, not emergency services work).

---

### 1.7.2 Proving Measurable Impact

#### 🔴 US-O17: Receive Outcome Report After Emergency
**As an** alert originator whose life was saved
**I want to** see how much faster neighbor response was vs. EMS
**So that** I understand the impact of this community network

**Acceptance Criteria:**
- After emergency resolved, originator receives summary:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  YOUR EMERGENCY SUMMARY

  Neighbor arrived: 2 min 15 sec
  Professional EMS arrived: 14 min 30 sec

  Time advantage: 12 minutes, 15 seconds

  For cardiac arrest, every minute without CPR
  reduces survival chance by 10%. Your neighbor's
  rapid response likely saved your life.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Summary includes:
  - Responder name(s) and response time(s)
  - Professional EMS arrival time
  - Time advantage calculation
  - Emergency-type-specific survival statistics
- Optional: Share summary on social media to encourage others to join
- One-tap "Thank Responder" button
- Data collected for aggregate impact metrics

**Notes:** Captures the **quantifiable value** of the network. Hard data = proof of concept.

**Priority:** 🔴 P0 - Critical for demonstrating ROI and encouraging network growth

---

### 1.7.3 Network Discovery & Hidden Resources

#### 🟡 US-R27: Discover Nearby Equipment I Didn't Know Existed
**As a** user who just had a medical emergency
**I want to** learn which neighbors have life-saving equipment
**So that** I understand the hidden resources in my community

**Acceptance Criteria:**
- After emergency resolved, system shows map visualization:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  LIFE-SAVING EQUIPMENT NEAR YOU
  Within 0.5 miles:

  🏥 3 AEDs (neighbors have them at home)
  💊 7 Naloxone kits
  🔥 2 Fire extinguishers (large)
  🩹 5 Advanced first aid kits
  🚑 2 CPR-certified nurses (off-duty)

  You live in a resource-rich community you
  didn't know about.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Map shows anonymized density (no specific addresses unless responder consents)
- User sees they're surrounded by capability
- Call-to-action: "You could be next door to someone who needs you. Add your skills?"
- Link to responder onboarding

**Notes:** Shows **hidden network effects** - equipment density that's invisible until you need it.

**Priority:** 🟡 P1 - Powerful for user education and responder recruitment

---

### 1.7.4 Safety & Trust

#### 🟡 US-O18: Cancel False Alarm Without Penalty
**As an** alert originator who accidentally triggered an alert
**I want to** cancel it immediately without consequences
**So that** I'm not afraid to use the system when I really need it

**Acceptance Criteria:**
- User can cancel alert within 60 seconds with one tap, no questions asked
- Immediate cancellation screen:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Alert Cancelled

  No problem! Better safe than sorry.
  We'd rather you call for help and not
  need it than hesitate when you do.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Responders notified: "Alert cancelled - False alarm. No worries!"
- System tracks false alarm rate per user, but uses graduated response:
  - **First 3 false alarms:** No penalty, just gentle reminder about appropriate use
  - **4-6 false alarms:** Warning message: "We've noticed several cancelled alerts. Need help understanding when to use this?"
  - **7+ false alarms:** Required to watch "Appropriate Use" training video (2 min)
  - **Extreme abuse (20+):** Account review, possible suspension
- Compassionate messaging emphasizes safety over punishment

**Notes:** Removes **psychological barrier to calling for help**. People hesitate if they fear consequences. Same philosophy as "Good Samaritan" laws.

**Priority:** 🟡 P1 - Critical for user trust and actual emergency usage

---

### 1.7.5 Accessibility & Inclusion

#### 🟡 US-R28: Suspend Receiving Alerts While Still Able to Send
**As an** 82-year-old or recently hospitalized user
**I want to** stop receiving alerts I can't respond to
**So that** I'm not burdened, but can still call for help myself

**Acceptance Criteria:**
- User age 80+ sees option during onboarding or in settings:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ALERT PREFERENCES (Age 80+)

  You've earned the right to rest.

  ☐ Disable all inbound alerts
     (You can still REQUEST HELP anytime)

  ☑ Gentle life-threatening alerts only
     (Soft notifications during waking hours)

  Why we offer this:
  Your safety matters. We don't want to
  burden you with requests you can't fulfill.
  But we'll always be here if YOU need us.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Users who select "Hospitalized" or "Immobilized" in temporary suspension menu see:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  TEMPORARY SUSPENSION ACTIVE

  You won't receive alerts while you're recovering.

  But you can still REQUEST HELP anytime.
  We're here for you.

  [Resume Responding When Ready]
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Home screen shows badge: "Receive-only mode: You can send alerts but won't receive them"
- User can re-enable receiving alerts with one tap when ready
- No stigma or loss of access - just compassionate adjustment

**Notes:** Shows **aging in place support** and **bidirectional safety net**. Elderly/injured users remain part of the community as potential beneficiaries.

**Priority:** 🟡 P1 - Critical for inclusive design and elder care

---

#### 🟢 US-R29: Respond as a Teenage Capable Helper
**As a** 16-year-old with CPR certification
**I want to** respond to emergencies just like adult responders
**So that** I can use my skills to help my community

**Acceptance Criteria:**
- Users age 13-17 can sign up as responders
- Parent/guardian consent required during onboarding (MVP: checkbox attestation, Future: verified consent)
- Can select all Tier 1 capabilities (wellness check, quit companion, bystander witness, 911 coordination)
- Can select Tier 2-3 if certified (CPR, AED, naloxone) - same certification requirements as adults
- Alert dispatch doesn't discriminate by age - certified youth get same alerts as adults
- After first successful response, youth sees:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🌟 FIRST RESPONSE COMPLETE 🌟

  You just proved that age doesn't determine
  capability. You're a hero.

  Your community is safer because you
  recognized your power to help.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Badge: "Youth Responder" (displayed with pride, not as limitation)

**Notes:** Captures **intergenerational mutual aid** and empowers youth sufferers (age 13+) who recognize emergencies. Aligns with "from suffering to suffrage" philosophy - if you can recognize community emergencies, you're civically aware.

**Priority:** 🟢 P2 - Powerful for youth empowerment, but not critical for MVP

---

### 1.7.6 Community Building & Retention

#### 🟢 US-R30: Join Community Safety Network as New Resident
**As** someone who just moved to a new neighborhood
**I want to** immediately connect with neighbors who can help me
**So that** I feel safe even though I don't know anyone yet

**Acceptance Criteria:**
- During onboarding, system detects new user in area (new address in database)
- Welcome screen after registration:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  WELCOME TO LINCOLN PARK!

  You're joining a community of 47 nearby
  responders ready to help you.

  Within 0.25 miles of your new home:

  🏥 12 people with CPR training
  💊 3 AEDs
  💪 8 quit companions
  🚪 23 wellness check volunteers
  🔥 4 fire response trained

  You're not alone anymore.

  [Explore My Neighborhood]
  [Add My Skills to Help Others]
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- User can see anonymized density heatmap (no names/addresses, just general coverage)
- Encourages: "Add your skills to help others too"
- Optional: "Introduce yourself" feature (post-MVP) - send non-emergency greeting to nearby responders

**Notes:** Shows **instant community integration** for transient populations (renters, transplants, remote workers). Addresses modern loneliness epidemic while building safety network.

**Priority:** 🟢 P2 - Great for retention and community feel, not critical for life-saving MVP

---

#### 🟡 US-R31: Adjust Availability After Feeling Overwhelmed
**As a** responder who's been getting too many alerts
**I want to** reduce my alert frequency without fully opting out
**So that** I can stay in the network without burning out

**Acceptance Criteria:**
- **Alert categorization for burnout tracking:**
  - **CRITICAL (never count toward burnout threshold - always sent to opted-in responders):**
    - 🔴 Life-threatening medical: CPR, AED, overdose, fire, drowning, severe medical emergency
    - 🟠 Safety/security: Bedroom consent emergency, domestic violence, sexual assault, active bystander witness (for abuse/assault situations), missing child (suspected abduction)
    - 🟡 Time-sensitive critical: Wellness check (potential medical emergency/suicide), missing child (lost/wandering)
  - **NON-CRITICAL (count toward weekly limit, default 5/week):**
    - 🟢 Companionship/support: Quit companion, loneliness/companionship, mental health crisis (non-suicidal)
    - 🟡 Preventive/advocacy: Brain-damaging youth sport dissuasion, missing pet
    - 🟢 Coordination: 911 coordination (if standalone, not part of critical emergency)
- **IMPORTANT:** Users must **opt-in to each capability** during onboarding/settings. You only receive alerts for capabilities you've selected.
- **Weekly limit customization:**
  - Default limit for non-critical alerts: **5 per week**
  - During capability selection, users can set their own limit per capability:
    ```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    SELECT YOUR CAPABILITIES

    ☑ Quit Companion
       How many per week? [1] [2] [3] [5] [No limit]
       Currently: 5/week (default)

    ☑ Brain-Damaging Sport Dissuasion
       How many per week? [1] [2] [3] [5] [No limit]
       Currently: 5/week (default)

    ☐ Companionship / Loneliness Support
       How many per week? [1] [2] [3] [5] [No limit]
       Not selected

    ☑ Wellness Check (Critical - no weekly limit)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ```
  - Users can set **different limits for each non-critical capability**
  - Users can set limits **lower than 5/week** (e.g., "I'll do 2 quit companion requests per week max")
  - Combined total across all non-critical capabilities still tracked
- **Automatic cutback trigger:** When user receives **their chosen weekly limit** of non-critical alerts (default 5 total across all non-critical types)
- **When user hits their weekly limit for non-critical alerts, system shows:**
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  👋 YOU'VE HIT YOUR WEEKLY LIMIT

  You've received your limit of non-critical
  alerts this week (5 total).
  That's already a lot of community service!

  This week you responded to:
  - 3 quit companion requests
  - 2 brain-damaging sport dissuasion calls

  To prevent burnout, we'll pause your
  non-critical alerts until next week.

  You'll still receive:
  ✓ Life-threatening emergencies (CPR, AED, overdose, fire)
  ✓ Safety/security (consent conflicts, DV, SA, bystander)
  ✓ Critical wellness checks (potential suicide/medical)

  Paused until next week:
  ⏸ Quit companion requests
  ⏸ Companionship/loneliness support
  ⏸ Brain-damaging sport dissuasion
  ⏸ Missing pet searches

  Want to keep receiving more this week?
  [YES - I CAN HANDLE MORE]  [NO - PAUSE UNTIL NEXT WEEK]
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- **If user selects "YES - I CAN HANDLE MORE":**
  - System asks: "How many more non-critical alerts are you willing to receive this week?"
    - Options: 1-2 more, 3-5 more, 6-10 more, No limit
  - System tracks new threshold and pauses when reached
  - Next week, resets to default 5-alert threshold (not the custom number)
- **If user selects "NO - PAUSE UNTIL NEXT WEEK":**
  - Non-critical alerts paused for remainder of week
  - Critical alerts continue normally
  - User can manually resume anytime in settings
- **Proactive check-in after high-volume weeks:**
  - If user receives 3+ alerts in one day (any type), system shows gentle message:
    ```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    👋 BUSY DAY, HUH?

    You've responded to 3 emergencies today.
    You're a hero, but don't forget to care
    for yourself too.

    Your wellbeing matters. Options:

    ☐ I'm fine, keep current settings

    ☐ Reduce to life/safety critical only
       (Medical emergencies + safety/security alerts)

    ☐ Set stricter schedule
       (Evenings/weekends only)

    ☐ Take a break
       (Suspend for 2 weeks, resume automatically)

    No judgment either way. We want you to
    stay engaged long-term, not burn out.
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ```
- System tracks responder fatigue metrics and suggests adjustments proactively
- Users who reduce availability aren't penalized - no negative badges or messages
- If user selects "Take a break," system auto-resumes after specified time with gentle notification:
  ```
  Your 2-week break is over. Ready to help
  neighbors again?

  [Yes, I'm Back]  [Extend Break 2 More Weeks]
  ```
- **Weekly reset:** Every Monday at 12:01 AM, non-critical alert counter resets to 0 for all users

**Notes:** Demonstrates **long-term sustainability** thinking. Prevents churn. Respects that helpers need help too. **5 non-critical alerts per week is the sustainable default, but users can set it lower (or higher) based on their capacity.**

**Key principle:** Users must **opt-in to each capability** - you don't receive quit companion alerts unless you've selected that capability. For non-critical capabilities, you can set your own weekly limit during selection (1, 2, 3, 5, or no limit).

Critical alerts include not just life-threatening medical emergencies, but also **consent conflicts, domestic violence, sexual assault, and wellness checks** (which could be suicide prevention or undiscovered medical emergencies). These always get through to opted-in responders regardless of weekly limits.

Non-critical includes preventive work (sports brain damage dissuasion), support work (quit companion, companionship), and search work (missing pets). These respect weekly limits. We don't want volunteer work to become a part-time job.

**Priority:** 🟡 P1 - Critical for responder retention and network health

**Rationale:** Consent emergencies, DV, SA, and critical wellness checks are time-sensitive safety situations that need immediate response. They don't count toward burnout limits. But asking volunteers to do 5+ quit companion calls or brain damage dissuasion interventions per week is asking a lot of preventive/support work. We protect our responder community from exhaustion on non-critical work while ensuring all safety-critical situations get immediate help. Letting users set their own limits (including lower than 5/week) respects individual capacity differences.

---

### 1.7.7 Institutional Legitimacy

#### 🟡 US-A7: Receive Neighbor Response Notification as 911 Dispatcher
**As a** 911 dispatcher receiving a cardiac arrest call
**I want to** know if a neighbor with an AED is already en route
**So that** I can coordinate EMS arrival and update caller

**Acceptance Criteria:**
- When responder accepts alert, system sends notification to 911 dispatch (method depends on integration level):
  - **MVP:** Email to dispatch center (pre-configured address)
  - **Phase 1.5:** SMS to dispatch center
  - **Phase 2:** API integration with CAD system
- Notification format:
  ```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  NEIGHBOR RESPONSE ACTIVE

  Address: 123 Main St, Apt 4B
  Emergency Type: Cardiac arrest

  Responder: Mike S. (555-0123)
  Equipment: AED, CPR certified
  Cert Date: 2024-11-01
  ETA: 2 minutes

  Professional EMS still needed.
  Responder will provide aid until EMS arrives.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ```
- Dispatcher can see:
  - Responder's ETA vs. EMS ETA
  - Equipment/certification status
  - Contact info for direct coordination (if needed)
- Dispatcher knows not to tell caller "no one is coming" - neighbor is already responding
- Update sent when responder arrives: "Neighbor on-scene, EMS still en route"
- Final update when EMS arrives and takes over

**Notes:** Shows **collaboration, not competition** with professional services. Gets institutional support. Critical for preventing 911 centers from seeing this as vigilante interference.

**Priority:** 🟡 P1 - Essential for institutional partnerships and avoiding regulatory pushback

---

## 5. Detailed Use Cases

### Use Case 1: Wellness Check Response (End-to-End)

**Primary Actor:** Sarah (Responder)
**Secondary Actor:** John (Alert Originator)
**Trigger:** John creates a wellness check alert for his elderly neighbor

**Preconditions:**
- Sarah is signed up, has selected "Wellness Check" capability, and is marked "Available Now"
- John is signed up and has permission to send alerts
- Both are within 0.5 miles of each other

**Main Flow:**

1. **John creates alert**
   - John opens Neighbor911 app
   - Taps "REQUEST HELP" button
   - Selects "Wellness Check / Door Knock"
   - Confirms location: "Mrs. Johnson, Apt 3A, 123 Main St"
   - Adds description: "Haven't seen her in 3 days, not answering door"
   - Taps "SEND ALERT"
   - System shows: "Alert sent to 5 nearby responders"

2. **System dispatches**
   - System queries users within 0.5 miles with "Wellness Check" capability who are "Available Now"
   - Finds 8 eligible responders, sorted by distance
   - Sends push notification to closest 5: Sarah, Mike, Lisa, Tom, Emma

3. **Sarah receives alert**
   - Sarah's phone vibrates and plays loud alert sound (bypasses DND)
   - Push notification shows: "🚨 WELLNESS CHECK NEEDED - 0.2 miles away"
   - Sarah taps notification, app opens to alert detail screen
   - Screen shows:
     ```
     🚨 WELLNESS CHECK NEEDED

     DISTANCE: 0.2 miles (3 min walk)
     PERSON: Elderly neighbor, Apt 3A
     DETAILS: Haven't seen her in 3 days, not answering door

     ⏱️ Respond in: 00:27

     [ACCEPT - I'M ON MY WAY]
     [DECLINE - Find Someone Else]
     ```

4. **Sarah accepts**
   - Sarah reads the details and decides to help
   - Taps "ACCEPT - I'M ON MY WAY" at 12 seconds
   - System dismisses alert from Mike, Lisa, Tom, Emma's screens
   - System notifies John: "Sarah is on her way - ETA 3 minutes"
   - Sarah sees navigation screen

5. **Sarah navigates**
   - Screen shows "Start Navigation" button
   - Sarah taps button, Google Maps opens with destination: 123 Main St
   - Sarah starts walking
   - App shows status buttons: "On My Way" (auto-selected), "Arrived", "Emergency Resolved"

6. **Sarah arrives**
   - Sarah reaches the building (GPS shows within 50 meters)
   - App switches to Bluetooth homing mode
   - Screen shows:
     ```
     ↑ 75 feet

     [Directional Arrow pointing up-right]

     KEEP GOING - 3RD FLOOR
     ```
   - Sarah goes to Apt 3A

7. **Sarah performs wellness check**
   - App detects proximity < 10 feet (via Bluetooth)
   - Screen shows "YOU'VE ARRIVED" with tips:
     ```
     ✓ YOU'VE ARRIVED

     Quick Tips for Wellness Check:
       1. Knock firmly on door
       2. Identify yourself as a neighbor
       3. Ask "Are you okay? Do you need help?"
       4. Call 911 if unresponsive or injured
       5. Stay with them until help arrives

     [CALL 911]
     [MARK EMERGENCY RESOLVED]
     ```
   - Sarah knocks on door, identifies herself
   - Mrs. Johnson answers—she's okay, just had the flu
   - Sarah chats briefly, offers to help with groceries

8. **Sarah resolves alert**
   - Sarah taps "MARK EMERGENCY RESOLVED"
   - System notifies John: "Emergency resolved by Sarah"
   - John receives notification: "Mrs. Johnson is okay! Sarah checked on her."
   - John can optionally send thanks to Sarah (post-MVP feature)

9. **Post-resolution**
   - Sarah returns home, toggles availability back ON
   - Sarah's response recorded in system (for future analytics)
   - Mrs. Johnson grateful for caring neighbor

**Postconditions:**
- Alert marked as "Resolved" in system
- John and Sarah both notified of resolution
- Sarah's response time logged: 3 minutes

**Alternative Flows:**

**A1: Sarah declines**
- At step 4, Sarah taps "DECLINE"
- System immediately sends alert to next responder (Mike)
- Mike accepts within 15 seconds, continues from step 5

**A2: Sarah times out**
- At step 4, Sarah doesn't respond within 30 seconds
- Alert auto-dismissed from Sarah's screen
- System sends to next responder (Mike)

**A3: No one accepts**
- All 5 initial responders decline or timeout
- System sends to next 5 responders
- If still no response, re-alerts Sarah with urgent message:
  ```
  🚨 NO OTHER RESPONDERS AVAILABLE 🚨

  We couldn't find anyone else to respond.
  You may be Mrs. Johnson's only chance.

  [PLEASE ACCEPT - I'LL GO]
  [FINAL DECLINE]
  ```

**A4: Mrs. Johnson doesn't answer**
- At step 7, Sarah knocks but no answer
- Sarah taps "CALL 911" button
- Phone dialer opens with 911 pre-dialed
- Sarah calls 911, reports possible medical emergency
- Sarah waits outside apartment for EMS
- Sarah taps status button: "EMS Called - Waiting"

**A5: Alert cancelled before Sarah arrives**
- At step 6, John realizes Mrs. Johnson just returned home
- John taps "Cancel Alert"
- Sarah receives notification: "Alert cancelled - Emergency resolved"
- Sarah can return home

---

### Use Case 2: Cardiac Arrest with AED (High-Stakes Emergency)

**Primary Actor:** Mike (Responder with AED)
**Secondary Actor:** Linda (Alert Originator, calling for her husband Tom)
**Trigger:** Tom collapses from cardiac arrest

**Preconditions:**
- Mike is certified in CPR and owns an AED, marked "Available Now"
- Linda's husband Tom has collapsed, not breathing
- Linda has Neighbor911 app installed

**Main Flow:**

1. **Linda creates urgent alert**
   - Tom collapses at home, not breathing
   - Linda opens Neighbor911, taps "REQUEST HELP"
   - Selects "CPR / Cardiac Arrest"
   - Confirms location (auto-detected)
   - Adds description: "Husband collapsed, not breathing, 62 years old"
   - Taps "SEND ALERT"
   - System: "Alert sent. CALL 911 NOW."
   - Linda calls 911 on her phone while waiting

2. **System dispatches to AED owners**
   - System queries users within 1 mile with "CPR" and "AED Delivery" capabilities
   - Finds 3 eligible responders with AEDs: Mike (0.3 mi), Sarah (0.7 mi), Tom (1.2 mi)
   - Sends alerts to all 3 simultaneously (high-priority emergency)

3. **Mike receives alert**
   - Mike's phone buzzes intensely with alert:
     ```
     🚨🚨🚨 CARDIAC ARREST 🚨🚨🚨

     CRITICAL - AED NEEDED
     DISTANCE: 0.3 miles (2 min drive)
     PERSON: Male, 62, not breathing

     ⏱️ Respond in: 00:28

     [ACCEPT - I'M ON MY WAY]
     [DECLINE]
     ```

4. **Mike accepts immediately**
   - Mike reads "cardiac arrest" and taps ACCEPT within 5 seconds
   - Screen shows equipment reminder:
     ```
     ✓ Alert Accepted

     Remember to grab:
       🏥 YOUR AED
       📱 Your phone

     [START NAVIGATION]
     ```
   - Mike grabs AED from hallway closet, runs to car

5. **Mike navigates urgently**
   - Taps "START NAVIGATION", Google Maps opens
   - Mike drives toward location, ETA 2 minutes
   - App auto-updates Linda: "Mike is on the way with AED - ETA 2 min"
   - Linda continues CPR (911 dispatcher instructing her over phone)

6. **Mike arrives**
   - Mike parks, runs toward building with AED
   - App shows Bluetooth homing: "↑ 50 feet - KEEP GOING"
   - Mike enters building, follows arrow to apartment
   - App: "YOU'VE ARRIVED"

7. **Mike performs CPR + AED**
   - App shows critical tips:
     ```
     ✓ YOU'VE ARRIVED

     CPR + AED Protocol:
       1. Check responsiveness - shout "Are you OK?"
       2. Start chest compressions (100-120/min)
       3. Open AED, turn it on
       4. Apply pads to bare chest as shown
       5. STAND BACK when AED analyzes
       6. Press SHOCK button if advised
       7. Continue CPR until EMS arrives

     [VIEW FULL PROTOCOL]
     [CALL 911]
     ```
   - Mike relieves Linda from CPR
   - Mike applies AED pads, delivers shock
   - Tom's heart restarts, begins breathing
   - Mike continues monitoring until EMS arrives (4 minutes later)

8. **EMS arrives**
   - EMS takes over from Mike
   - Mike provides brief handoff: "Cardiac arrest, 1 shock delivered, patient breathing now"
   - EMS transports Tom to hospital
   - Mike taps "MARK EMERGENCY RESOLVED"
   - System notifies Linda (who went with Tom in ambulance)

9. **Post-emergency**
   - Linda sends thanks to Mike from hospital (next day)
   - Tom survives because of rapid neighbor + AED response
   - Mike's response time: 2 min 15 sec from alert to arrival
   - **Mike saved Tom's life**

**Postconditions:**
- Alert resolved
- Patient survived due to rapid AED response
- EMS handoff completed
- Response logged in system

**Alternative Flows:**

**A1: No AED owners available**
- System finds no nearby AED owners
- Sends alert to all CPR-certified responders (without AED)
- Also sends alert to general "911 Coordination" responders to call 911
- Responders do CPR until EMS arrives

**A2: Mike can't find AED**
- At step 4, Mike realizes AED battery is dead
- Mike taps "DECLINE - Equipment Issue"
- System immediately dispatches to Sarah (next closest AED owner)

**A3: Patient already deceased**
- At step 7, Mike finds Tom has been down too long, no pulse, rigor mortis
- Mike does not start CPR (obviously deceased)
- Mike taps "CALL 911" to report
- Mike waits for police/EMS
- Mike updates status: "Patient Deceased - EMS Called"

---

### Use Case 3: Bedroom Consent Emergency (Safeword Alert)

**Primary Actor:** Alex (Responder, Active Bystander)
**Secondary Actor:** Jamie (Alert Originator via Safeword app)
**Trigger:** Jamie calls safeword during intimate encounter

**Preconditions:**
- Alex is signed up, selected "Active Bystander Witness" and "Bedroom Consent Emergency" capabilities
- Jamie has Safeword app integrated with Neighbor911
- Both in same apartment building

**Main Flow:**

1. **Jamie triggers safeword alert**
   - Jamie is in bedroom with date, situation becomes non-consensual
   - Jamie says safeword "Red"
   - Safeword app detects safeword via voice recognition
   - Safeword app triggers Neighbor911 alert automatically
   - Alert type: "Bedroom Consent Emergency"
   - Location: Jamie's apartment (from Safeword app)

2. **System dispatches discreetly**
   - System queries users with "Bedroom Consent Emergency" or "Active Bystander" capability
   - Finds 6 eligible responders in building
   - Sends to closest 3: Alex, Casey, Riley
   - Alert marked "SENSITIVE - Discretion Required"

3. **Alex receives alert**
   - Push notification:
     ```
     🚨 BEDROOM CONSENT EMERGENCY

     DISTANCE: Same building, Apt 7C
     PERSON: Neighbor needs witness presence

     ⏱️ Respond in: 00:30

     [ACCEPT - I'LL GO]
     [DECLINE]
     ```

4. **Alex accepts**
   - Alex taps ACCEPT within 8 seconds
   - Screen shows guidance:
     ```
     ✓ Alert Accepted

     Important Reminders:
       • Your presence alone can de-escalate
       • You don't need to confront or fight
       • Just knock and be a witness
       • Call police if needed

     [START NAVIGATION]
     ```

5. **Alex navigates**
   - Alex walks to Apt 7C (same floor, 30 seconds away)
   - App shows Bluetooth homing: "← 20 feet"

6. **Alex performs bystander intervention**
   - App shows arrival tips:
     ```
     ✓ YOU'VE ARRIVED

     Active Bystander Protocol:
       1. Knock firmly on door
       2. Identify yourself: "Hi, I'm a neighbor"
       3. Ask: "Is everything okay in there?"
       4. Your presence is powerful - just being there helps
       5. Call 911 if you hear violence or distress

     [CALL 911]
     [CALL POLICE (Non-Emergency)]
     ```
   - Alex knocks on door: "Hey, it's Alex from down the hall, just checking if everything's okay?"
   - Jamie's date hears the knock, immediately backs off
   - Jamie comes to door: "I'm okay now, thank you so much"
   - Alex asks: "Do you want me to stay? Want me to call anyone?"
   - Jamie: "No, he's leaving now. Thank you for coming."
   - Alex waits in hallway until date leaves building

7. **Alex resolves alert**
   - Alex taps "MARK EMERGENCY RESOLVED"
   - Alex checks in with Jamie briefly: "You sure you're okay?"
   - Jamie confirms safe
   - Alex returns to apartment

8. **Post-intervention**
   - Jamie receives follow-up resources (RAINN hotline, local support)
   - Alex's intervention prevented potential assault
   - **Power of witness presence demonstrated**

**Postconditions:**
- Situation de-escalated without violence
- Jamie safe
- Perpetrator left
- No further intervention needed

**Alternative Flows:**

**A1: Situation escalated**
- At step 6, Alex hears yelling/violence through door
- Alex immediately taps "CALL 911"
- Alex stays in hallway, continues knocking, shouts: "Police are on the way!"
- Police arrive within 5 minutes
- Alex provides witness statement

**A2: No one answers door**
- At step 6, Alex knocks but no answer
- Alex calls out: "Jamie, I got your alert, are you okay?"
- Still no answer
- Alex calls 911: "Possible assault in progress, Apt 7C"
- Alex waits for police

**A3: False alarm**
- At step 6, Jamie answers: "Oh sorry, I accidentally triggered that, I'm fine"
- Alex verifies Jamie seems genuinely okay (not coerced)
- Alex marks alert as resolved
- No penalty for false alarm (better safe than sorry)

---

### Use Case 4: Overdose Response with Naloxone

**Primary Actor:** Casey (Responder with naloxone)
**Secondary Actor:** Friend of person overdosing
**Trigger:** Friend finds person unresponsive, suspects overdose

**Main Flow:**

1. **Friend creates alert**
   - Friend finds person unresponsive, shallow breathing, blue lips
   - Opens Neighbor911, taps "REQUEST HELP"
   - Selects "Overdose / Naloxone Needed"
   - Confirms location
   - Description: "Friend unresponsive, suspect opioid overdose"
   - Taps "SEND ALERT"
   - App: "Alert sent. CALL 911 NOW."
   - Friend calls 911

2. **System dispatches to naloxone owners**
   - System queries users with naloxone within 0.5 miles
   - Finds 4 responders: Casey (0.2 mi), Jordan (0.4 mi), Sam (0.6 mi), Morgan (0.8 mi)
   - Sends alert to all 4

3. **Casey receives and accepts**
   - Casey sees "OVERDOSE - NALOXONE NEEDED"
   - Accepts immediately
   - Equipment reminder: "🏥 Grab your Narcan nasal spray"
   - Casey grabs naloxone from bathroom cabinet

4. **Casey navigates and arrives**
   - Google Maps navigation, ETA 2 minutes
   - Arrives at location via Bluetooth homing

5. **Casey administers naloxone**
   - App shows instructions:
     ```
     ✓ YOU'VE ARRIVED

     Naloxone Administration:
       1. Check if person is breathing (shallow = overdose)
       2. Tilt head back, insert nasal spray
       3. Press plunger firmly - 1 spray per nostril
       4. Wait 2-3 minutes for response
       5. If no response, give 2nd dose
       6. Place in recovery position (side)
       7. Stay until EMS arrives

     [VIEW FULL PROTOCOL]
     [CALL 911]
     ```
   - Casey administers naloxone nasal spray (one spray per nostril)
   - Person wakes up within 2 minutes
   - Person confused but breathing normally
   - Casey places person in recovery position, stays with them

6. **EMS arrives**
   - EMS takes over, transports to hospital
   - Casey provides info: "Naloxone administered 5 minutes ago, 2 sprays total"
   - EMS thanks Casey
   - Casey marks alert resolved

7. **Post-emergency**
   - **Casey saved a life with $45 over-the-counter nasal spray**
   - Friend thanks Casey profusely
   - Person survives overdose

**Postconditions:**
- Person survived due to rapid naloxone administration
- EMS handoff completed
- Response time: 2 min 30 sec

---

### Use Case 5: Quit Companion - Nicotine Craving Support

**Primary Actor:** Jamie (Responder, former smoker)
**Secondary Actor:** Alex (Alert Originator, quitting tobacco)
**Trigger:** Alex has intense nicotine craving at 11pm, home alone

**Preconditions:**
- Jamie is signed up with "Addiction Support / Quit Companion" capability
- Alex is trying to quit smoking (day 4 of quit attempt)
- Both live in same apartment building
- Jamie is marked "Available Now"

**Main Flow:**

1. **Alex experiences intense craving**
   - Alex is home alone, 11pm, stressed after work call
   - Intense urge to smoke cigarettes
   - Alex knows the craving will pass in 10-15 minutes but feels overwhelming alone
   - Alex opens Neighbor911 app

2. **Alex creates quit support alert**
   - Taps "REQUEST HELP"
   - Selects "Addiction Support / Quit Companion"
   - Confirms location (auto-detected: Apt 3B)
   - Adds description: "Day 4 quitting smoking, really bad craving right now, need someone to walk with me"
   - Taps "SEND ALERT"
   - System: "Alert sent to 3 nearby quit companions"

3. **System dispatches**
   - System queries users with "Addiction Support" capability within 0.5 miles
   - Finds 3 available: Jamie (same building), Casey (2 blocks away), Sam (3 blocks away)
   - Sends alerts to all 3, prioritized by proximity

4. **Jamie receives alert**
   - Jamie's phone buzzes with notification:
     ```
     💪 QUIT SUPPORT NEEDED

     DISTANCE: Same building, Apt 3B
     TYPE: Nicotine craving - Day 4 quit
     REQUEST: Need someone to walk with me

     ⏱️ Respond in: 00:29

     [ACCEPT - I'LL HELP]
     [DECLINE]
     ```

5. **Jamie accepts immediately**
   - Jamie (who quit smoking 2 years ago) remembers how hard it was
   - Taps "ACCEPT - I'LL HELP" within 5 seconds
   - Screen shows:
     ```
     ✓ Alert Accepted

     Remember:
       • The craving will pass in 10-15 minutes
       • Distraction is key - walk, talk, stay busy
       • No judgment, just support
       • Celebrate this moment of strength

     [START NAVIGATION]
     ```

6. **Jamie navigates**
   - Jamie grabs jacket, phone, pack of gum
   - Walks down two floors to Apt 3B
   - App shows Bluetooth homing: "↓ 30 feet"
   - Arrives in 30 seconds

7. **Jamie provides quit support**
   - App shows arrival tips:
     ```
     ✓ YOU'VE ARRIVED

     Quit Companion Support:
       1. Greet warmly - "I'm proud of you for reaching out"
       2. Suggest a walk or activity
       3. Talk about anything else (distraction)
       4. Remind them why they quit
       5. Offer alternative (gum, candy, water)
       6. Stay for 15 min until craving passes
       7. No judgment if they relapse - just be there

     [VIEW CRAVING MANAGEMENT TIPS]
     [MARK CRAVING PASSED]
     ```
   - Jamie knocks on door: "Hey Alex, it's Jamie from upstairs. Wanna go for a quick walk?"
   - Alex grateful someone showed up
   - They walk around the block together for 15 minutes
   - Jamie shares: "Day 4 is brutal. I remember. You're doing amazing."
   - Jamie offers Alex some gum
   - They talk about random stuff - weekend plans, new coffee shop, anything but smoking

8. **Craving passes**
   - After 12 minutes of walking and talking, Alex realizes the craving has faded
   - Alex: "Wow, it's gone. Thank you so much, I was about to drive to the gas station."
   - Jamie: "You just saved yourself from a relapse. That takes real strength. I'm proud of you."
   - They return to building
   - Jamie: "Text me anytime. I'm on this app whenever you need."

9. **Alex marks resolved**
   - Alex taps "MARK CRAVING PASSED"
   - Optional feedback: "Jamie saved my quit. 5 stars."
   - System notifies Jamie: "Alex successfully got through the craving!"
   - Jamie taps "MISSION ACCOMPLISHED"

10. **Post-support**
    - **Alex didn't relapse because a neighbor showed up**
    - Alex goes to bed proud, quit streak intact
    - Jamie feels good about helping someone through what they went through
    - Response time: 30 seconds from alert to arrival

**Postconditions:**
- Alex successfully resisted craving, quit continues
- Alex knows they have support network
- Jamie reinforced their own quit journey by helping
- Trust and connection built between neighbors

**Alternative Flows:**

**A1: Jamie brings nicotine gum**
- At step 6, Jamie brings nicotine gum to offer as substitute
- Alex chews gum while they walk
- Harm reduction approach - gum is safer than cigarettes

**A2: Alex relapses despite support**
- At step 8, craving doesn't pass
- Alex decides to smoke one cigarette
- Jamie stays non-judgmental: "It's okay. Tomorrow's a new day. You made it 4 days - that's huge."
- Jamie still stays and supports Alex
- Alex can try quitting again tomorrow
- **No penalty for relapse - compassion is key**

**A3: Late night, Jamie offers alternative**
- At step 7, it's too cold/late to walk outside
- Jamie suggests: "Want to just hang in the lobby and chat?"
- They sit in apartment lobby, play a phone game together
- Distraction works just as well

**A4: Alex having substance craving (not tobacco)**
- At step 1, Alex is in recovery from alcohol, has strong urge to drink
- Same flow applies
- Jamie provides accountability: "Let's get you through the next 15 minutes"
- Jamie may suggest calling sponsor in addition to physical presence

**A5: Multiple neighbors respond**
- At step 4, both Jamie and Casey accept simultaneously
- System notifies both: "Alex would love support from both of you!"
- Jamie and Casey both show up
- Mini support group walk
- Even more powerful intervention

**Impact:**
- **Neighbor support can be the difference between staying quit and relapsing**
- **10-15 minutes of companionship = crisis averted**
- **No special skills needed - just show up, be kind, distract, encourage**

---

## 6. User Story Prioritization for 8-Week MVP

### Sprint 1 (Week 1-2): Foundation
- 🔴 US-R1: Sign up as responder
- 🔴 US-R2: Create basic profile
- 🔴 US-R3: Select capabilities (no training)
- 🔴 US-R6: Set availability toggle
- 🔴 US-O1: Sign up to request help

### Sprint 2 (Week 3): Responder Flow
- 🔴 US-R9: Receive emergency alert
- 🔴 US-R10: View alert details
- 🔴 US-R11: Accept emergency alert
- 🔴 US-R12: Decline emergency alert
- 🔴 US-R13: Handle timeout

### Sprint 3 (Week 4): Originator Flow & Dispatch
- 🔴 US-O2: Create emergency alert
- 🔴 US-O3: Add emergency description
- 🔴 US-O4: Confirm alert location
- 🔴 US-O6: See alert status
- 🔴 US-S1: Query available responders
- 🔴 US-S2: Send alerts to top matches
- 🔴 US-S3: Handle first acceptance
- 🔴 US-S4: Escalate on no response

### Sprint 4 (Week 5): Navigation & Resolution
- 🔴 US-R15: Get directions (Maps deep link)
- 🔴 US-R16: Use Bluetooth homing
- 🔴 US-R17: View on-scene tips
- 🔴 US-R19: Call 911 manually
- 🔴 US-R20: Update response status
- 🔴 US-O7: See responder arrival updates
- 🔴 US-O8: Cancel alert
- 🔴 US-O11: Mark emergency resolved

### Sprint 5 (Week 6): Training & Testing
- 🟡 US-R4: Watch training video (1-2 videos)
- 🟡 US-R5: Pass capability quiz (simple)
- Internal testing with team

### Sprint 6 (Week 7): Beta Testing
- Real-world testing with 50-100 users
- Bug fixes
- 🔴 US-O10: Call responding neighbor

### Sprint 7 (Week 8): Launch Prep
- 🔴 US-R23: Edit profile
- 🔴 US-R24: Manage capabilities
- Privacy & security hardening
- App store submission

### Post-MVP (Week 9+)
- All 🟡 and 🟢 priority stories
- Government dashboard
- Advanced features

---

## 7. Acceptance Test Scenarios

### Test Scenario 1: End-to-End Wellness Check
**Given:** Sarah is a responder with Wellness Check capability, marked Available
**And:** John creates a Wellness Check alert
**When:** System dispatches alert to Sarah
**And:** Sarah accepts within 30 seconds
**And:** Sarah navigates to location
**And:** Sarah performs wellness check
**And:** Sarah marks emergency resolved
**Then:**
- Alert delivered to Sarah within 3 seconds
- John sees "Sarah is on the way"
- Sarah sees navigation to John's location
- Sarah sees on-screen tips for wellness check
- John sees "Emergency resolved by Sarah"
- Alert marked as resolved in system

**Success Criteria:**
- ✅ Total response time < 5 minutes
- ✅ All notifications delivered in real-time
- ✅ Navigation accuracy within 10 meters

---

### Test Scenario 2: Decline and Escalation
**Given:** 5 responders with CPR capability in range
**And:** Alert originator creates cardiac arrest alert
**When:** First 3 responders decline
**And:** 4th responder times out (30 seconds)
**And:** 5th responder accepts
**Then:**
- Alert immediately dispatched to next responder on each decline
- 5th responder receives alert within 2 minutes of initial dispatch
- Originator sees status updates: "Finding responder..."
- Once accepted, other responders' alerts dismissed

---

### Test Scenario 3: No Responders Available
**Given:** 0 responders with AED capability in range
**And:** Alert originator creates cardiac arrest (AED needed) alert
**When:** System queries for AED responders
**Then:**
- System shows originator: "No AED responders available. Alerting CPR responders."
- System falls back to CPR-only responders
- Originator advised: "CALL 911 IMMEDIATELY"

---

### Test Scenario 4: Bluetooth Homing Accuracy
**Given:** Responder accepted alert and is within 50 meters
**And:** Bluetooth homing activates
**When:** Responder moves toward originator
**Then:**
- Distance updates in real-time (refreshes every 2 seconds)
- Directional arrow points toward originator (±15 degrees accuracy)
- "You've Arrived" triggers when within 10 feet

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-07 | Initial | User stories and use cases for 8-week MVP |

---

**Next Steps:**
1. Review and refine user stories with stakeholders
2. Assign story points and prioritize for sprints
3. Create technical specification for implementation
4. Design UI mockups based on user flows
