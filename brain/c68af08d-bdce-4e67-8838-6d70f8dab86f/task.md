# Dashboard Widgets — Task Checklist

## Shared Infrastructure
- [x] Update `widgetRegistry.ts` — add 9 new widget definitions
- [x] Update `DashboardLayoutManager.tsx` — add imports + renderWidget cases for all 9

## Pure Frontend Widgets
- [x] `NowNextClass.tsx` — current/next class with progress bar
- [x] `SpotifyMini.tsx` — compact Spotify player wrapper
- [x] `QuickLinks.tsx` — localStorage bookmarks with edit mode
- [x] `SemesterCountdown.tsx` — exam date countdown per module
- [x] `TaskProgress.tsx` — in-progress tasks with progress bars

## Frontend Widgets (Multi-API)
- [x] `WeekAtAGlance.tsx` — 7-column mini calendar with density dots
- [x] `WorkloadHeatmap.tsx` — 4-week busyness grid

## Backend-Dependent Widgets
- [x] `SyncStatus.tsx` — uses existing `GET /sync/freshness` + `POST /sync/canvas`
- [x] `study_tip.py` — `GET /api/v1/dashboard/study-tip` backend endpoint
- [x] `AIStudyTip.tsx` — AI-generated tip with refresh

## Verification
- [x] `npm run build` — zero TypeScript errors
- [ ] Visual browser test — add each widget via picker
