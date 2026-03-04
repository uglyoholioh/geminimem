# Implementation Plan — 9 New Dashboard Widgets

Add 9 new widgets to the customizable dashboard widget system: Now/Next Class, Spotify Mini, Quick Links, Semester Countdown, Task Progress, Week-at-a-Glance, Canvas Sync Status, AI Study Tip, and Workload Heatmap.

---

## Proposed Changes

### Widget Registry & Layout Manager (Shared)

#### [MODIFY] [widgetRegistry.ts](file:///Users/oli/Desktop/CraftCanvas/frontend/lib/widgetRegistry.ts)

Add 9 new entries to `WIDGET_REGISTRY` array. Each entry needs `id`, `label`, `icon` (from lucide-react), `description`, and `defaultSize`. New IDs:
- `now_next_class`, `spotify_mini`, `quick_links`, `semester_countdown`, `task_progress`, `week_glance`, `sync_status`, `ai_study_tip`, `workload_heatmap`

#### [MODIFY] [DashboardLayoutManager.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/DashboardLayoutManager.tsx)

- Import all 9 new widget components
- Add 9 new `case` entries in the `renderWidget` switch statement

---

### Widget 1: Now / Next Class

#### [NEW] [NowNextClass.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/NowNextClass.tsx)

**What it shows:**
- Single focused card: current class (with progress bar + time remaining) OR next upcoming class (with countdown + venue)
- "No more classes today" state with ✨ emoji

**Data source:** `GET /timetable/today` — fetches today's slots, then uses `Date.now()` to find current/next

**Key logic:**
- Parse each slot's `start_time` / `end_time` into today's Date objects
- Compare against `now`: if `now` is between start and end → "In class now"
- Otherwise find first slot where `start > now` → "Next class"
- Auto-refreshes every 60s via `setInterval`
- Progress bar width = `(now - start) / (end - start) * 100`

---

### Widget 3: Spotify Mini Player

#### [NEW] [SpotifyMini.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/SpotifyMini.tsx)

**What it shows:** Compact player — spinning album art, track name (marquee if long), artist, play/pause/skip controls

**Data source:** Reuses existing `SpotifyPlayer.tsx` component with `isLargeMode={false}` — but wrapped in a simpler dashboard-sized container. Alternatively, extract core Spotify hook logic and render a minimal UI.

**Approach:** Import `SpotifyPlayer` directly and render it. The component already handles all states: loading, not connected (with "Link Account" CTA), connected but no track, and active playback. It's compact enough as-is with `isLargeMode={false}`.

---

### Widget 4: Quick Links

#### [NEW] [QuickLinks.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/QuickLinks.tsx)

**What it shows:** 2×3 grid of customizable bookmark tiles (icon + label + URL)

**Data source:** `localStorage` key `craftcanvas_quick_links` — no backend needed

**Default links:**
```ts
[
  { label: 'Canvas', url: 'https://canvas.nus.edu.sg', emoji: '🎓' },
  { label: 'NUSMods', url: 'https://nusmods.com', emoji: '📅' },
  { label: 'Library', url: 'https://nus.edu.sg/nuslibraries', emoji: '📚' },
  { label: 'Webmail', url: 'https://outlook.office.com', emoji: '📧' },
  { label: 'Gradescope', url: 'https://www.gradescope.com', emoji: '✏️' },
  { label: 'LumiNUS', url: 'https://luminus.nus.edu.sg', emoji: '💡' },
]
```

**Edit mode:** Small pencil icon toggles edit mode — shows input fields for label/URL per tile, add/remove buttons. Max 8 links.

---

### Widget 6: Semester Countdown

#### [NEW] [SemesterCountdown.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/SemesterCountdown.tsx)

**What it shows:** List of courses with exam dates, days remaining countdown, urgency coloring

**Data source:** `GET /courses?active_only=true` — the `Course` type already has `exam_date` and `exam_duration` fields

**Key logic:**
- Filter courses where `exam_date` is non-null and in the future
- Sort by soonest first
- Show: module code badge (colored), "Exam in X days", exam date
- Color: red < 7 days, amber < 14 days, green otherwise
- Empty state: "No exam dates set — add them in module settings"

---

### Widget 7: Task Progress

#### [NEW] [TaskProgress.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/TaskProgress.tsx)

**What it shows:** Top 3–5 in-progress tasks with progress bars

**Data source:** `GET /tasks?status=in_progress` — the `Task` type has `percentage_done` (number) and `subtasks` (JSON string)

**Key logic:**
- Fetch tasks, filter to `status === 'in_progress'`, take top 5
- Each row: course colour dot, task title (truncated), progress bar (`percentage_done`%)
- If `subtasks` is non-null, parse JSON and show "X/Y subtasks" below the progress bar
- Click navigates to `/tasks`

---

### Widget 8: Week-at-a-Glance

#### [NEW] [WeekAtAGlance.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/WeekAtAGlance.tsx)

**What it shows:** Compact 7-column mini calendar (Mon–Sun) with density dots per day

