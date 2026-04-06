# Provider Inbox — Cypress E2E Test Mapping

> Source: `webapps/provider-inbox/cypress/e2e/`
> Total test cases: **105**

---

## 1. error-handling-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | when a gql failure mid session - reverts sidebar filter; sort; and pill filter changes with an error toast; never redirects to error page | Simulates a GraphQL failure mid-session by forcing a network error on a subsequent GQL request. Verifies the sidebar filter reverts to its prior selection; the sort order reverts; pill filter changes revert; an error toast is displayed; and the user is never redirected to an error page. |

---

## 2. inbox-appointment-cancellation-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | cancel an appointment - starts and aborts cancellation modal | Opens an unconfirmed appointment card; clicks "Cancel Appointment"; verifies modal title and first step (patient agreement) are shown; clicks the close button and confirms the modal closes. |
| 2 | cancel an appointment - step 1 - patient agreement | Starts cancellation flow; verifies step 1 heading ("Did you ask this patient..."); checks "Yes" and "No" radio options exist; selects "Yes" and advances to step 2. |
| 3 | cancel an appointment - step 2 - requester | Verifies step 2 heading ("Who is requesting..."); checks radio options for "Patient" and "Practice"; selects "Patient" and advances to step 3. |
| 4 | cancel an appointment - step 3 - reason | Verifies step 3 heading ("Why are they cancelling?"); confirms multiple reason radio options are shown; selects "Scheduling conflict" and advances to step 4. |
| 5 | cancel an appointment - step 4 - tell us more (and submits) | Verifies step 4 heading ("Tell us more"); enters freeform text; clicks "Cancel Appointment" button; confirms modal closes; verifies success toast "Appointment cancelled" and card status updates to cancelled. |
| 6 | cancel an appointment - fires correct metrics | Walks through the full 4-step cancellation flow and verifies V2 action metrics fire at each step: open modal; step 1 selection; step 2 selection; step 3 selection; step 4 submission. |
| 7 | cancel an appointment - visit reason abatement - flag off - does not show visit reason abatement | With ProviderInterface_Cancellation_VisitReasonFlow flag OFF; completes step 3 (reason) and verifies it goes directly to step 4 (tell us more) without any abatement step. |
| 8 | cancel an appointment - visit reason abatement - flag on - role is not gated - does not show visit reason abatement | With flag ON but user role not in the gated list; verifies abatement is not shown after step 3. |
| 9 | cancel an appointment - visit reason abatement - flag on - reason is "Scheduling conflict" - requester is "Patient" - shows abatement | With flag ON and gated role; selects "Patient" requester and "Scheduling conflict" reason; verifies visit reason abatement step appears between steps 3 and 4. |
| 10 | cancel an appointment - visit reason abatement - flag on - reason is "Scheduling conflict" - requester is "Practice" - shows abatement | Same as above but with "Practice" as requester; verifies abatement still appears. |
| 11 | cancel an appointment - visit reason abatement - flag on - reason is "Visit reason" - requester is "Patient" - shows abatement | Selects "Visit reason" and "Patient"; verifies abatement step is shown. |
| 12 | cancel an appointment - visit reason abatement - flag on - reason is "Visit reason" - requester is "Practice" - does not show abatement | Selects "Visit reason" and "Practice"; verifies abatement is NOT shown (practice-initiated visit reason cancellations skip abatement). |
| 13 | cancel an appointment - visit reason abatement - flag on - reason is "Insurance" - does not show visit reason abatement | Selects "Insurance" reason; verifies visit reason abatement is not shown (insurance has its own abatement flow). |
| 14 | cancel an appointment - visit reason abatement - flag on - reason is "Other" - does not show visit reason abatement | Selects "Other" reason; verifies visit reason abatement is not shown. |
| 15 | cancel an appointment - visit reason abatement - flag on - reason is "No longer needed" - does not show visit reason abatement | Selects "No longer needed" reason; verifies visit reason abatement is not shown. |
| 16 | cancel an appointment - visit reason abatement - definitely mismapped - shows abatement with correct content | Selects a definitely-mismapped scenario; verifies the abatement step shows "visit reason" specific messaging with a "Send a message" action for the patient. |
| 17 | cancel an appointment - visit reason abatement - maybe mismapped - shows abatement with correct content | Selects a maybe-mismapped scenario; verifies the abatement shows appropriate messaging with options to contact the patient. |
| 18 | cancel an appointment - visit reason abatement - fires correct metrics | Walks through the visit reason abatement flow and verifies V2 metrics fire for the abatement step interactions. |
| 19 | cancel an appointment - insurance abatement - flag on - role is not gated - does not show insurance abatement | With insurance abatement flag ON but user role not gated; selects "Insurance" reason; verifies insurance abatement is not shown. |
| 20 | cancel an appointment - insurance abatement - flag on - in-network - shows abatement | With flag ON and gated role; for an in-network appointment with "Insurance" reason; verifies insurance abatement step appears with in-network messaging. |
| 21 | cancel an appointment - insurance abatement - flag on - out-of-network - shows abatement | Same as above but for out-of-network appointment; verifies abatement shows out-of-network specific messaging. |
| 22 | cancel an appointment - insurance abatement - send intake reminder via abatement | During insurance abatement; clicks "Send intake reminder" action; verifies the reminder is sent and a success toast appears. |
| 23 | cancel an appointment - set up intake modal - shown on 1st cancellation for insufficient info | On the user's very first cancellation where reason is insufficient info; after completing cancellation; verifies the "Set Up Intake" modal appears prompting the user to configure intake collection. |
| 24 | cancel an appointment - set up intake modal - shown on 6th cancellation for insufficient info | On the 6th cancellation (counter tracked); verifies the "Set Up Intake" modal appears again as a periodic reminder. |
| 25 | cancel an appointment - set up intake modal - not shown on 2nd-5th cancellation | On cancellations 2 through 5; verifies the "Set Up Intake" modal does NOT appear. |
| 26 | cancel an appointment - one-click insurance removal modal - shows modal | After cancelling for insurance reason on an appointment with insurance; verifies a one-click insurance removal confirmation modal appears. |
| 27 | cancel an appointment - one-click insurance removal modal - confirm removal | In the one-click insurance removal modal; clicks confirm; verifies insurance is removed and success toast shows. |
| 28 | cancel an appointment - one-click insurance removal modal - decline removal | In the one-click insurance removal modal; clicks decline; verifies modal closes without removing insurance. |
| 29 | cancel an appointment - one-click insurance removal modal - fires correct metrics | Verifies V2 metrics fire for modal open; confirm click; and decline click. |
| 30 | cancel an appointment - abatement - send intake request via abatement | During abatement flow; clicks "Send intake request"; verifies intake request is sent and success toast appears. |

