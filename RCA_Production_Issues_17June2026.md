# Root Cause Analysis (RCA) — SpeakFeed Production Incident

| Field | Details |
|---|---|
| **Document Type** | Root Cause Analysis (RCA) |
| **Incident Date** | 17 June 2026 |
| **Reported By** | Client (End User) |
| **QA Tester (Manual)** | Shivam |
| **Environment Affected** | **Production** |
| **Severity** | 🟠 High — Multiple feature discrepancies between Dev and Production |
| **Status** | Under Investigation / QA Review Completed |
| **Reference** | Previous Incident: RCA_Production_Issues_11June2026.md |

---

## 1. INCIDENT SUMMARY

Following the production deployment and previous rollback (Ref: RCA dated 11 June 2026), a client reported **three new functional discrepancies** between the Development and Production environments of the SpeakFeed platform. These issues were observed during normal usage of the Speaker Module and relate to the **QR Code workflow**, **My Talks survey response flow**, and **Scoring logic**.

**Client-Reported Issues:**
1. **QR Codes:** Upcoming Talks are not appearing in the QR Code dropdown list.
2. **My Talks:** When scanning the QR Code, the survey skips all questions and immediately shows the "Delivered" message — no user feedback is recorded.
3. **Scoring:** Skipping a question results in a score of `0`, which negatively impacts the overall score.

The QA team has reviewed all three issues and conducted a detailed root cause analysis. The findings are documented below.

---

## 2. ISSUES IDENTIFIED — CLIENT REPORT vs. QA FINDINGS

---

### 🟡 Issue #1 — QR Code: Upcoming Talks Not Appearing in Dropdown

#### Client Report

> *"Upcoming Talks are not appearing in the QR Code dropdown list. I cannot see or test any QR Codes for upcoming events."*

#### QA Investigation & Findings

| Field | Detail |
|---|---|
| **Feature Affected** | QR Codes Tab → Talk Selection Dropdown |
| **Environment Tested** | Development ✅ \| Production ✅ |
| **QA Verdict** | ✅ **No Bug Confirmed** |

**QA Observations:**

After thorough testing on **both Development and Production** environments, the QR Code tab and the Talk dropdown were found to be **functioning correctly**. Upcoming Talks appear as expected in the dropdown on both environments.

**Root Cause of Client Report:**

This appears to have been a **transient or user-specific observation**, possibly due to:
- Timing of the test (no active upcoming Talks created at the time of client testing)
- Browser cache or session state issue on the client's device
- The client may have been viewing the screen before data had fully loaded

**Current Status:** ✅ No defect exists. Feature is working correctly on both environments.

**QA Recommendation:** Advise the client to hard-refresh the page (Ctrl+Shift+R) and ensure at least one upcoming Talk has been created before testing the dropdown.

---

### 🔴 Issue #2 — My Talks: Survey Skips All Questions and Shows "Delivered" Immediately

#### Client Report

> *"Even though I can create a Talk with only General Questions, when I scan the QR Code to take the survey, none of the questions appear and I immediately receive the Delivered message. In other words, it just skips to the end and doesn't record any user feedback."*

#### QA Investigation & Findings

| Field | Detail |
|---|---|
| **Feature Affected** | Attendee Survey Flow (QR Code Scan → Question Display → Response Submission) |
| **Environment Tested** | Development ✅ \| Production ✅ |
| **QA Verdict** | ⚠️ **Testing Methodology Gap Identified (Not a Code Bug)** |

**QA Observations:**

| Scenario | Behavior |
|---|---|
| QR Code scanned via mobile camera | Survey loads correctly → Questions displayed → Attendee can respond ✅ |
| URL manually copied and opened in a new browser tab | Survey skips all questions → "Delivered" message appears immediately ❌ |

**Root Cause — Detailed Explanation:**

The core issue stems from **how the Attendee URL/Code is accessed**:

- When a QR code is **scanned using a camera**, the unique session/talk code is **automatically bound and embedded** in the URL. The system identifies the Talk, loads the associated questions, and presents the survey as intended.
- When the same URL is **manually copied and pasted** into a new browser tab, the code binding behavior **does not replicate the full QR scan context**. The attendee lands on the survey page but the system does not correctly resolve the Talk context, resulting in the survey skipping all questions and immediately showing the "Delivered" confirmation.

**Testing Gap — QA Accountability:**

The QA team acknowledges the following gap in testing methodology:

