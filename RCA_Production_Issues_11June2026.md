# Root Cause Analysis (RCA) — SpeakFeed Production Incident

| Field | Details |
|---|---|
| **Document Type** | Root Cause Analysis (RCA) |
| **Incident Date** | 11 June 2026 |
| **Reported Time** | Morning (IST) |
| **Reported By** | Jon (End User) via direct message |
| **Escalated By** | Ankesh (Team Lead) |
| **QA Tester (Manual)** | Shivam |
| **Backend Action Taken** | Pankaj (Backend Dev) — Code Revert Initiated |
| **Environment Affected** | **Production** |
| **Severity** | 🔴 Critical — Core user flow broken |
| **Status** | Under Investigation / Rollback Initiated |

---

## 1. INCIDENT SUMMARY

On 11 June 2026, a production deployment was pushed to the SpeakFeed platform. Following the deployment, **three critical bugs** were discovered by an end user (Jon) during normal platform usage. These issues directly break the **Talk Creation workflow** — one of the most fundamental features of the SpeakFeed Speaker Module.

The affected user was unable to:
- Select any template from the dropdown (admin templates missing)
- Save a self-created custom template (not reflected in dropdown)
- Complete the Talk creation process due to a General Questions error blocking QR code generation

A rollback of backend code was initiated by Pankaj as an emergency response.

---

## 2. USER-REPORTED ISSUE (Verbatim)

> *"Good morning Tim! Quick item for the dev team… for some reason when I go to create a new talk, the template(s) are missing and when you create your own the platform won't save it… and when it asks for you to select additional questions, and you do, you can't complete the process… here's a screenshot."*
>
> — **Jon**, End User, Production

---

## 3. ISSUES IDENTIFIED

### 🔴 Issue #1 — Admin Templates Missing from Dropdown

| Field | Detail |
|---|---|
| **Feature Affected** | Create New Talk → Template Selection Dropdown |
| **Expected Behavior** | Admin-configured templates (pre-built by the platform) should appear in the dropdown when a user is creating a new Talk |
| **Actual Behavior** | Dropdown is empty — no admin templates are visible |
| **User Impact** | User cannot use any pre-built template to start a Talk |
| **Severity** | Critical |

**Steps to Reproduce:**
1. Log in to SpeakFeed Speaker Mode → `speakfeed.com/speaker/dashboard`
2. Navigate to **My Talks** → click **"+ Create Talk"**
3. Reach the Template Selection step
4. Click the template dropdown
5. **Bug:** Dropdown is empty — no admin templates displayed

---

### 🔴 Issue #2 — Custom Template Not Saving / Not Appearing in Dropdown

| Field | Detail |
|---|---|
| **Feature Affected** | Create Custom Template → Save → Dropdown Reflection |
| **Expected Behavior** | When a user creates a new custom template and saves it, it should immediately appear in the template dropdown during Talk creation |
| **Actual Behavior** | Template appears to save (no error shown), but does NOT appear in the dropdown |
| **User Impact** | Users cannot use their own templates — effectively cannot proceed with Talk creation using custom templates |
| **Severity** | Critical |

**Steps to Reproduce:**
1. Log in → Speaker Dashboard → **My Talks** → **+ Create Talk**
2. On the Template step → click **"Create Custom Template"**
3. Fill in template name, questions, and other fields
4. Click **Save**
5. Return to the template dropdown
6. **Bug:** Newly created template does NOT appear in the dropdown

---

### 🔴 Issue #3 — QR Code Generation Blocked by General Questions Error

| Field | Detail |
|---|---|
| **Feature Affected** | Talk Creation → General Questions → QR Code Generation |
| **Expected Behavior** | After selecting general/additional questions, the user should be able to complete the Talk creation flow and generate a QR code |
| **Actual Behavior** | An error is thrown when the user tries to select/submit General Questions — this blocks the entire process and prevents QR code generation |
| **User Impact** | The entire QR code workflow is non-functional. Speakers cannot create new Talks or collect audience feedback |
| **Severity** | Critical |

