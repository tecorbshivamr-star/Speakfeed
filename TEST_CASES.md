# SpeakFeed End-to-End Test Cases

> Status key:  
> ✅ = Automated in Playwright  
> 🔄 = Planned automation  
> ❌ = Blocked / manual only

---

## 1. Login & Authentication

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| AUTH-001 | Speaker login with valid credentials | Speaker test account exists with completed profile setup (`step=6`, `stepStatus=true`) | 1. Go to `/login` <br>2. Enter valid speaker email <br>3. Enter valid password <br>4. Click **Log In** | User is redirected to `/dashboard` and dashboard heading is visible | ✅ |
| AUTH-002 | Organizer login with valid credentials | Organizer test account exists with completed profile setup; `NEXT_PUBLIC_ENV=test` | 1. Go to `/login` <br>2. Click **Organizers** tab <br>3. Enter valid organizer email <br>4. Enter valid password <br>5. Click **Log In** | User is redirected to `/organizer/dashboard` | ✅ |
| AUTH-003 | Login with invalid credentials | None | 1. Go to `/login` <br>2. Enter `invalid@speakfeed.test` <br>3. Enter `WrongPassword123!` <br>4. Click **Log In** | Error message shown; user stays on `/login` | ✅ |
| AUTH-004 | Login with empty email and password | None | 1. Go to `/login` <br>2. Click **Log In** without filling fields | "Please enter an email" and password validation messages are shown | ✅ |
| AUTH-005 | Session persistence after refresh | User is logged in | 1. Log in as speaker <br>2. Refresh `/dashboard` | User remains on dashboard without re-login | 🔄 |
| AUTH-006 | Logout | User is logged in | 1. Click logout <br>2. Navigate to `/dashboard` | User is redirected to `/login` | 🔄 |

---

## 2. Role Switching (Speaker ↔ Organizer)

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| ROLE-001 | Speaker switches to Organizer role | Same email exists for both Speaker and Organizer roles | 1. Log in as Speaker <br>2. Click role switcher <br>3. Select **Organizer** | URL changes to `/organizer/dashboard`; organizer navigation loads | 🔄 |
| ROLE-002 | Organizer switches to Speaker role | Same email exists for both Speaker and Organizer roles | 1. Log in as Organizer <br>2. Click role switcher <br>3. Select **Speaker** | URL changes to `/dashboard`; speaker navigation loads | 🔄 |
| ROLE-003 | Role switch preserves authentication | User has both roles | 1. Log in once <br>2. Switch roles back and forth | No re-login prompt; token/session remains valid | 🔄 |

---

## 3. Talk / Event Creation

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| TALK-001 | Speaker creates a new talk | Speaker is logged in with active subscription | 1. Go to `/talks-list` <br>2. Click **Create Talk** <br>3. Fill title, description, date, location <br>4. Click **Save** | Talk appears in talks list with correct details | 🔄 |
| TALK-002 | Organizer creates a new event | Organizer is logged in with active subscription | 1. Go to organizer events page <br>2. Click **Create Event** <br>3. Fill event details <br>4. Click **Save** | Event appears in events list | 🔄 |
| TALK-003 | Talk creation validation errors | Speaker is logged in | 1. Open create talk form <br>2. Leave required fields empty <br>3. Click **Save** | Validation errors shown for required fields; talk not saved | 🔄 |
| TALK-004 | Edit an existing talk | Talk exists | 1. Open talk details <br>2. Click **Edit** <br>3. Change title <br>4. Click **Save** | Updated title reflected in list and details | 🔄 |
| TALK-005 | Delete/archive a talk | Talk exists | 1. Open talk menu <br>2. Click **Delete/Archive** <br>3. Confirm | Talk removed or archived; success toast shown | 🔄 |
| TALK-006 | Read-only mode blocks talk creation | Speaker account is expired/read-only | 1. Log in as read-only user <br>2. Go to talks page | Create button disabled or upgrade banner shown | 🔄 |

---

## 4. QR Code Feedback Collection

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| FEED-001 | Speaker generates QR code for a talk | Talk exists and is published | 1. Open talk <br>2. Click **Get QR Code / Feedback Link** | QR code image and feedback URL are generated | 🔄 |
| FEED-002 | Attendee submits feedback via QR code | QR code/link is available | 1. Open feedback URL (incognito) <br>2. Fill rating and comments <br>3. Submit | Feedback submits successfully; thank-you message shown | 🔄 |
| FEED-003 | Feedback appears in speaker dashboard | Feedback submitted | 1. Submit feedback via QR <br>2. Open speaker dashboard | New feedback visible under correct talk | 🔄 |
| FEED-004 | Feedback form validation | Feedback URL is open | 1. Open feedback form <br>2. Submit without required rating | Validation error shown | 🔄 |
| FEED-005 | Anonymous feedback submission | Feedback URL is open | 1. Submit feedback without identifying info | Feedback saved as anonymous | 🔄 |

