# SpeakFeed Platform — Complete Workflow Document

> **Platform:** SpeakFeed | **Modes:** Speaker Mode & Organizer Mode  
> **URLs:** Speaker → `https://speakfeed.com/speaker/dashboard` | Organizer → `https://speakfeed.com/organizer/dashboard`  
> **Source:** Organizer_Module.pdf, Speaker_Module.pdf, Dev Team Q&A DOCX

---

## 1. PLATFORM OVERVIEW

SpeakFeed is a dual-mode SaaS platform designed to bridge the gap between **event Speakers** and **event Organizers**. It enables speakers to collect audience feedback via QR codes, manage leads, send AI-powered follow-up emails, and build professional profiles. Organizers can create and manage events, invite speakers, monitor live audience responses, and track engagement analytics.

### Two Modes, One Account
A single email address can be used for **both** Speaker Mode and Organizer Mode. Each mode requires a **separate subscription payment**.

| Feature | Speaker Mode | Organizer Mode |
|---|---|---|
| Primary URL | `/speaker/dashboard` | `/organizer/dashboard` |
| Core Purpose | Manage talks, collect leads & feedback | Manage events, invite speakers, monitor sessions |
| Subscription | Paid separately | Paid separately |
| Shared Login | Yes (same email) | Yes (same email) |

---

## 2. AUTHENTICATION WORKFLOW

### 2.1 Sign Up
**Real-life example:** *Sarah is a keynote speaker who wants to use SpeakFeed to collect audience feedback at conferences.*

**Steps:**
1. Visit `https://speakfeed.com` → click **Sign Up**
2. Enter: Full Name, Email Address, Password
3. Select mode: **Speaker** or **Organizer** (can add the other later)
4. Verify email via confirmation link
5. Complete profile setup wizard
6. Land on respective dashboard

### 2.2 Sign In
**Steps:**
1. Visit login page → enter email + password
2. System detects which mode(s) are active for the account
3. If user has **both modes**: a mode selector appears → choose Speaker or Organizer
4. If user has **one mode only**: directly redirected to that dashboard

### 2.3 Forgot Password
**Steps:**
1. Click "Forgot Password" on login page
2. Enter registered email address
3. Receive password reset link via email
4. Click link → set new password
5. Log in with new credentials

---

## 3. SPEAKER MODE — COMPLETE WORKFLOW

> **URL:** `https://speakfeed.com/speaker/dashboard`

### 3.1 Speaker Navigation Menu
- Dashboard
- My Talks
- Live View
- Leads
- Audience Feedback
- AI Follow-Up
- Performance
- Profile Builder
- Public Profile
- Billing
- Settings
- Integrations
- Invite a Friend
- Switch to Organizer Mode *(if subscribed)*

---

### 3.2 Dashboard
**Real-life example:** *Sarah logs in and sees a summary of her last 3 talks, total leads captured, and pending follow-up emails.*

The dashboard shows:
- Total Talks created
- Total Leads captured
- Feedback responses received
- Pending AI Follow-up emails
- Quick action buttons: Create Talk, View Leads, Send Follow-up

---

### 3.3 My Talks (Talk Management)

**Real-life example:** *Sarah spoke at "Marketing World 2025" and "TechConf India." Each talk has its own QR code, feedback questions, and lead list.*

#### Create a New Talk
1. Click **"+ Create Talk"**
2. Fill in:
   - **Talk Title** (e.g., "The Future of AI in Marketing")
   - **Event Name** (e.g., "Marketing World 2025")
   - **Event Date**
   - **Event Location**
   - **Organizer Details** *(optional — needed to send AI Feedback Summary directly to organizer)*
3. Add **General Questions** (standard audience feedback questions)
4. Add **Custom Feedback Questions** (speaker-specific questions)
5. Add **Resources & Gifts** (e.g., free eBook download links, discount codes)
6. Click **Save & Generate QR Code**
7. Talk is now live with a unique QR code