---

## 3. inbox-appointment-card-part1-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | forms and ID card display | Opens an appointment card and verifies that submitted forms (intake forms) and insurance ID card images are displayed in the card details section. |
| 2 | initial intake request - desktop | On desktop viewport; opens an appointment card; clicks "Request Intake"; verifies the intake request is sent and status updates to "Intake Requested". |
| 3 | initial intake request - mobile | On mobile (iphone-x) viewport; opens an appointment card; clicks "Request Intake"; verifies the request succeeds on mobile layout. |
| 4 | send reminder | Opens an appointment card with existing intake request; clicks "Send Reminder"; verifies reminder is sent and success toast appears. |
| 5 | patient confirmation | Opens a confirmed appointment card; verifies the "Patient Confirmed" badge/status is displayed. |
| 6 | request a call | Opens an appointment card; clicks "Request a Call"; verifies the call request is initiated and UI updates accordingly. |
| 7 | print appointment card | Opens an appointment card; clicks the print button; verifies the print dialog/flow is triggered (uses window.print stub). |
| 8 | referral info - image present | Opens an appointment card with referral info that includes an image; verifies the referral image is displayed. |
| 9 | referral info - phone present | Opens an appointment card with referral info that includes a phone number; verifies the phone number is displayed. |
| 10 | referral info - practice name present | Opens an appointment card with referral info; verifies the referring practice name is displayed. |
| 11 | referral info - image absent | Opens an appointment card with referral info but no image; verifies the image placeholder or fallback is shown appropriately. |
| 12 | referral info - phone absent | Opens an appointment card with referral info but no phone; verifies the phone section is hidden or shows appropriate fallback. |
| 13 | referral info - practice name absent | Opens an appointment card with referral info but no practice name; verifies the practice name section is hidden or shows fallback. |
| 14 | insurance cards | Opens an appointment card; verifies front and back insurance card images are displayed correctly. |
| 15 | self-pay display | Opens an appointment card for a self-pay patient (no insurance); verifies "Self-Pay" label is shown instead of insurance details. |
| 16 | report a patient - full modal flow | Clicks "Report a Patient" on an appointment card; completes the full report modal (selecting reason; entering details); submits and verifies success. |
| 17 | delete intake submission | Opens an appointment card with an intake submission; clicks delete; confirms deletion in the confirmation dialog; verifies submission is removed. |
| 18 | go to calendar | Opens an appointment card; clicks "Go to Calendar" link; verifies navigation to the provider calendar page with correct appointment context. |
| 19 | view patient record | Opens an appointment card; clicks "View Patient Record" link; verifies navigation to the patient record page. |
| 20 | sync confirm | Opens a sync-confirmed appointment (SyncConfirmed status); verifies the card displays the synced confirmation state correctly. |
| 21 | provider notes - full CRUD | Opens an appointment card; creates a new provider note (verifies save); reads/views the note; edits the note text (verifies update); deletes the note (verifies removal). |
| 22 | collect intake - reminder and request and appointment list error | Tests the collect intake flow: sends a reminder; sends a new request; triggers the appointment list source error state; verifies appropriate error handling. |
| 23 | attachments display and download | Opens an appointment card with attachments; verifies attachments are listed; clicks download and verifies the download is triggered. |