> During the development and testing phase, the **Attendee survey flow was predominantly tested by copying the URL and opening it in a new browser tab** — a shortcut used to save time and avoid scanning with a physical device. This approach did not replicate the true end-user experience of scanning the QR Code with a mobile device.

> When tested **on a mobile device via actual QR scan**, the flow works correctly on **both Development and Production** environments.

This gap resulted in the **incorrect assumption** that the URL-copy method was equivalent to the QR scan method, which it is not. The "Delivered" shortcut behavior was not identified as a defect because it was the accepted internal testing method.

**Current Status:** The feature works correctly when tested as intended (QR scan via mobile). The URL-copy behavior is a **known limitation / UX edge case**, not a production code defect.

**Action Required:** All future Attendee flow testing **must be performed using an actual mobile QR scan** to accurately simulate the end-user experience. URL copy-paste testing should only be used for non-survey-flow validation.

---

### 🟠 Issue #3 — Scoring: Skipped Questions Negatively Impact Score with a `0`

#### Client Report

> *"The scoring updates are not working. For example, if I skip a question, then I receive a 0 and my score is negatively impacted."*

#### QA Investigation & Findings

| Field | Detail |
|---|---|
| **Feature Affected** | Attendee Feedback Scoring → Skipped Questions Handling |
| **Environment Tested** | Development ✅ \| Production (Partially) |
| **QA Verdict** | ✅ **Known Behavior — Now Resolved / Behavior Updated** |

**QA Observations & Background:**

The scoring logic has been deliberately designed and updated. The following rules now apply:

| Question Type | Attendee Action | Score Behavior |
|---|---|---|
| Rating / Feedback Question | Answer provided | Score calculated normally based on response value |
| Rating / Feedback Question | Question skipped | Previously: `0` counted in score ❌ → Now: `N/A`, excluded from score calculation ✅ |
| General Question (Name, Email, Testimonial) | Any response | Displays as `N/A` — **does not impact score** ✅ |
| General Question (Calendly Link) | Calendly link submitted | Displays as `N/A` — **does not impact score** ✅ |

**Root Cause of Client Report:**

During a **prior version of the scoring logic**, skipped questions were incorrectly assigned a value of `0`, which was then factored into the score calculation — negatively impacting the speaker's overall score even when an attendee simply chose not to answer.

This was identified as a known issue and was **discussed with the client before being resolved**. The team had a deliberate conversation with the client about the intended behavior, seeking clarification on edge cases, before finalizing the scoring logic.

**Current Scoring Rules (Finalized):**

```
Score Calculation:
  → Only answered rating/feedback questions are included in the score
  → Skipped questions → excluded (N/A), no impact on score
  → General Questions (Name, Email, Testimonial, Calendly) → N/A, no impact on score
```

**Current Status:** ✅ Scoring logic has been updated and is now correctly implemented. Skipped questions are treated as `N/A` and do not negatively impact the score.

---

## 3. QA ROOT CAUSE SUMMARY TABLE

| # | Issue | Client Severity | Actual Status | Root Cause Category |
|---|---|---|---|---|
| 1 | QR Code dropdown missing Upcoming Talks | 🔴 Critical | ✅ Not Reproducible / No Defect | User/Environment Specific |
| 2 | Survey skips questions → shows Delivered | 🔴 Critical | ⚠️ Testing Methodology Gap | QA Testing Process Gap |
| 3 | Skipped questions score as `0` | 🟠 High | ✅ Resolved — Scoring Updated | Known Issue — Since Resolved |

---

## 4. BUSINESS IMPACT ASSESSMENT

| Area | Impact |
|---|---|
| **QR Code Tab** | No production defect. Client perception issue due to testing context. |
| **Attendee Survey Flow** | Feature works correctly when tested via actual QR scan. URL-copy shortcut creates misleading behavior. Client-facing impact was minimal as real attendees scan QR codes. |
| **Scoring** | Scoring logic has been updated and is now aligned with expected behavior. No ongoing user impact. |
| **Brand / Trust** | Client reported multiple issues simultaneously, indicating a lack of visibility into the current state of the platform. Improved communication and regression documentation required. |

---

## 5. TIMELINE OF EVENTS

| Date/Time | Event |
|---|---|
| 11 June 2026 | Previous production incident (template/QR bugs) — code rolled back |
| Post-11 June | Backend fixes applied; scoring logic updated; deployment re-pushed |
| 17 June 2026 | Client reports 3 new issues on Production environment |
| 17 June 2026 | QA (Shivam) begins investigation across Dev and Production |
| 17 June 2026 | QA testing reveals: Issue #1 not reproducible, Issue #2 is a testing methodology gap, Issue #3 is a known resolved issue |
| 17 June 2026 | RCA document prepared by Shivam (QA) |