> ⚠️ **Note:** If Organizer Details are left blank, the speaker cannot send the AI Feedback Summary directly to the organizer through the platform. They would need to send it manually from their external email.

#### Edit / Delete a Talk
- Open any talk → click **Edit** to modify details, questions, or resources
- Click **Delete** → confirmation dialog appears
- Talks can be organized into **folders and subfolders**

#### Folder Management
- Create folders (e.g., "2024 Talks", "Corporate Events", "Webinars")
- Create subfolders inside folders (e.g., "2024 Talks → Q1 → January")
- Two-pane layout: left pane = folder tree, right pane = events table
- Delete folder → pop-up warning: *"All events contained in this folder will be deleted. Please move any events that you want to keep to a new folder before deleting."*

---

### 3.4 QR Code System

**Real-life example:** *During her talk, Sarah displays a QR code slide. Audience members scan it with their phones, fill out feedback, and their contact info is captured as a lead.*

**How it works:**
1. Each Talk generates a unique QR code
2. Speaker displays QR code (on slide, printed card, or screen)
3. Audience scans QR with their phone camera
4. A mobile-friendly form opens — audience fills in:
   - Name, Email (lead capture)
   - Answers to feedback questions
   - Rating (if configured)
5. On submission:
   - Lead is added to the speaker's **Leads** list
   - Feedback is recorded under **Audience Feedback**
   - AI Follow-up trigger is activated

---

### 3.5 Live View

**Real-life example:** *While Sarah is on stage, her assistant monitors the Live View dashboard on a tablet. As audience members scan the QR, responses appear in real-time.*

- Real-time feed of incoming audience responses
- Shows: respondent name, email, feedback summary
- Speaker/assistant can monitor engagement while talk is ongoing
- No manual refresh needed (auto-updates)

---

### 3.6 Leads

**Real-life example:** *After "TechConf India," Sarah has 47 leads. She filters by Talk Title to see leads from that specific event only.*

**Features:**
- View all leads captured across all talks
- Filter leads by:
  - Talk Title
  - Date/Time
  - Event Name
- Export leads to CSV
- View individual lead details (name, email, feedback submitted)
- Mark leads as contacted / follow-up pending

---

### 3.7 Audience Feedback

**Real-life example:** *Sarah reviews feedback from 47 audience members. The AI generates a summary: "Most attendees found your storytelling compelling but wanted more data-driven examples."*

**Features:**
- View all feedback per talk
- AI-generated feedback summary for each talk
- See individual responses to each question
- Star ratings overview
- Send AI Feedback Summary to Event Organizer:
  - If organizer email was added to the Talk → send directly via platform
  - If not added → generate shareable link → send manually

---

### 3.8 AI Follow-Up

**Real-life example:** *Sarah's 47 leads each receive a personalized email: "Hi [Name], thank you for attending my talk on AI in Marketing at TechConf India. Here's the free resource I promised..." — all sent automatically.*

**How it works:**
1. After a QR code is scanned and lead is captured, AI Follow-up is queued
2. Speaker can configure:
   - Follow-up email template
   - Delay (immediately / 24 hrs / custom)
   - Personalization tokens: [FirstName], [TalkTitle], [ResourceLink]
3. AI personalizes each email using the feedback data submitted
4. Emails are sent automatically

**Templates:**
- Create and save custom email templates
- Variables: [Name], [Talk Title], [Event], [Resource Link], [Gift Link]
- Template library for reuse across talks

**Scheduling:**
- View upcoming scheduled follow-up emails
- Cancel or reschedule individual emails

---

### 3.9 Performance Tab

**Real-life example:** *Sarah sent 47 AI follow-up emails. Performance shows: 38 Opens (81%), 22 Clicks (47%), 8 Replies (17%). She filters by "TechConf India" to isolate those results.*

**Tracks:**
- **Opens** — how many recipients opened the email
- **Clicks** — how many clicked a link in the email
- **Replies** — how many replied to the email