**Data source:** Multiple existing endpoints:
- `GET /timetable/week` — classes per day
- `GET /tasks` — filter tasks with due dates this week
- `GET /assignments` — filter assignments due this week

**Key logic:**
- Compute current week range (Mon–Sun)
- For each day, count: classes, tasks due, assignments due
- Render a 7-column grid, each day labeled (M T W T F S S)
- Today column highlighted with accent border
- Colored dots below each day label: 🔵 class count, 🟢 task count, 🔴 assignment count
- Compact — no text, just visual density

---

### Widget 10: Canvas Sync Status

#### [NEW] [SyncStatus.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/SyncStatus.tsx)

**What it shows:** Last sync time, freshness status indicator, "Sync Now" button

**Data source:** Existing endpoints:
- `GET /sync/freshness` — returns `{ canvas: { last_synced_at, age_seconds, status } }` where status is `fresh|stale|expired|none`
- `POST /sync/canvas` — triggers sync, returns stats

**Key logic:**
- On mount, fetch freshness
- Display: status dot (green=fresh, amber=stale, red=expired/none), "Last synced X min ago", items synced count
- "Sync Now" button triggers `POST /sync/canvas`, shows loading spinner, then refreshes freshness
- Auto-refresh freshness every 60s

---

### Widget 11: AI Study Tip

#### [NEW] [study_tip.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/study_tip.py)

New lightweight backend endpoint: `GET /api/v1/dashboard/study-tip`

**Logic:**
- Gather: count of pending assignments (+ nearest due date), count of open tasks, today's class count
- Build a short prompt: "Given this student has X assignments due (nearest: Y), Z open tasks, and N classes today, give ONE short, specific, actionable study tip in 1-2 sentences. Be concise."
- Call `ai_service.generate(...)` (same service used by briefs)
- Cache the tip in `settings` table with key `study_tip_cache` + `study_tip_date` — only regenerate if date changed or user requests refresh
- Return `{ tip: string, generated_at: string }`

#### [MODIFY] [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py)

Register the new router: `app.include_router(study_tip_router, prefix="/api/v1/dashboard")`

#### [NEW] [AIStudyTip.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/AIStudyTip.tsx)

**What it shows:** AI-generated study tip card with regenerate button

**Data source:** `GET /dashboard/study-tip`

**Key logic:**
- Fetch tip on mount
- Show: lightbulb icon, tip text, "generated X min ago" timestamp
- "New Tip" button re-fetches with `?refresh=true` query param
- Loading state with skeleton text animation
- Empty/error state gracefully handled

---

### Widget 12: Workload Heatmap

#### [NEW] [WorkloadHeatmap.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/WorkloadHeatmap.tsx)

**What it shows:** GitHub-style 4-week grid (past 2 weeks + next 2 weeks) colored by daily busyness

**Data source:** Client-side aggregation from:
- `GET /timetable/week?date=YYYY-MM-DD` for each of the 4 weeks (4 API calls)
- `GET /tasks` — client-side filter by due_date
- `GET /assignments` — client-side filter by due_at

**Key logic:**
- Generate 28 day cells (4 rows of 7 columns — Mon–Sun)
- For each cell: count classes + tasks due + assignments due = "busyness score"
- Color intensity: 0 items → blank, 1–2 → light accent, 3–4 → medium, 5+ → strong
- Today cell has a border highlight
- Row labels: "2 weeks ago", "Last week", "This week", "Next week"
- Column headers: M T W T F S S
- Stays compact: each cell is a small ~18×18px square

> [!NOTE]
> To avoid 4 separate timetable API calls, we can fetch all timetable slots once (they're keyed by `day_of_week` not specific dates) and map them to each day. Tasks and assignments are already fetched by the dashboard.

---

## Verification Plan

### Build Check
```bash
cd /Users/oli/Desktop/CraftCanvas/frontend && npm run build
```
Ensures no TypeScript errors across all 9 new widget files + registry changes.

### Browser Testing

1. Navigate to `http://localhost:3000` (dashboard)
2. Click "Customise" button in the dashboard header
3. Click "+ Add New Widget" button
4. Verify all 9 new widgets appear in the widget picker
5. Add each widget one by one and verify:
   - **Now/Next Class**: shows current/next class or "no classes" state
   - **Spotify Mini**: shows connect CTA or player controls
   - **Quick Links**: shows default bookmarks grid, links open in new tab
   - **Semester Countdown**: shows exam countdowns or "no exams set" empty state
   - **Task Progress**: shows in-progress tasks with progress bars or empty state
   - **Week-at-a-Glance**: shows 7-column mini calendar with density dots
   - **Sync Status**: shows last sync time + sync button works
   - **AI Study Tip**: shows generated tip (if AI is running) or graceful error
   - **Workload Heatmap**: shows 4-week grid with today highlighted
6. Test widget removal (X button in edit mode)
7. Test drag-and-drop reordering
8. Refresh page — verify layout persists

### Backend Test (AI Study Tip only)
```bash
cd /Users/oli/Desktop/CraftCanvas/backend && python -m pytest tests/ -x -q
```
Ensure existing tests still pass after adding the new router.
