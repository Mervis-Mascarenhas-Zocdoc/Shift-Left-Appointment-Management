# Provider Calendar — Test Gap Coverage Report

**Date:** 2026-05-04
**Scope:** Unit tests (97) + Cypress E2E tests (114) for `webapps/provider-calendar`
**Source:** `frontend-monorepo/webapps/provider-calendar/src/pages/Calendar/`

---

## Executive Summary

| Metric | Unit Tests | E2E Tests |
|--------|-----------|-----------|
| Total tests analyzed | 97 | 114 |
| Relevant | **97 (100%)** | **~110 (96%)** |
| Irrelevant / Stale | 0 | 3 skipped (SQUAWK-6014, SQUAWK-6015, SQUAWK-6178), feature-flag-gated tests now active path |
| High-priority missing gaps | 6 | 1 |
| Medium-priority missing gaps | 18 | 10 |

> **2026-05-04 update:** 24 new E2E tests across 13 new sections were added covering mobile appointment-card, mobile availability/busy blocks, DST week-start (SQUAWK-6090) and DST availability (SQUAWK-5477, SQUAWK-6178), global-nav (provgro_global_nav), location approval filtering (SQUAWK-5997), patient record redirect, collect-intake refactor, and smoke/calendar-mobile. Several previously-flagged high-priority E2E gaps are now closed — see the resolved-gap notes inline below.

---

## Part 1: Unit Test Analysis

### 1.1 AllDayHolidayRow-tests.tsx (10 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | All-day exclusion (Out of Office) rendering | AllDayRow renders "Out of office" text when an all-day exclusion is present. Core feature (line 164 of AllDayRow.tsx) is untested. | **High** |
| 2 | Hover interaction on cells | Hovering over an all-day cell displays "Out of office" and applies elevation styles via `onHoverChange` callbacks. | Medium |
| 3 | Click interaction on all-day cell | `handleClick` from `useHandleAllDayRowClick` is wired up but never tested. | Medium |
| 4 | Compact vs Default zoom level appearance | AllDayRow renders different heights (32px vs 48px) based on `calendarZoomLevel`. | Low |

---

### 1.2 AppointmentCard-tests.tsx (4 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | AppointmentCard receives correct `applicationTimezone` | The timezone prop is passed from CalendarPageView but never verified at the card. | Medium |
| 2 | Click on modal/popover does NOT close appointment card | OutsideClickWrapper has specific selectors to exclude modals/popovers — exclusion logic is untested. | Medium |
| 3 | `onUpdateAppointment` callback triggers refetch | useUpdateAppointment hook integration with calendar data refetch is untested. | Medium |
| 4 | AppointmentCard does not render when no appointment selected | Initial null state not directly tested. | Low |

---

### 1.3 BlockTypeFilters-tests.tsx (3 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Shows and hides appointment blocks | Toggling appointments checkbox hides/shows appointment blocks. Only availability toggle is tested today. | **High** |
| 2 | Exclusion blocks visibility filter | Exclusion blocks (busy/out-of-office) visibility based on filter state is untested. | Medium |
| 3 | Metrics fired on block type select/deselect | v2 metrics on selectBlockType/deselectBlockType are untested. | Low |

---

### 1.4 CalendarPage-tests.tsx (11 tests)

**Irrelevant Tests:** None — all tests are relevant.

> **Note:** MPL dropdown tests rely on `shouldShowMPLDropdown` feature flag (default OFF). If fully rolled out, the "does not render when flag disabled" test would be testing dead logic.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Welcome tutorial modal on first visit | `useWelcomeTutorial` shows modal for first-time users when `TUTORIAL_SEEN_KEY` is absent from localStorage. | **High** |
| 2 | `setTimeframe` called when URL has provider-id param | CalendarPageContainer calls `dateControls.setTimeframe('week')` when `urlParams.providerId` is present. | Medium |
| 3 | CMS modal integration | `useCmsModal` is called in CalendarPageView gated by tutorial state. | Medium |
| 4 | `shouldSupportMultiDayBlocks` feature flag effect | Flag is read and passed to CalendarGridContainer but behavior is untested. | Medium |
| 5 | Calendar grid view metric fires only once | `hasLoggedMetric` ref ensures one-time metric fire — not verified. | Low |