**Speaker Mode Filters:**
- Talk Title
- Time (date range)

---

### 3.10 Profile Builder

**Real-life example:** *Sarah builds her public speaker profile: headshot, bio, speaking topics, testimonials, social links. This profile can be shared with event organizers.*

**Fields:**
- Profile Photo
- Full Name
- Title / Tagline
- Bio (short and long versions)
- Speaking Topics
- Past Events / Speaking History
- Testimonials
- Social Media Links
- Contact / Booking Information
- Videos (talk clips, demos)

---

### 3.11 Public Profile

- Shareable public-facing URL for Sarah's speaker profile
- Organizers can view this profile when considering speakers
- Can be linked from LinkedIn, email signature, website

---

### 3.12 Billing (Speaker Mode)

**Features:**
- View current subscription plan
- Upgrade / Downgrade plan
- View payment history and invoices
- Update payment method
- Cancel subscription
- **Switch & Save** — if subscribed to both modes, bundle pricing may apply

---

### 3.13 Settings (Speaker Mode)

- Update profile information
- Change password
- Notification preferences
- Account preferences
- Delete account option

---

### 3.14 Integrations (Speaker Mode)

- Connect CRM tools (e.g., HubSpot, Salesforce)
- Email platform integrations
- Calendar integrations
- Zapier / webhook support

---

### 3.15 Invite a Friend

- Generate referral link
- Track invitations sent and accepted
- Referral rewards (if applicable)

---

## 4. ORGANIZER MODE — COMPLETE WORKFLOW

> **URL:** `https://speakfeed.com/organizer/dashboard`

### 4.1 Organizer Navigation Menu
- Dashboard
- Events
- Live View
- Leads / Inquiry
- AI Follow-Up
- Performance
- Schedule
- Templates
- Organizers (Team)
- Presenter QR
- Profile Builder
- Public Profile
- Billing
- Settings
- Integrations
- Invite a Friend
- Switch to Speaker Mode *(if subscribed)*

---

### 4.2 Dashboard (Organizer Mode)

**Real-life example:** *John runs a conference company. His dashboard shows: 3 upcoming events, 12 speakers invited, 240 audience leads collected this month, 5 pending speaker inquiries.*

Displays:
- Upcoming events count
- Total events managed
- Total leads captured across all events
- Pending speaker inquiries
- Quick actions: Create Event, View Leads, Manage Speakers

---

### 4.3 Events Management

**Real-life example:** *John is organizing "EdTech Summit 2025." He creates the event, adds sessions, invites speakers, and shares presenter QR codes.*

#### Create a New Event
1. Click **"+ Create Event"**
2. Fill in:
   - **Event Title** (e.g., "EdTech Summit 2025")
   - **Event Type** (Conference / Webinar / Workshop / Seminar)
   - **Date & Time**
   - **Location / Virtual Link**
   - **Description**
3. Add sessions/tracks within the event
4. Assign speakers to sessions
5. Configure feedback questions for the event
6. Save event

#### Event Folder Management
- Organize events into folders and subfolders
- Two-pane layout: folder tree (left) + events table (right)
- Can nest: "2025 Events → Conferences → Q1"
- Delete folder warning: *"All events contained in this folder will be deleted. Please move any events that you want to keep to a new folder before deleting."*

---

### 4.4 Organizers (Team Management)

**Real-life example:** *John's company has 5 team members. He invites co-organizers Rachel and Mike to help manage EdTech Summit 2025.*

**Features:**
- Add co-organizers by email
- Assign roles / permissions
- View team member list
- Remove team members

---

### 4.5 Presenter QR Code

**Real-life example:** *John generates a QR code specific to each speaker session. Speaker Alex displays this QR during his talk — audience scans it, and John can track which session each lead came from.*