---

## 4. inbox-appointment-card-part2-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | confirm appointment | Opens an unconfirmed appointment card; clicks "Confirm"; verifies appointment status updates to confirmed and success toast appears. |
| 2 | remove cancelled appointment | Opens a cancelled appointment card; clicks "Remove"; verifies the card is removed from the inbox list. |
| 3 | cancel appointment (full flow) | Opens an appointment card; walks through the full 4-step cancellation flow (patient agreement -> requester -> reason -> tell us more); submits; verifies cancellation success and card status update. |
| 4 | reschedule (SKIPPED) | Test is skipped (it.skip). Would test the reschedule appointment flow but is currently disabled. |
| 5 | close card via multiple methods | Opens an appointment card; closes it using the X button; reopens and closes using the Escape key; reopens and closes by clicking outside the card; verifies all close methods work. |

---

## 5. inbox-auto-refresh-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | auto-refresh with polling | Sets mockInboxAutoRefreshIntervalMs cookie to 1000ms (1 second); loads inbox; verifies new appointments appear automatically via polling without manual refresh; checks pagination updates when new items arrive; verifies sort order is maintained; confirms location pill filter does not change sidebar counts during refresh. |

---

## 6. inbox-filter-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | patient search metrics | Types a patient name in the search box; verifies V2 search metrics fire with correct event properties including search term and result count. |
| 2 | search and pill filter set and clear with localStorage verification | Sets a patient search term and selects provider/location pill filters; verifies results update; checks localStorage persists the filter selections; clears each filter; verifies results reset and localStorage is updated. |

---

## 7. inbox-intake-manual-request-flow-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | error clearing on exit | Opens manual intake request form; triggers field validation errors; closes the form; reopens it; verifies all previous errors are cleared. |
| 2 | field-level error clearing on type | Opens manual intake request form; triggers validation errors; starts typing in an errored field; verifies the error for that specific field clears while others remain. |
| 3 | successful submission | Opens manual intake request form; fills in all required fields (patient name; email/phone; date; time; visit reason); submits; verifies success toast and form closes. |
| 4 | customer RSVP submission | Opens manual intake request form; fills in fields; checks the "Customer RSVP" option; submits; verifies the RSVP-specific request is sent. |
| 5 | past date/time validation | Opens manual intake request form; enters a date/time in the past; attempts to submit; verifies validation error appears indicating date/time must be in the future. |
| 6 | comprehensive metrics | Opens manual intake request form; completes and submits; verifies V2 metrics fire for: form open; field interactions; submission; and success/failure outcomes. |

---

## 8. inbox-page-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | empty state | Loads inbox with no appointments; verifies empty state illustration and message ("No appointments to show") are displayed. |
| 2 | loading spinner | Loads inbox with delayed GQL response; verifies loading spinner appears while data is fetching; spinner disappears when data arrives. |
| 3 | pagination | Loads inbox with more appointments than one page; verifies pagination controls appear; clicks next page; verifies new appointments load; navigates back. |
| 4 | feedback link | Verifies the "Give Feedback" link is present in the inbox and points to the correct feedback URL. |
| 5 | footer calendar link | Verifies the footer contains a "Go to Calendar" link that navigates to the provider calendar page. |
| 6 | appointment flyout via query params - MARKETPLACE source | Loads inbox with query param specifying a MARKETPLACE appointment ID; verifies the appointment card flyout opens automatically for that appointment. |
| 7 | appointment flyout via query params - API source | Loads inbox with query param specifying an API-sourced appointment; verifies flyout opens for the correct appointment. |
| 8 | appointment flyout via query params - ManualIntake source | Loads inbox with query param for a ManualIntake appointment; verifies flyout opens. |
| 9 | appointment flyout via query params - AppointmentList source | Loads inbox with query param for an AppointmentList source appointment; verifies flyout opens. |
| 10 | API bookings link | Verifies the "API Bookings" navigation link is present and navigates to the API bookings page. |
| 11 | timezone handling - default | Loads inbox without a preferred timezone set; verifies appointment times display in the practice's default timezone. |
| 12 | timezone handling - preferred | Loads inbox with a preferred timezone cookie/setting; verifies appointment times display in the user's preferred timezone. |
| 13 | onboarding task completion | Loads inbox for a new user with onboarding tasks; completes an onboarding task (e.g.; views first appointment); verifies the task is marked complete in the onboarding checklist. |
| 14 | synced appointments | Loads inbox with synced appointments (from external calendar sync); verifies they display with the correct sync status indicators. |
| 15 | appointment list exclusion | Verifies that appointments marked for exclusion from the inbox list (e.g.; certain statuses) do not appear in the appointment list. |