---

### 1.5 DateHeaderNav-tests.tsx (12 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Day/Week toggle fires metrics | CalendarHeader sends metric with v2TargetName "Day" or "Week" on toggle. | Medium |
| 2 | Today button fires metric | "Today" button is wrapped in MetricsClickable. | Low |
| 3 | Arrow buttons fire metrics | Back/forward arrows have MetricsClickable wrappers. | Low |

---

### 1.6 FullPageCalendarGrid-tests.tsx (2 tests)

**Irrelevant Tests:** None — both tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Restricted time range expands for early morning appointments | If an appointment exists at 5am, grid should start from 5am. | **High** |
| 2 | Restricted time range expands for late evening appointments | If an appointment exists at 10pm, grid should show slots until 10pm. | **High** |
| 3 | Week view restricted time range | Both existing tests are day view only. | Medium |
| 4 | Grid shows time labels correctly | TimeLabels component rendering is untested. | Low |

---

### 1.7 HomePage-tests.tsx (7 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | SessionExpirationHandler renders on non-preview hosts | Conditional rendering based on host is untested. | Medium |
| 2 | V2 page view event is sent on mount | `sendV2PageViewEvent` called in useEffect. | Medium |
| 3 | Mobile view flag gates `showMobilePage` prop | `shouldEnableMobileView` combined with `useIsSmallBreakpoint`. | Medium |

---

### 1.8 PageView-tests.tsx (4 tests)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 3 | "renders CalendarPageContainer when SHOULD_ENABLE_MOBILE_VIEW is on with a mobile viewport" | **Misleading name** — test mocks `useIsSmallBreakpoint` to return `false` (desktop), not mobile. Test is technically relevant but name is wrong. |

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Mobile view renders when flag ON + small viewport | No test mocks `useIsSmallBreakpoint` to return `true`. The actual mobile path is never tested. | **High** |
| 2 | PatientResultPageContainer renders on "See All" | `shouldRenderCalendarPage=false` path not directly tested. | Medium |
| 3 | "Back to Calendar" resets patient search input | `onGoBackClicked` sets `patientNameSearchInput` to "". | Low |

---

### 1.9 PatientResultsPage-tests.tsx (5 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Empty state when no patients found | "0 patient matches" display. | Medium |
| 2 | Loading state | Component behavior when `useGetSearchPatients` returns `loading: true`. | Medium |
| 3 | Error state | Component behavior on API error. | Medium |
| 4 | Phone number formatting | `formatPhoneNumber` output format verification. | Low |
| 5 | Sex display for non-MALE/FEMALE | Empty string fallback when sex is neither MALE nor FEMALE. | Low |

---

### 1.10 ProviderHeader-tests.tsx (24 tests)

**Irrelevant Tests:** None — all 24 tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Provider profile link renders in day view for < 5 providers | Conditional profile link rendering (lines 137-159 of ProviderHeader.tsx). | Medium |
| 2 | Provider name click in week view fires metric | `onClickProviderCellWrapper` sends metric. | Low |
| 3 | Provider header cell height changes with zoom level | Compact = 48px, Default = 64px. | Low |

---

### 1.11 ProviderLocationFilters-tests.tsx (10 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Timezone selector interaction | CalendarSettingsView has a timezone dropdown that fires metrics on open/select — untested in any file. | **High** |
| 2 | Calendar date picker interaction | Sidebar mini Calendar date picker that calls `selectDate`. | Medium |
| 3 | "See all providers" button for > 10 providers | `shouldShowSeeAllProviders` flag and button rendering. | Medium |
| 4 | "See all locations" button for > 10 locations | Same for locations. | Medium |
| 5 | Prevent deselecting ALL providers | Minimum selection enforcement behavior. | Medium |
| 6 | Resources section links (tutorial, help, feedback) | Sidebar links are untested. | Low |

