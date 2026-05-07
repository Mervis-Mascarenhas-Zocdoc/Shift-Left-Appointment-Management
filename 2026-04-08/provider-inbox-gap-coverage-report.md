# Provider Inbox — Test Gap Coverage Report

**Date:** 2026-04-08
**Scope:** Unit tests (106) + Cypress E2E tests (~105) for `webapps/provider-inbox`
**Source:** `frontend-monorepo/webapps/provider-inbox/src/pages/Inbox/`

---

## Executive Summary

| Metric | Unit Tests | E2E Tests |
|--------|-----------|-----------|
| Total tests analyzed | 106 | ~105 |
| Relevant | **105 (99%)** | **~95 (90%)** |
| Irrelevant / Stale | 1 partially stale | 1 skipped, ~8 mapping entries not in actual test files |
| High-priority missing gaps | 7 | 3 |
| Medium-priority missing gaps | 12 | 12 |

### E2E Mapping Accuracy Note

The E2E test mapping document lists 105 test cases, but several entries (tests #21, #23-30 in cancellation section, test #23 in card-part1) describe tests that **do not exist** in the actual test files. The mapping appears to include aspirational/planned tests alongside actual ones. Actual test files contain approximately 80-85 unique `it()` blocks.

---

## Part 1: Unit Test Analysis

### 1.1 BookingRowCellComponents-tests.tsx (27 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | ProviderName component renders name and avatar | ProviderName component (avatar logic, viewport threshold) has no direct unit test. | Medium |
| 2 | LastUpdatedDate with isShortFormat prop | `isShortFormat` triggers short labels ("1m ago" vs "1 min ago") — never tested. | Medium |
| 3 | PatientName component renders full name | PatientName component is exported/used but has no direct unit test. | Low |
| 4 | AppointmentDate fallback for missing dateTime | Fallback "--/--/--" for missing dateTime/practiceTimeZone (line 100-101). | Low |
| 5 | EHR error does NOT show for Confirmed + hasEhrError=true | Only Unconfirmed + hasEhrError=true is tested. | Low |

---

### 1.2 BookingsTable-tests.tsx (22 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | `getStatusString` maps each AppointmentStatus correctly | Exported function used for status column rendering — **zero tests**. | **High** |
| 2 | Row selection highlighting (isSelected logic) | `isSelected` logic based on `isFlyoutOpen` and `selectedAppointmentId`. | Medium |
| 3 | Notification tags display in table row | NotificationsColumn / `getNotificationTags` integration. | Medium |
| 4 | Empty table renders EmptyBookingsView | `emptyTableContent` prop rendering when appointments is empty. | Low |

---

### 1.3 EmptyBookingsView-tests.tsx (6 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Mobile vs Desktop styling variation | Component uses `useIsSmallBreakpoint()` for `$isDesktop` styling. | Low |
| 2 | "No bookings found" text always displays | Basic rendering test for title text. | Low |

---

### 1.4 ErrorView-tests.tsx (1 test)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | "Return to Inbox" button click triggers `window.location.assign` | Button onClick calls `window.location.assign` with practice ID — untested. | **High** |
| 2 | Renders error text and Navbar | Basic content rendering + Navbar with `inboxNotificationCount` prop. | Medium |

---

### 1.5 InboxPageContainer-tests.tsx (9 tests)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| N/A (mock setup) | `shouldShowDetabbed` mock | `shouldShowDetabbed` selector no longer exists in `src/config/ab/selector.ts`. Mock is dead code — harmless but stale infrastructure. |

> All 9 actual test assertions remain relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | `markAsRead` called when clicking an unread row | `shouldMarkAsRead`/`markAsRead` logic for read status is untested. | **High** |
| 2 | URL parameter `openIntake=true` opens intake request flyout | `shouldOpenIntakeRequestFlyout` URL param logic. | Medium |
| 3 | URL parameter `reminderAppointmentId` opens reminder flyout | `appointmentIdFromReminderUrl` logic. | Medium |
| 4 | Auto-refresh via `useInterval` fires `refreshInboxRowData` | Auto-refresh polling mechanism. | Medium |
| 5 | MPLDropdown visibility based on `isInternalUser` and `isMobile` | `shouldShowMPLDropdown` computed from `!isInternalUser && !isMobile`. | Low |

---

### 1.6 InboxPageManualIntakeRequestFlow-tests.tsx (6 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Manual intake button hidden for non-FullAdmin when intake IS activated | Tests only cover non-admin hiding the "Get Intake" button, not the manual request button. | Medium |
| 2 | Flyout overlay and form validation on submit | Form submission behavior untested. | Low |

---

### 1.7 InboxPageManualIntakeRequestFlyoutCloseActions-tests.tsx (4 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | "Are you sure" modal appears when form has dirty input | All 3 close-action tests verify modal does NOT appear for empty forms, but no test verifies it DOES appear for dirty forms. | **High** |
| 2 | Flyout close animation restores manual-intake-request-btn | Only tested for overlay click close, not other close methods. | Low |

---

### 1.8 InboxPageView-tests.tsx (6 tests)

**Irrelevant Tests:** None — all 6 tests (3 desktop + 3 mobile) are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | AppointmentCard renders when `selectedAppointmentIdentifier` is set | `shouldShowAppointmentCard` logic (line 137-138) is untested. | **High** |
| 2 | MPLDropdown renders when `shouldShowMPLDropdown` is true | MPL dropdown conditional rendering. | Medium |
| 3 | ManualIntakeFlyout responds to `isIntakeFlyoutOpen` | ManualIntakeFlyout rendering. | Medium |
| 4 | Tutorial Metrics context wraps Tutorial (desktop only) | Tutorial rendering branch for non-mobile. | Low |

---

### 1.9 InboxSidebar-tests.tsx (5 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Sidebar count badges display when count > 0 | Conditional `!!item.count &&` badge rendering. | Medium |
| 2 | Sidebar count badges hidden when count is 0 or undefined | Negative case for badge visibility. | Low |
| 3 | Selected filter has bold styling / visual indicator | `$isSelected` styled prop controls bold text and background. | Low |
| 4 | `getDisplayText` utility function | Exported function used by InboxPageView. | Low |

---

### 1.10 MobileInboxView-tests.tsx (3 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Filters button opens MobileInboxFiltersModal | `handleOpenFilters` callback and modal integration. | **High** |
| 2 | Pagination renders when `pagesCount > 0` | Pagination component conditional rendering. | Medium |
| 3 | Status filter chips in MobileInboxHeaderFilters | MobileInboxHeaderFilters component receiving status filter props. | Medium |
| 4 | Sticky header behavior with IntersectionObserver | `isStuck` state driving box-shadow. | Low |
| 5 | Navbar renders with `unconfirmedCountForNavbar` | Navbar prop from data. | Low |

---

### 1.11 MPLDropdown-tests.tsx (17 tests)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 1 | "not visible if hidden" | **Partially stale**: Button now renders `currentPracticeName ?? 'Other practices'`. Test doesn't mock `getPagePropsPractice()` so falls back to "Other practices" — still passes but doesn't reflect production behavior. |

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Dropdown button shows `currentPracticeName` when available | `currentPracticeName ?? 'Other practices'` logic with a mocked practice name. | **High** |
| 2 | Practice item click calls `switchPracticeForInbox` and navigates | onClick handler calls `switchPracticeForInbox` then `window.location.assign`. | **High** |
| 3 | Notification count displays "99+" when > 99 | `totalNotifications > 99 ? '99+' : totalNotifications` logic. | Medium |
| 4 | Outside click closes the dropdown | `useOutsideClickHandler` hook. | Medium |
| 5 | Notification count badge hidden when totalNotifications is 0 | Conditional `totalNotifications > 0 &&` rendering. | Low |
| 6 | "No results" text shows when search yields no matches | `items.length === 0` case in popover. | Low |

---

## Part 2: Cypress E2E Test Analysis

### 2.1 error-handling-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | GQL failure on initial page load | ErrorView renders on initial GQL failure (not just mid-session). | Medium |
| 2 | Error toast dismissal behavior | Toast can be dismissed and does not persist. | Low |

---

### 2.2 inbox-appointment-cancellation-tests.ts

**Irrelevant Tests:**

| Mapping Tests | Issue |
|--------------|-------|
| #23-25 (SetUpIntakeModal on 1st/6th/2nd-5th cancellation) | **Not in actual test file** — mapping entries are aspirational/planned. |
| #26-29 (one-click insurance removal modal) | **Not in actual test file** — no `data-test` for `one-click-insurance-removal` exists in codebase. |
| #30 (send intake request via abatement) | **Not in actual test file.** |

> All actual `it()` blocks in the file are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | SetUpIntakeModal after cancellation | Modal appears on 1st and 6th cancellations with insufficient info reason. Component exists at `practice-appointment-card`. | Medium |
| 2 | Insurance abatement send-intake-reminder action | Clicking "Send intake reminder" from insurance abatement step. | Medium |
| 3 | Cancellation from a SyncConfirmed appointment | Cancellation flow for sync-sourced appointments. | Low |

---

### 2.3 inbox-appointment-card-part1-tests.ts

**Irrelevant Tests:** None — all actual tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Attachments display and download | Listed in mapping but not present in test file. Attachment feature may exist but lacks coverage. | Medium |
| 2 | Insurance eligibility display | `shouldShowInsuranceEligibilityOnAppointmentCard` prop exists. | Medium |
| 3 | Provider notes CRUD | Listed in mapping but absent from actual tests. | Medium |
| 4 | View Patient Record navigation | Listed in mapping but absent from actual tests. | Low |

---

### 2.4 inbox-appointment-card-part2-tests.ts

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 4 (reschedule) | Reschedule appointment | **Permanently SKIPPED** (`it.skip`). References `ProviderInterface_ShouldUseNewModifyAppointmentModal` which is not in inbox experiment config. |

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Close flyout by clicking on empty table area | Only search field and sort clicks tested. | Low |

---

### 2.5 inbox-auto-refresh-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Auto-refresh with open appointment card | Card stays open and data refreshes during auto-refresh. | Medium |
| 2 | Auto-refresh after GQL failure | Polling continues/recovers after transient error. | Low |

---

### 2.6 inbox-filter-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Location pill filter set and clear | Location filter independently (only provider filter tested). | Medium |
| 2 | Combined provider + location filter | Both filters applied simultaneously. | Medium |
| 3 | Filter persistence across page reloads | localStorage restoration after full page reload. | Medium |
| 4 | Patient search debounce behavior | Rapid typing doesn't cause excessive GQL requests (500ms debounce). | Low |

---

### 2.7 inbox-intake-manual-request-flow-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Manual intake for practice without intake activated | GetIntakeByZocdocButton shows instead of ManualIntakeRequestButton. | Medium |
| 2 | "Are you sure" modal when closing with unsaved data | AreYouSureModal in IntakeFlyout. | Low |

---

### 2.8 inbox-page-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Multi-Practice Login (MPL) dropdown | Practice switching, search, notification counts — **zero E2E coverage**. | **High** |
| 2 | Footer "View intake from past 60 days" link | `footer-link-to-intake-hub` rendering for intake-activated practices. | Medium |
| 3 | Inbox Experience Survey Modal | `InboxExperienceSurveyModal` gated by experiment flag. | Low |

---

### 2.9 inbox-routing-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Session expiration handler | `SessionExpirationHandler` gated by experiment flag in InboxPageWrapper. | Medium |

---

### 2.10 inbox-sidebar-filter-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Sidebar filter with combined pill filters | Sidebar counts remain correct when provider/location pill filters are active. | Medium |
| 2 | Sidebar count updates after appointment actions | Confirming/cancelling updates counts without page reload. | Medium |

---

### 2.11 inbox-sort-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Sort persistence or reset on page reload | Default sort behavior after reload. | Low |
| 2 | Sort with active patient search | Sort works correctly when patient name search is filtering results. | Low |

---

### 2.12 inbox-table-row-read-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Read state persistence across filter navigation | Read rows remain read after switching sidebar filters and returning. | Medium |

---

### 2.13 mobile-inbox-page-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Mobile sidebar/status filter buttons | MobileInboxHeaderFilters component — mobile status filter chips. | **High** |
| 2 | Mobile confirmation/cancellation flow | Confirming or cancelling from mobile card view. | **High** |
| 3 | Mobile manual intake request | Manual intake request button and flow on mobile viewport. | Medium |
| 4 | Mobile feedback link | InboxFeedbackLink visibility on mobile. | Low |

---

### 2.14 tutorial-coachmark-tests.ts

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 5 | Does not show edit appointment button coachmark during tutorial | Uses `ProviderInterface_ShouldUseNewModifyAppointmentModal=on` which is NOT in inbox experiment config. **Potentially irrelevant** if flag is fully rolled out. |

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Tutorial on mobile viewport | Tutorial does NOT show on mobile (`!isMobile` gating). | Medium |
| 2 | Tutorial metrics firing | V2 metrics for each tutorial step (view/click). | Low |

---

### 2.15 smoke/inbox-tests.ts

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Smoke test for intake-activated practice | Manual intake request flow in real environment. | Medium |
| 2 | Smoke test for mobile viewport | Full mobile inbox flow in real environment. | Medium |

---

## Part 3: High-Priority Gaps Summary

### Unit Tests — High Priority

| # | Component / File | Missing Coverage |
|---|-----------------|-----------------|
| 1 | BookingsTable | `getStatusString` function has zero tests |
| 2 | ErrorView | "Return to Inbox" button click behavior |
| 3 | InboxPageContainer | `markAsRead` flow when clicking unread rows |
| 4 | InboxPageManualIntakeRequestFlyoutCloseActions | "Are you sure" modal for dirty forms |
| 5 | InboxPageView | AppointmentCard rendering based on `shouldShowAppointmentCard` |
| 6 | MobileInboxView | Filters button opening MobileInboxFiltersModal |
| 7 | MPLDropdown | Practice name display + practice item click navigation |

### E2E Tests — High Priority

| # | Test File | Missing Coverage |
|---|----------|-----------------|
| 1 | inbox-page-tests | Multi-Practice Login (MPL) dropdown — zero E2E coverage |
| 2 | mobile-inbox-page-tests | Mobile sidebar/status filter buttons |
| 3 | mobile-inbox-page-tests | Mobile confirmation/cancellation flow |

### Skipped / Non-Existent Tests Needing Attention

| Item | Issue |
|------|-------|
| Reschedule test (card-part2 #4) | Permanently SKIPPED — references flag not in inbox config |
| Mapping tests #23-30 (cancellation) | Listed in mapping but **do not exist** in actual test files |
| Mapping test #23 (card-part1 attachments) | Listed in mapping but **does not exist** in actual test file |
| Mapping test #21 (provider notes CRUD) | Listed in mapping but **does not exist** in actual test file |

### Feature Flag Tests to Monitor

| Tests | Flag | Status |
|-------|------|--------|
| Tutorial coachmark #5 | `ProviderInterface_ShouldUseNewModifyAppointmentModal` | Not in inbox experiment config — may always be true |
| Various MPL tests | `shouldShowMPLDropdown` | Based on multi-practice user state, not a flag |