---

## 5. Lead Conversion

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| LEAD-001 | Convert attendee feedback to lead | Feedback exists with attendee info | 1. Open feedback list <br>2. Click **Convert to Lead** on a feedback <br>3. Confirm | Feedback appears in Leads section with contact details | 🔄 |
| LEAD-002 | Lead captures correct attendee details | Feedback with email/phone submitted | 1. Convert feedback to lead <br>2. Open lead details | Email, name, talk/event, and responses are accurate | 🔄 |
| LEAD-003 | Update lead status | Lead exists | 1. Open lead <br>2. Change status to "Contacted" <br>3. Save | Status updated and persisted | 🔄 |
| LEAD-004 | Read-only mode blocks lead conversion | Account is expired | 1. Open feedback list as read-only user | Convert-to-lead action disabled or hidden | 🔄 |

---

## 6. AI Follow-Up to Attendees

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| AI-001 | Generate AI follow-up for one lead | Lead exists | 1. Open leads list <br>2. Select a lead <br>3. Click **Send AI Follow-up** <br>4. Generate message | AI-generated message appears and is editable | 🔄 |
| AI-002 | Send AI follow-up email | Follow-up message generated | 1. Edit or accept message <br>2. Click **Send** | Success confirmation shown; email sent or queued | 🔄 |
| AI-003 | Bulk AI follow-up to multiple leads | Multiple leads selected | 1. Select multiple leads <br>2. Click **Send Follow-up** <br>3. Generate and send | Messages sent to all selected leads | 🔄 |
| AI-004 | AI follow-up history | Follow-up sent | 1. Open lead details <br>2. View follow-up history | Sent message and timestamp visible | 🔄 |
| AI-005 | AI service failure handled | AI service unavailable | 1. Try to generate follow-up | User-friendly error shown; UI does not crash | 🔄 |
| AI-006 | Read-only mode blocks follow-up | Account expired | 1. Open leads as read-only user | Send follow-up action disabled | 🔄 |

---

## 7. Booking Inquiry

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| BOOK-001 | Visitor submits booking inquiry for a speaker | Speaker public profile exists | 1. Visit speaker public profile <br>2. Click **Book Speaker** <br>3. Fill inquiry form <br>4. Submit | Success message shown; inquiry received | 🔄 |
| BOOK-002 | Visitor submits booking inquiry for an organizer | Organizer public profile exists | 1. Visit organizer public profile <br>2. Click **Book / Inquiry** <br>3. Fill form <br>4. Submit | Inquiry submitted successfully | 🔄 |
| BOOK-003 | Booking inquiry validation | Public profile open | 1. Open booking form <br>2. Submit with empty required fields | Validation errors shown | 🔄 |
| BOOK-004 | Speaker receives booking inquiry | Inquiry submitted | 1. Submit inquiry <br>2. Log in as target speaker <br>3. Check inquiries/notifications | Inquiry appears with correct details | 🔄 |
| BOOK-005 | Booking inquiry from search/profile listing | Public profile indexed | 1. Search for speaker <br>2. Click **Book** from search result | Booking form opens pre-filled with speaker info | 🔄 |

---

## 8. Subscription & Billing

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| BILL-001 | 14-day free trial activates on first login | New user created via backend seeding | 1. Log in as trial user <br>2. Check billing/pricing page | Trial countdown visible; full features accessible | 🔄 |
| BILL-002 | Speaker purchases Starter plan ($19) | Speaker on trial or read-only | 1. Go to pricing <br>2. Select Starter <br>3. Enter test card `4242 4242 4242 4242` <br>4. Pay | Payment succeeds; account status active; read-only banner removed | 🔄 |
| BILL-003 | Speaker purchases Pro plan ($29) | Speaker on trial or read-only | 1. Go to pricing <br>2. Select Pro <br>3. Enter test card <br>4. Pay | Pro plan active; premium features unlocked | 🔄 |
| BILL-004 | Organizer purchases plan ($29) | Organizer on trial or read-only | 1. Go to organizer pricing <br>2. Select plan <br>3. Pay | Organizer subscription active | 🔄 |
| BILL-005 | Pause subscription | Active subscriber | 1. Open billing settings <br>2. Click **Pause** <br>3. Confirm | Subscription paused; limited access or banner shown | 🔄 |
| BILL-006 | Resume subscription | Subscription paused | 1. Open billing settings <br>2. Click **Resume** | Full access restored | 🔄 |
| BILL-007 | Cancel subscription | Active subscriber | 1. Open billing settings <br>2. Click **Cancel** <br>3. Confirm cancellation | Account enters 90-day read-only mode | 🔄 |
| BILL-008 | Re-purchase after cancel | Account in read-only mode | 1. Click upgrade/purchase CTA <br>2. Complete payment | Account becomes active again | 🔄 |
| BILL-009 | Payment failure handling | User on pricing page | 1. Enter declined test card `4000 0000 0000 0002` <br>2. Submit | Error message shown; account remains unchanged | 🔄 |