---

### 1.12 ZoomLevel-tests.tsx (5 tests)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Zoom level change fires metric | `handleZoomLevelChange` sends metric with v2TargetName "Default" or "Compact". | Medium |
| 2 | All-day row height changes with zoom level | AllDayCell renders different heights (32px vs 48px). | Low |

---

## Part 2: Cypress E2E Test Analysis

### 2.1 appointment-card-tests.ts (Tests #1-7)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 5 | Opens and reschedules in new modal | Behind `ProviderInterface_ShouldUseNewModifyAppointmentModal` (OFF by default). **Keep if flag is in active rollout; remove if abandoned.** |
| 7 | Shows error when typing 2-digit year in modify modal | Same flag dependency as #5. |

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | "Mark as No Show" appointment action | `shouldDisableMarkAsNoShowForZvsAppt` experiment exists; no E2E test covers this. | Medium |
| 2 | Virtual appointment icon on block | AppointmentBlock.tsx renders `IconVideoOn` for virtual appointments. | Low |

---

### 2.2 availability-block-tests.ts (Tests #8-33)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Add availability with "Virtual" appointment type | Tests only cover "In person"; no test selects virtual visit type. | Medium |
| 2 | Edit overlapping availability — select a one-off slot | Test #33 navigates to edit modal but does not actually edit. | Low |
| 3 | Add availability with multi-location dropdown selection | Test #14 checks dropdown content but does not test selecting + saving. | Low |

---

### 2.3 busy-block-tests.ts (Tests #34-45)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Create OOO from desktop all-day row | AllDayRow.tsx supports desktop OOO creation. Only mobile is tested (test #79). | **High** |
| 2 | Edit OOO — change end date (desktop) | Test #45 opens edit modal but does not change end date. | Medium |
| 3 | Delete OOO from desktop | No desktop E2E test covers OOO deletion. | Medium |
| 4 | "Mark as out of office" link from Edit Busy Block modal | EditBusyBlockModal has this link; untested. | Low |

---

### 2.4 calendar-filter-tests.ts (Tests #46-55)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Sidebar toggle (collapse/expand) | CalendarPageView includes `useToggleSidebar` with toggle button. | Medium |
| 2 | Filter interaction with week view | All filter tests run in day view only. | Low |

---

### 2.5 calendar-grid-tests.ts (Test #56)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Dynamic grid time range expansion | `useRestrictedCalendarTimeRange` expands for blocks outside 7AM-8PM. | Medium |
| 2 | Calendar zoom level control | Zoom level change + grid rendering verification. | Medium |
| 3 | Provider pagination (7+ providers in day view) | Pagination arrows from `useProviderPagination`. | Medium |

---

### 2.6 calendar-redirect-tests.ts (Tests #57-58)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | No providers/locations error page | PageView renders `NoProviderLocationsErrorPage`. | Low |

---

### 2.7 calendar-timezone-tests.ts (Test #59)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Timezone persistence across page reloads | Selected timezone preserved after reload. | Low |

---

### 2.8 calendar-url-parameters-tests.ts (Tests #60-68)

**Irrelevant Tests:** None — all tests are relevant.

**Missing Tests:** No critical gaps identified.

---

### 2.9 cross-day-appointment-block-tests.ts (Test #69)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Cross-day appointment in day view | Test #69 only covers week view. | Low |

---

### 2.10 cross-day-availability-block-tests.ts (Tests #70-71)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 71 | Timezone-based creation and editing workflow | **Permanently SKIPPED** (SQUAWK-6015 flakiness). Provides no coverage. |

**Missing Tests:** No gaps beyond what Test #71 would cover if unskipped.

---

### 2.11 cross-day-busy-block-tests.ts (Tests #72-73)

**Irrelevant Tests:** None.

**Missing Tests:** No critical gaps.

---

### 2.12 date-navigation-tests.ts (Tests #74-77)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Date picker widget in sidebar | CalendarSettingsView has a Calendar date picker; no E2E test covers clicking a specific date. | Low |