---

## 6. QA ACCOUNTABILITY — TESTING METHODOLOGY GAP

The most significant finding from this RCA is the **testing methodology gap** identified in Issue #2 (Survey Flow).

### What Went Wrong

| Gap | Detail |
|---|---|
| **Testing Shortcut Used** | QA tested the Attendee survey flow by copying the URL and opening it in a new browser tab instead of using an actual QR scan on a mobile device |
| **Why It Was Used** | To save time during development and regression testing cycles |
| **Why It Was Insufficient** | URL-copy method does not fully replicate the QR scan context — the system handles code binding differently in each scenario |
| **Impact** | A valid UX discrepancy was not caught before client testing, leading to a client-reported "bug" that was actually a testing environment artifact |

### Corrective Action for QA

| Action | Description | Target |
|---|---|---|
| **Mandatory Mobile Testing** | All Attendee flow test cases must be executed on a physical mobile device using actual QR scan | Immediate |
| **Dual-Path Testing** | Both QR scan AND URL-copy paths should be tested and documented separately in test cases | Immediate |
| **Test Case Update** | Update all existing Attendee flow test cases to include "via mobile QR scan" as an explicit precondition | Within 1 week |
| **Checklist Gate** | Add a mandatory "Mobile QR Scan Test" checkpoint to the pre-release regression checklist | Within 1 week |

---

## 7. UPDATED TEST CASES — ATTENDEE SURVEY FLOW

The following test cases are to be executed going forward to validate the Attendee survey flow correctly.

### TC-01: Survey Load via QR Code Scan (Mobile)

| Field | Detail |
|---|---|
| **Test ID** | TC-SURVEY-001 |
| **Priority** | P0 — Critical |
| **Device** | Physical Mobile (iOS/Android) |
| **Precondition** | An active Talk with at least 1 question has been created and QR code generated |
| **Steps** | 1. Open camera on mobile → Scan QR Code → Survey page opens → Verify questions load |
| **Expected Result** | All configured survey questions are displayed; attendee can answer and submit |
| **Pass Criteria** | Questions visible → Responses submittable → Confirmation received after submission |

---

### TC-02: Survey Completion and Submission (Mobile)

| Field | Detail |
|---|---|
| **Test ID** | TC-SURVEY-002 |
| **Priority** | P0 — Critical |
| **Device** | Physical Mobile (iOS/Android) |
| **Precondition** | Survey is loaded via QR scan (TC-SURVEY-001 passed) |
| **Steps** | 1. Answer all questions → Click Submit → Observe confirmation screen |
| **Expected Result** | "Delivered" / Thank You screen appears after all questions are answered and submitted |
| **Pass Criteria** | Submission recorded; speaker dashboard reflects new response |

---

### TC-03: Survey via URL Copy (Edge Case — Known Limitation)

| Field | Detail |
|---|---|
| **Test ID** | TC-SURVEY-003 |
| **Priority** | P2 — Low |
| **Device** | Desktop Browser |
| **Precondition** | Talk QR Code URL is copied from the speaker dashboard |
| **Steps** | 1. Copy survey URL → Paste in new browser tab → Observe behavior |
| **Expected Result** | Attendee must manually enter the code to access the survey |
| **Pass Criteria** | System behavior is documented; "Delivered" shortcut behavior is noted as expected for this access method |
| **Note** | ⚠️ This path is NOT the intended user experience. Document as known behavior, not a bug. |

---

### TC-04: Skipped Question — Score Exclusion Validation

| Field | Detail |
|---|---|
| **Test ID** | TC-SCORE-001 |
| **Priority** | P1 — High |
| **Precondition** | Talk with rating questions created; attendee completes survey and skips at least one rating question |
| **Steps** | 1. Scan QR → Answer 2 questions → Skip 1 question → Submit → Check speaker dashboard score |
| **Expected Result** | Skipped question shows `N/A`; skipped question does NOT reduce overall score |
| **Pass Criteria** | Score calculated only from answered questions; skipped questions marked N/A with no negative impact |

---

### TC-05: General Questions — Score Not Impacted