**Steps to Reproduce:**
1. Log in → Speaker Dashboard → **My Talks** → **+ Create Talk**
2. Fill in Talk details (Title, Event, Date, etc.)
3. Reach the **General Questions** step
4. Select additional questions from the list
5. Click **Next / Save / Generate QR**
6. **Bug:** Error is thrown — process cannot be completed — QR code not generated

---

## 4. BUSINESS IMPACT

| Area | Impact |
|---|---|
| **User Experience** | Core Talk creation flow is completely broken for all users |
| **Revenue Risk** | Users unable to use the product they have paid for |
| **Brand Reputation** | Real user (Jon) escalated directly — indicates visible, front-facing failure |
| **Data Loss Risk** | Templates created by users during this window may not be saved |
| **QR Code Workflow** | Completely non-functional — speakers cannot capture audience leads or feedback |
| **Ongoing Events** | Any speaker trying to set up a talk during this period is blocked |

---

## 5. TIMELINE OF EVENTS

| Time | Event |
|---|---|
| Pre-Incident | Backend code changes deployed to **Production** |
| Morning (IST) | Jon (end user) attempts to create a new Talk |
| Morning (IST) | Jon reports 3 issues via direct message to Tim |
| ~19:40 IST | Issue escalated internally — Shivam and Pankaj notified |
| ~19:40 IST | Pankaj instructed to **revert backend code** |
| Ongoing | RCA being prepared by QA (Shivam) |

---

## 6. ROOT CAUSE ANALYSIS

### Primary Root Cause

> **Backend code was deployed to Production without adequate QA testing and sign-off.**

The new backend deployment introduced breaking changes to:
1. The **template data-fetching API** — causing the dropdown to return empty results (admin templates not loaded)
2. The **template save/persist API** — causing newly created templates to not be committed to the database or returned in subsequent dropdown queries
3. The **General Questions validation/submission API** — throwing an unhandled error that blocks the Talk creation flow

### Contributing Factors

| # | Factor | Responsible Area |
|---|---|---|
| 1 | No QA regression testing was performed on the Talk Creation flow before production deployment | QA / Process |
| 2 | Backend code was pushed directly to Production without a staging environment verification step | Dev / DevOps |
| 3 | No pre-deployment checklist was followed to verify critical user flows | Process |
| 4 | No smoke test was run post-deployment on Production | QA / Dev |
| 5 | No rollback plan was pre-defined before the deployment | DevOps / Management |

---

## 7. QA ACCOUNTABILITY & REVIEW GAP

> *As stated by the team lead: "Without checking the latest backend code, why did we deploy to Production? @Shivam — you were mentioned multiple times to check this."*

As the **QA Tester** on this project, the following review gaps are acknowledged:

| Gap | Detail |
|---|---|
| **Pre-deployment Test Execution** | The latest backend changes were not tested against the Talk creation flow before deployment |
| **Regression Checklist** | A regression test suite covering core flows (Talk Creation, Template Save, QR Generation) was not executed |
| **Deployment Communication** | QA sign-off was not formally obtained before the production deployment |
| **Staging Verification** | Issues on the staging/dev environment (if present) were not escalated to block the release |

**Note:** This RCA acknowledges that QA testing of the latest backend code prior to production deployment was insufficient. Going forward, a formal **"QA Sign-Off Gate"** will be required before any backend push to production.

---

## 8. IMMEDIATE ACTIONS TAKEN

| Action | Owner | Status |
|---|---|---|
| Rollback of backend code initiated | Pankaj (Backend Dev) | ✅ Done |
| User (Jon) acknowledged and update sent | Ankesh (Team Lead) | ✅ Done |
| All three issues logged as Critical bugs | Shivam (QA) | 🔄 Done  |
| Verify rollback restores all 3 features | Shivam (QA) | 🔄 Done  |

---

## 9. TEST CASES — VERIFICATION AFTER ROLLBACK

The following test cases must be executed by QA **immediately after rollback** to confirm Production is stable.

### TC-01: Admin Templates Appear in Dropdown

| Field | Detail |
|---|---|
| **Test ID** | TC-TMPL-001 |
| **Priority** | P0 — Critical |
| **Precondition** | User logged in to Speaker Mode |
| **Steps** | 1. Go to My Talks → + Create Talk → Reach Template step → Open dropdown |
| **Expected Result** | All admin-configured templates are visible in the dropdown |
| **Pass Criteria** | At least all pre-existing admin templates are displayed |