---

### 2.13 mobile/mobile-calendar-page-tests.ts (Tests #78-79)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority | Status |
|---|----------------|-------------------|----------|--------|
| 1 | Mobile appointment card interaction | Mobile renders appointment blocks but no tap test exists. | **High** | ✅ Resolved by `mobile/appointment-card-tests.ts` (#103) |
| 2 | Mobile busy block interaction | No mobile test for clicking/creating a busy block. | Medium | ✅ Resolved by `mobile/mobile-busy-block-tests.ts` (#111-112) |
| 3 | Mobile availability block interaction | No mobile test for clicking an availability block. | Medium | ✅ Resolved by `mobile/mobile-availability-block-tests.ts` (#106-110) |

---

### 2.14 mobile/provider-filter-modal-tests.ts (Test #80)

**Irrelevant Tests:** None.

**Missing Tests:**

| # | Suggested Test | What It Would Test | Priority |
|---|----------------|-------------------|----------|
| 1 | Mobile provider filter — select and apply | Test #80 covers expand/collapse/search but does not test selecting a provider and verifying calendar updates. | Medium |

---

### 2.15 patient-search-tests.ts (Tests #81-83)

**Irrelevant Tests:** None.

**Missing Tests:** No critical gaps.

---

### 2.16 smoke/calendar-tests.ts (Tests #84-86)

**Irrelevant Tests:** None.

**Missing Tests:** No critical gaps (smoke tests are intentionally broad).

---

### 2.17 welcome-tutorial-tests.ts (Tests #87-90)

**Irrelevant Tests:**

| Test # | Test Name | Issue |
|--------|-----------|-------|
| 87 | First time users can progress through tutorial steps | **Permanently SKIPPED** (SQUAWK-6014 flakiness). No coverage. |

**Missing Tests:** No gaps beyond what Test #87 would cover if unskipped.

---

## Part 3: High-Priority Gaps Summary

### Unit Tests — High Priority

| # | Component / File | Missing Coverage |
|---|-----------------|-----------------|
| 1 | AllDayHolidayRow | Out of Office exclusion rendering |
| 2 | BlockTypeFilters | Appointment block visibility toggle |
| 3 | CalendarPage | Welcome tutorial modal on first visit |
| 4 | FullPageCalendarGrid | Dynamic time range expansion (early morning / late evening) |
| 5 | PageView | Mobile view path (`showMobilePage=true`) |
| 6 | ProviderLocationFilters | Timezone selector interaction |

### E2E Tests — High Priority

| # | Test File | Missing Coverage |
|---|----------|-----------------|
| 1 | busy-block-tests | Desktop OOO create/edit/delete flows (only mobile tested) |
| ~~2~~ | ~~mobile-calendar-page-tests~~ | ~~Mobile appointment card interaction~~ ✅ Resolved by `mobile/appointment-card-tests.ts` (#103) |

### Skipped Tests Needing Attention

| Test # | SQUAWK Ticket | What It Covers |
|--------|--------------|----------------|
| 71 | SQUAWK-6015 | Cross-day availability timezone workflow |
| 87 | SQUAWK-6014 | Welcome tutorial step progression |
| 94 | SQUAWK-6178 | DST availability creation (newly added; gated as skipped) |

### Feature Flag Tests to Monitor

| Tests | Flag | Default |
|-------|------|---------|
| #5, #7, #92-93 | `ProviderInterface_ShouldUseNewModifyAppointmentModal`, `Calendar_ShouldGetUnapprovedProviders` | varies |
| #69-73 | `ProviderCalendar_MultiDayBlocks` | OFF |
| #78-79, #103-116 | `ProviderCalendar_MobileView`, `provgro_global_nav`, `ProviderInterface_UseProviderLevelLocationApproval`, `ProviderInterface_PatientRecord_ShouldRedirectToNewVersion` | varies |

> If any of these flags are fully rolled out and removed, the corresponding tests need refactoring to test the default path.