**How it works:**
1. Each session/speaker gets a unique Presenter QR code
2. Organizer downloads/prints the QR or sends it to the speaker
3. Audience scans during the session
4. Leads are tagged to that specific session and speaker
5. Organizer can view per-session leads and feedback

---

### 4.6 Live View (Organizer Mode)

**Real-life example:** *During EdTech Summit, John monitors all 4 simultaneous sessions in Live View. He sees real-time audience scans happening in Session B (300 people) and Session C (180 people).*

**Features:**
- Real-time audience engagement monitoring
- See per-session QR scan counts
- Live feed of incoming leads and feedback
- Monitor all sessions simultaneously

---

### 4.7 Leads / Inquiry

**Real-life example:** *A speaker inquiry comes in: "I'd like to present at your next conference. Here's my profile..." John reviews it in the Inquiry section.*

**Leads:**
- View all audience leads captured at the event
- Filter by: Event Title, Event Type, Session, Time
- Export leads to CSV

**Inquiry:**
- Incoming speaker inquiries/applications
- Review speaker profile and pitch
- Accept / Decline / Request more info
- Track inquiry status

---

### 4.8 AI Follow-Up (Organizer Mode)

**Real-life example:** *After EdTech Summit, 500 attendees get a personalized email: "Thank you for attending EdTech Summit 2025! Here are the session recordings and resources..." — sent automatically by John's configured template.*

**Features:**
- Configure post-event follow-up emails for attendees
- Use templates with personalization tokens
- Track send status and performance

**Organizer Mode Filters (Performance):**
- Event Title
- Event Type
- Time (date range)

---

### 4.9 Schedule

**Real-life example:** *John can see all AI follow-up emails scheduled for post-event delivery and make adjustments before they go out.*

- View all upcoming scheduled emails
- Edit or cancel scheduled sends
- Reschedule emails

---

### 4.10 Templates (Organizer Mode)

- Create reusable email templates for post-event follow-up
- Personalization tokens: [AttendeName], [EventTitle], [SessionTitle], [ResourceLink]
- Template library — save and reuse across events

---

### 4.11 Performance (Organizer Mode)

**Real-life example:** *John sent 500 follow-up emails after EdTech Summit. Performance shows: 410 Opens (82%), 280 Clicks (56%), 45 Replies (9%). He filters by "Event Type: Conference" to compare with webinar results.*

**Tracks:**
- Opens, Clicks, Replies from AI Follow-up emails

**Filters:**
- Event Title
- Event Type
- Time (date range)

---

### 4.12 Profile Builder (Organizer Mode)

- Build a public organizer/company profile
- Organization name, logo, description
- Event history
- Contact & booking information

---

### 4.13 Billing (Organizer Mode)

- Current subscription plan details
- Payment history & invoices
- Upgrade/Downgrade plan
- Cancel subscription
- **Switch & Save** bundle pricing

---

### 4.14 Settings (Organizer Mode)

- Account and notification settings
- Team access management
- Integration configuration

---

### 4.15 Integrations (Organizer Mode)

- CRM integrations
- Email marketing platforms
- Calendar sync
- Zapier support

---

## 5. SPEAKER ↔ ORGANIZER SWITCHING WORKFLOW

### 5.1 Key Rules for Mode Switching

1. **Same Email:** One email can log into both modes
2. **Separate Subscriptions:** Each mode requires its own paid plan
3. **Separate URLs:** Different dashboards for each mode
4. **No Data Mixing:** Speaker data (talks, leads) is separate from Organizer data (events, sessions)

---

### 5.2 Scenario A: Speaker Switches to Organizer Mode

**Real-life example:**
> *Sarah is a keynote speaker who also runs her own small conference called "Women in Tech Forum." She pays for both Speaker and Organizer subscriptions using the same email.*