---

## 9. Read-Only Mode Enforcement

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| RO-001 | Read-only banner on dashboard | Account expired or canceled | 1. Log in as read-only user <br>2. Go to `/dashboard` | Read-only banner/modal visible | 🔄 |
| RO-002 | Read-only banner on talks list | Account expired | 1. Go to `/talks-list` | Banner visible; create/edit disabled | 🔄 |
| RO-003 | Read-only banner on leads page | Account expired | 1. Go to leads page | Banner visible; convert/send follow-up disabled | 🔄 |
| RO-004 | Create action blocked in read-only mode | Account expired | 1. Try to create talk/event | Button disabled or modal redirects to pricing | 🔄 |
| RO-005 | Edit action blocked in read-only mode | Existing talk exists | 1. Open talk <br>2. Try to edit | Edit option disabled or blocked | 🔄 |
| RO-006 | Delete action blocked in read-only mode | Existing talk exists | 1. Try to delete talk | Delete option disabled | 🔄 |
| RO-007 | Purchase CTA functional in read-only mode | Account expired | 1. Click upgrade/purchase CTA from banner <br>2. Complete payment | Redirected to pricing; after payment full access restored | 🔄 |
| RO-008 | 90-day expiration timer accuracy | Subscription canceled | 1. Cancel subscription <br>2. Check banner on multiple days | Timer counts down correctly toward expiration | 🔄 |
| RO-009 | Account expires after 90 days | Canceled 90+ days ago | 1. Log in | Account data inaccessible or "expired" state shown | 🔄 |

---

## 10. Sign-Up & Onboarding (Manual / API-seeded due to reCAPTCHA)

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| REG-001 | Speaker sign-up flow | reCAPTCHA bypassed or test environment | 1. Go to `/signup` <br>2. Choose Speaker <br>3. Fill name, email, password <br>4. Submit | Account created; verification email sent | ❌ |
| REG-002 | Organizer sign-up flow | reCAPTCHA bypassed or test environment | 1. Go to `/signup` <br>2. Choose Organizer <br>3. Fill details <br>4. Submit | Organizer account created | ❌ |
| REG-003 | Email verification | Sign-up completed | 1. Click verification link in email | Email verified; user can log in | ❌ |
| REG-004 | Profile setup wizard | Newly registered user | 1. Log in <br>2. Complete profile setup steps <br>3. Reach step 6 | Redirected to dashboard; profile saved | ❌ |
| REG-005 | Sign-up validation | On signup page | 1. Submit empty/invalid form | Validation errors shown | ❌ |
| REG-006 | Forgot password flow | Account exists | 1. Go to `/forgot` <br>2. Enter email <br>3. Submit reCAPTCHA | Reset email sent | ❌ |
| REG-007 | Social login (Google/Apple/Microsoft/LinkedIn) | Social account available | 1. Click social login button <br>2. Complete OAuth | User logged in or signed up | ❌ |

---

## 11. Navigation & Locale

| TC ID | Test Case | Precondition | Steps | Expected Result | Status |
|-------|-----------|--------------|-------|-----------------|--------|
| NAV-001 | Default locale routing | None | 1. Visit `/login` | Internally served as `/en/login` | 🔄 |
| NAV-002 | Arabic locale loads | None | 1. Visit `/ar/login` | Page renders in Arabic | 🔄 |
| NAV-003 | Unauthenticated access to protected route | Not logged in | 1. Visit `/dashboard` | Redirected to `/login` | 🔄 |
| NAV-004 | Public pages accessible without login | None | 1. Visit `/`, `/pricing`, `/features` | Pages load successfully | 🔄 |

---

## Automation Roadmap

| Phase | Flows | Goal |
|-------|-------|------|
| 1 (Current) | AUTH-001 to AUTH-004 | Validate login entry point for both roles |
| 2 | ROLE-001 to ROLE-003, TALK-001 to TALK-006 | Core content creation and role switching |
| 3 | FEED-001 to FEED-005, LEAD-001 to LEAD-004 | Feedback loop and lead conversion |
| 4 | AI-001 to AI-006 | AI follow-up differentiation |
| 5 | BOOK-001 to BOOK-005 | Discovery and booking path |
| 6 | BILL-001 to BILL-009, RO-001 to RO-009 | Revenue and read-only state machine |
| 7 | REG-001 to REG-007 | Sign-up/onboarding once reCAPTCHA can be bypassed in test env |

---

## Notes for Manual Testers

- **Blocked flows (❌)** require reCAPTCHA bypass in a dedicated test environment, backend API seeding, or manual execution.
- For automated tests, always use stable selectors like `data-testid`, `id`, or semantic ARIA roles. Avoid CSS class names.
- Keep test accounts in known states. Reset or recreate fixtures before destructive tests.
- When adding a new test, first run it 5 times locally to confirm it is not flaky before adding it to CI.