---

### TC-02: Custom Template Save and Appear in Dropdown

| Field | Detail |
|---|---|
| **Test ID** | TC-TMPL-002 |
| **Priority** | P0 — Critical |
| **Precondition** | User logged in to Speaker Mode |
| **Steps** | 1. Go to Create Talk → Template step → Create Custom Template → Fill details → Save → Re-open dropdown |
| **Expected Result** | Newly created template appears in the dropdown immediately after saving |
| **Pass Criteria** | Custom template is selectable from dropdown without page refresh |

---

### TC-03: General Questions — No Error on Selection

| Field | Detail |
|---|---|
| **Test ID** | TC-GQ-001 |
| **Priority** | P0 — Critical |
| **Precondition** | User logged in to Speaker Mode |
| **Steps** | 1. Create Talk → Fill basic details → Reach General Questions step → Select 2-3 questions → Click Next/Save |
| **Expected Result** | No error is thrown; user proceeds to next step |
| **Pass Criteria** | Flow completes without error messages |

---

### TC-04: End-to-End QR Code Generation

| Field | Detail |
|---|---|
| **Test ID** | TC-QR-001 |
| **Priority** | P0 — Critical |
| **Precondition** | User logged in to Speaker Mode |
| **Steps** | 1. Create Talk (fill all required fields) → Select template → Add general questions → Add resources → Save → Generate QR Code |
| **Expected Result** | QR code is generated and displayed successfully |
| **Pass Criteria** | QR code is visible, downloadable, and scannable |

---

### TC-05: Template Persistence Across Sessions

| Field | Detail |
|---|---|
| **Test ID** | TC-TMPL-003 |
| **Priority** | P1 — High |
| **Steps** | 1. Create and save a custom template → Log out → Log back in → Go to Create Talk → Check dropdown |
| **Expected Result** | Custom template is still present after logout/login |
| **Pass Criteria** | Template persists across sessions |

---

## 10. PROCESS IMPROVEMENT RECOMMENDATIONS

### Immediate (Before Next Deployment)
1. **QA Sign-Off Gate:** No backend code goes to Production without written QA approval
2. **Pre-Deployment Checklist:** Dev team to share list of changed APIs/features; QA to test each before release
3. **Smoke Test Suite:** A minimum smoke test covering 10 core flows must be run post every deployment

### Short-Term (Within 2 Weeks)
4. **Staging Environment Mandatory:** All backend changes must be tested on Staging first — Production deployment only after Staging sign-off
5. **Deployment Window:** Schedule deployments during low-traffic hours (not business hours)
6. **Rollback Plan:** Every deployment must have a documented rollback procedure ready before going live

### Medium-Term (Within 1 Month)
7. **Regression Test Suite:** Build a formal regression checklist covering all critical user flows for both Speaker and Organizer modes
8. **Bug Reporting Process:** Standardize how issues are reported, logged, and tracked (JIRA / Linear / equivalent)
9. **Release Notes:** Dev to provide release notes to QA for every deployment stating what changed

---

## 11. BUG LOG SUMMARY

| Bug ID | Issue | Severity | Status | Feature |
|---|---|---|---|---|
| BUG-001 | Admin templates missing from dropdown | 🔴 Critical | Open | Talk Creation → Template |
| BUG-002 | Custom template not appearing after save | 🔴 Critical | Open | Talk Creation → Custom Template |
| BUG-003 | General Questions error blocks QR generation | 🔴 Critical | Open | Talk Creation → QR Code |

---

## 12. SIGN-OFF

| Role | Name | Action | Date |
|---|---|---|---|
| QA Tester | Shivam | RCA Prepared | 11 June 2026 |
| Backend Developer | Pankaj | Rollback Initiated | 10 June 2026 |
| Team Lead | Ankesh | Notified User | 10 June 2026 |
| Verified by (post-fix) | Shivam | Pending QA Re-test | — |

---

*This RCA was prepared by Shivam (QA Tester) based on the incident reported on 10 June 2026. All process improvement recommendations are intended to prevent recurrence of similar production incidents.*