**Switching Steps:**
1. Sarah is logged in at `speakfeed.com/speaker/dashboard`
2. In the left sidebar, she clicks **"Switch to Organizer Mode"**
3. System redirects her to `speakfeed.com/organizer/dashboard`
4. She now sees the Organizer dashboard with her events, not her talks
5. All organizer features are now accessible: Events, Presenter QR, Team, etc.
6. Her speaker data (My Talks, Speaker Leads) is safely preserved — just not visible in Organizer mode

**What changes:**
- URL changes from `/speaker/dashboard` → `/organizer/dashboard`
- Sidebar navigation changes to Organizer menu
- Dashboard widgets show organizer metrics

**What stays the same:**
- Login session (no re-login required)
- Account email and settings
- Subscription for each mode remains active

---

### 5.3 Scenario B: Organizer Switches to Speaker Mode

**Real-life example:**
> *John organizes conferences but is also invited to give a keynote at "Future Leaders Summit." He pays for Speaker mode too and switches to manage his speaking engagement.*

**Switching Steps:**
1. John is at `speakfeed.com/organizer/dashboard`
2. In the left sidebar, he clicks **"Switch to Speaker Mode"**
3. System redirects him to `speakfeed.com/speaker/dashboard`
4. He now sees the Speaker dashboard with his talks and leads
5. He creates a new Talk for his upcoming keynote
6. Generates QR code for audience feedback
7. To go back: clicks **"Switch to Organizer Mode"** in the sidebar

---

### 5.4 Scenario C: New User — Only One Mode Active

**Real-life example:**
> *Mike is a corporate trainer. He signs up for Speaker Mode only. He does NOT see a "Switch to Organizer Mode" option in his sidebar — only Speaker features are available.*

**Behavior:**
- "Switch to Organizer" option is only visible if the user has an active Organizer subscription
- If Mike later purchases Organizer Mode, the switch option appears automatically

---

### 5.5 Switching Flow Diagram

```
[Login Page]
     |
     ↓
[Mode Selection] (if both modes active)
     |                      |
     ↓                      ↓
[Speaker Dashboard]    [Organizer Dashboard]
speakfeed.com/         speakfeed.com/
speaker/dashboard      organizer/dashboard
     |                      |
     | "Switch to Org"       | "Switch to Speaker"
     ↓                      ↓
[Organizer Dashboard]  [Speaker Dashboard]
```

---

## 6. FEATURE COMPARISON TABLE

| Feature | Speaker Mode | Organizer Mode |
|---|---|---|
| Dashboard | Talk stats, leads, feedback summary | Event stats, session leads, speaker inquiries |
| Primary Object | Talk | Event |
| QR Code | Per Talk — audience feedback | Per Session/Presenter — audience capture |
| Leads | Audience members who scanned talk QR | Attendees across all event sessions |
| Inquiry | N/A | Incoming speaker applications |
| AI Follow-Up | Post-talk email to leads | Post-event email to attendees |
| Performance Filters | Talk Title, Time | Event Title, Event Type, Time |
| Live View | Real-time scans for active talk | Real-time scans across all sessions |
| Feedback | Per-talk audience feedback + AI summary | Per-session audience feedback |
| AI Feedback Summary | Sent to organizer (if contact added) | Received from speakers |
| Profile | Speaker bio, topics, videos | Organizer/company info |
| Team | N/A | Add co-organizers, assign roles |
| Folder Mgmt | Talk folders | Event folders |
| Billing | Speaker subscription | Organizer subscription |
| Switch Option | Switch to Organizer *(if subscribed)* | Switch to Speaker *(if subscribed)* |

---

## 7. BILLING & SUBSCRIPTIONS

### 7.1 Subscription Model
- Each mode (Speaker / Organizer) has its own paid subscription
- Subscriptions are managed separately under each mode's Billing section
- A user can hold both subscriptions simultaneously

### 7.2 Switch & Save
**Real-life example:** *Sarah subscribes to both modes. The "Switch & Save" option offers a bundled pricing discount for holding both subscriptions.*

- Available under Billing in either mode
- Shows current plans side-by-side
- Bundle discount applied automatically