---

## 9. inbox-routing-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | no JWT redirect | Accesses inbox without a JWT token; verifies the user is redirected to the login page. |
| 2 | expired JWT redirect | Accesses inbox with an expired JWT token; verifies the user is redirected to the login/re-authentication page. |

---

## 10. inbox-sidebar-filter-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | sidebar filters - All; New Bookings; Reschedules; Cancellations; Intake Submissions | Clicks each sidebar filter (All; New Bookings; Reschedules; Cancellations; Intake Submissions); verifies the appointment list updates to show only matching appointments; checks notification tags on each filter show correct counts; verifies clicking a different filter closes any open appointment card; tests Intake Submissions filter shows intake-specific view with delete capability. |

---

## 11. inbox-sort-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | notification date sort toggle | Clicks the notification date sort header; verifies appointments reorder by notification date; clicks again to toggle sort direction; verifies order reverses. |
| 2 | appointment date sort with persistence across filter changes | Selects appointment date sort; verifies order updates; switches sidebar filter; switches back; verifies sort preference persists across filter changes. |

---

## 12. inbox-table-row-read-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | row read state on click | Clicks an unread appointment row; verifies the row visual state changes from unread (bold) to read (normal weight); verifies the unread count in sidebar decrements. |
| 2 | read state via query param | Loads inbox with a query param pointing to a specific appointment; verifies that appointment's row is automatically marked as read. |
| 3 | correct notification tags | Loads inbox with appointments of various types; verifies each row shows the correct notification tag (New Booking; Reschedule; Cancellation; Intake Submission) matching the appointment's notification type. |

---

## 13. mobile-inbox-page-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | empty state | On mobile (iphone-x) viewport; loads inbox with no appointments; verifies mobile-specific empty state is displayed. |
| 2 | loading spinner | On mobile viewport; loads inbox with delayed response; verifies mobile loading spinner appears. |
| 3 | pagination with filters | On mobile viewport; loads inbox with multiple pages; applies a filter; verifies pagination works correctly with filtered results. |
| 4 | appointment card click | On mobile viewport; taps an appointment row; verifies the mobile appointment card expands/opens with full details. |
| 5 | query param flyout | On mobile viewport; loads inbox with appointment query param; verifies the appointment card opens automatically in mobile layout. |
| 6 | source param flyout | On mobile viewport; loads inbox with source query param; verifies the correct appointment opens based on source. |
| 7 | API source param | On mobile viewport; loads inbox with API source param; verifies the API-sourced appointment opens correctly in mobile view. |

---

## 14. tutorial-coachmark-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | 4-step tutorial (no appointments) | Loads inbox with no appointments and tutorial not completed; verifies 4 tutorial coachmark steps appear sequentially; each step highlights the correct UI element; completes all steps; verifies tutorial is marked done. |
| 2 | 6-step tutorial (with appointments) | Loads inbox with appointments and tutorial not completed; verifies 6 tutorial coachmark steps (additional steps for appointment-specific features); completes all steps. |
| 3 | URL-specified appointment on step 4 | Loads inbox with a query param appointment during tutorial; verifies tutorial reaches step 4 and the specified appointment is highlighted/opened. |
| 4 | guided tour button restart | Completes the tutorial; clicks "Guided Tour" button; verifies the tutorial restarts from step 1. |
| 5 | edit button coachmark hidden during tutorial | During an active tutorial; verifies the edit button coachmark (which normally shows independently) is hidden to avoid overlapping guidance. |

---

## 15. smoke/inbox-tests.ts

| # | Test Name | Summary |
|---|-----------|---------|
| 1 | Full Admin smoke test | End-to-end smoke test using real external Zocdoc developer API; logs in as Full Admin role; loads inbox; verifies appointments are fetched and displayed; checks core UI elements are functional. |
| 2 | Appointment Management smoke test | End-to-end smoke test using real external API; logs in with Appointment Management role; loads inbox; verifies role-appropriate appointments and UI elements are displayed. |