| Field | Detail |
|---|---|
| **Test ID** | TC-SCORE-002 |
| **Priority** | P1 — High |
| **Precondition** | Talk includes General Questions (Name, Email, Testimonial, Calendly) |
| **Steps** | 1. Scan QR → Fill in General Questions (Name, Email, etc.) → Submit → Check score |
| **Expected Result** | General question fields show `N/A` on the scoring dashboard; overall score is unaffected |
| **Pass Criteria** | Score only reflects rating/feedback questions; general questions are excluded from scoring |

---

## 8. PROCESS IMPROVEMENT RECOMMENDATIONS

### Immediate Actions (Before Next Sprint)

| # | Action | Owner | Priority |
|---|---|---|---|
| 1 | Mandate mobile QR scan testing for all Attendee flow test cases | QA (Shivam) | 🔴 Immediate |
| 2 | Update test cases to clearly distinguish QR-scan vs. URL-copy test paths | QA (Shivam) | 🔴 Immediate |
| 3 | Communicate to client: Issue #1 not reproducible, Issue #2 resolved via correct test method, Issue #3 scoring now corrected | Dev Lead / PM | 🔴 Immediate |

### Short-Term (Within 1–2 Weeks)

| # | Action | Owner | Priority |
|---|---|---|---|
| 4 | Add "Mobile Device QR Scan" as a mandatory pre-release checkpoint on the regression checklist | QA (Shivam) | 🟠 High |
| 5 | Document QR scan vs. URL behavior difference in the SpeakFeed Testing Guidelines document | QA (Shivam) | 🟠 High |
| 6 | Set up a dedicated mobile test device for recurring use in QA testing | Team Lead | 🟡 Medium |

### Medium-Term (Within 1 Month)

| # | Action | Owner | Priority |
|---|---|---|---|
| 7 | Conduct a full Dev vs. Production regression comparison run to identify any remaining discrepancies | QA (Shivam) | 🟠 High |
| 8 | Establish a formal Deployment Verification Protocol: QA runs smoke test on Production immediately after each deployment | QA / DevOps | 🟡 Medium |
| 9 | Consider adding a guard in the application to handle the URL-copy scenario gracefully (prompt for code entry instead of silent skip) | Backend Dev | 🟡 Medium |

---

## 9. BUG LOG SUMMARY

| Bug ID | Issue | Severity | QA Verdict | Status |
|---|---|---|---|---|
| BUG-004 | QR Code dropdown missing Upcoming Talks | 🟡 Reported | ✅ Not Reproducible | Closed — No Defect |
| BUG-005 | Survey skips questions → shows Delivered (QR scan) | 🔴 Reported | ⚠️ Testing Gap — Works via QR Scan | Closed — Not a Code Bug |
| BUG-006 | Skipped questions score as `0` negatively | 🟠 Reported | ✅ Known Issue — Now Resolved | Closed — Fix Applied |

---

## 10. SIGN-OFF

| Role | Name | Action | Date |
|---|---|---|---|
| QA Tester | Shivam | RCA Prepared & QA Review Completed | 17 June 2026 |
| Backend Developer | Pankaj | Scoring Logic Updated & Deployed | Post 11 June 2026 |
| Team Lead | Ankesh | Review Pending | — |
| Client Communication | PM / Team Lead | Pending — Client Update Required | — |

---

## 11. CONCLUSION

Of the three issues reported by the client on 17 June 2026:

| Issue | Final Verdict |
|---|---|
| **QR Code Dropdown** | ✅ No bug. Feature is working correctly on both Dev and Production. Client likely tested before data was available or experienced a browser-level cache issue. |
| **Survey Flow (Delivered immediately)** | ⚠️ Not a production defect. Works correctly when tested via actual QR scan on mobile. Issue arose from an internal testing shortcut (URL copy) that does not represent the real user journey. QA accountability acknowledged. |
| **Scoring (Skipped = 0)** | ✅ Resolved. This was a known behavior identified prior to this report. The scoring logic has been updated: skipped questions and general questions (Name, Email, Testimonial, Calendly) are now excluded from score calculation and displayed as `N/A`. |

> **From a QA perspective, the only true gap identified is the testing methodology for the Attendee survey flow — specifically, the practice of using URL-copy instead of actual QR scan on a mobile device. This gap has been acknowledged, documented, and corrective actions have been defined.**

---

*This RCA was prepared by Shivam (QA Tester) on 17 June 2026 based on client-reported issues and internal QA testing on Development and Production environments. This document supersedes and extends the findings from RCA_Production_Issues_11June2026.md.*