### 7.3 Billing Features (Both Modes)
- View current plan and renewal date
- Upgrade or downgrade plan
- Payment history
- Download invoices
- Update payment method (card details)
- Cancel subscription

---

## 8. COMPLETE END-TO-END SCENARIO

### Scenario: The Speaker-Organizer

**Background:** *Alex Chen is a professional speaker AND runs an annual technology summit. He uses both SpeakFeed modes.*

---

#### Phase 1 — As a Speaker (Morning of his talk)
1. Alex logs in at `speakfeed.com/speaker/dashboard`
2. Opens **My Talks** → finds "AI Revolution" talk for today's conference
3. Opens the talk → displays the **QR Code slide** on his presentation screen
4. Audience of 200 people scans the QR code during his talk
5. In real-time, Alex's assistant watches **Live View** — 156 scans logged
6. After the talk, Alex checks **Leads** — 156 new leads from this talk
7. Reviews **Audience Feedback** → AI Summary: "Audience loved the demos. Wanted more Q&A time."
8. AI Summary is sent to the event organizer via the platform (organizer email was pre-filled)
9. **AI Follow-Up** emails are queued → 156 personalized emails scheduled for next morning

---

#### Phase 2 — Switching to Organizer Mode (Afternoon)
10. Alex clicks **"Switch to Organizer Mode"** in the sidebar
11. Redirected to `speakfeed.com/organizer/dashboard`
12. Dashboard now shows: "Tech Leaders Summit 2025" — his own event happening next week
13. He navigates to **Events** → opens "Tech Leaders Summit 2025"
14. Checks **Organizers** section → adds co-organizer Rachel with event management access
15. Goes to **Presenter QR** → generates QR codes for 6 speaker sessions
16. Sends each QR code to the respective speaker via email
17. Reviews **Inquiry** section → 3 new speaker applications received
18. Accepts one speaker, declines one, requests more info from the third

---

#### Phase 3 — Event Day (As Organizer)
19. Alex monitors **Live View** — all 6 sessions running simultaneously
20. Session 3 has the highest engagement: 340 scans in 45 minutes
21. **Leads** section shows 890 total audience leads captured so far
22. Post-event: configures **AI Follow-Up** template for all 890 attendees
23. Schedules the email for next morning 9 AM
24. Checks **Performance** the next day → 720 Opens, 410 Clicks, 62 Replies

---

#### Phase 4 — Switching Back to Speaker Mode
25. Alex clicks **"Switch to Speaker Mode"** in the sidebar
26. Returns to `speakfeed.com/speaker/dashboard`
27. Checks his **Performance** for the AI follow-up sent to his 156 leads:
    - Opens: 134 (86%), Clicks: 89 (57%), Replies: 22 (14%)
28. Reviews new **Leads** — 8 people replied and want to book him for future events
29. Updates his **Profile Builder** with the new talk title and testimonial

---

## 9. KEY BUSINESS RULES SUMMARY

| Rule | Detail |
|---|---|
| Same email for both modes | ✅ Yes — one account, two modes |
| Separate payment required | ✅ Yes — each mode billed independently |
| Separate URLs | ✅ Speaker: `/speaker/dashboard` · Organizer: `/organizer/dashboard` |
| Switch option visibility | Only shown if user has active subscription for the other mode |
| Folder delete behavior | Warning shown; contained events are also deleted |
| Organizer field in Talk | Optional; required for in-platform AI Summary delivery |
| AI Follow-Up tracking | Opens, Clicks, Replies tracked per campaign |
| Performance filters (Speaker) | Talk Title, Time |
| Performance filters (Organizer) | Event Title, Event Type, Time |
| QR code scope | Speaker: per Talk · Organizer: per Session/Presenter |

---

*Document prepared based on: Orzanizer_Module.pdf, Speaker_Module.pdf, and 5.13.26_Answers to Dev Team Questions.docx*
