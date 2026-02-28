# CraftCanvas – Comprehensive App Review & Improvement Roadmap

After a deep review of the entire codebase (frontend, backend, models, services, design system), here are recommended improvements grouped by category: **Architecture**, **UX/Features**, and **Quality**.

---

## 🏗️ Architecture & Code Quality

### 1. Componentize the Monolithic Page Files
Several pages are extremely large single files that would benefit from decomposition:

| Page | Lines | Recommendation |
|------|-------|----------------|
| `tasks/page.tsx` | 1,726 | Extract `InlineTaskCreator`, `TaskRow`, `TimelineView`, `DetailPanel` into `/components/tasks/` |
| `timetable/page.tsx` | 1,151 | Extract `WeekGrid`, `SlotBlock`, `ImportModal`, `EventCreator` |
| `focus/page.tsx` | 736 | Extract `MatrixRain`, `Starfield`, `PixelGrid`, `Synthwave`, clock renderers |
| `assignments/page.tsx` | 702 | Extract `AssignmentRow`, `DetailPanel`, `FilterBar` |

**Why**: Smaller components are easier to test, reuse, and maintain. The task page alone is nearly 2K lines — that's a maintenance risk.

### 2. Deduplicate Sort Logic on Dashboard
The task sorting logic (criticality → in_progress → due_date → priority) is duplicated verbatim in `page.tsx` lines 166–181 and again in `refreshTasks()` at lines 265–283. There's also a `taskSort.ts` utility already, but the dashboard doesn't use it.

**Fix**: Consolidate all sorting into `lib/taskSort.ts` and import everywhere.

### 3. Type Inconsistencies
- `TimetableSlot.id` is `number` in the dashboard but `string | number` in the timetable page
- `Assignment` type is defined separately in `page.tsx` (dashboard), `assignments/page.tsx`, and `announcements/page.tsx` — each with different fields
- No shared type definitions between pages

**Fix**: Create a `lib/types.ts` with canonical types shared across all pages.

### 4. Backend Timestamp Inconsistency
- `Task` model uses `Asia/Singapore` timezone via `_now_sg()`
- `Assignment` model uses `datetime.utcnow` (UTC)
- These are in the same DB, which could cause confusing comparisons

**Fix**: Standardize on Singapore timezone everywhere (per your GEMINI.md rule).

---

## ✨ UX Improvements & New Features

### 5. ⭐ Exam Countdown & Exam Mode
**High Impact** — Students live and die by exam schedules.

- Add an `exams` table (or flag on assignments) for exam dates
- Dashboard widget showing countdown to next exam
- "Exam Mode" in Focus page: auto-suggests revision tasks, disables distractions
- Timetable shows exam slots distinctly from regular classes

### 6. ⭐ Weekly Review / Retrospective Page
The spec mentions weekly reviews, but there's no `/review` page.

- Auto-generated summary: tasks completed, focus hours logged, assignments submitted
- Comparison vs. previous week (trend arrows)
- AI-generated reflection: "You spent 4.2 hours on CS2103T this week. Your midterm is in 9 days — consider allocating more focus time."
- Streak tracking (consecutive days with focus sessions)

### 7. ⭐ Study Analytics Dashboard
Currently, focus time is tracked (`actual_minutes` on tasks) but never visualized.

- Weekly/monthly charts: focus hours per module, tasks completed, assignments on-time rate
- Heatmap (like GitHub's contribution graph) showing study activity
- "Busiest day" and "most productive time" insights
- Integration with grades: correlate study time with grade outcomes

### 8. Smart Notifications / Alert System
There's a `notification_service.py` backend but no frontend notification center.

- Toast notifications for approaching deadlines (2h, 24h, 48h before due)
- Browser notification support for urgent items
- Notification center in sidebar (bell icon with unread badge)
- Customizable notification preferences per category

### 9. Assignment Submission Tracking Improvements
- Add a **progress bar** per assignment (not just status chips)
- Show **peer submission rate** from Canvas if available (motivational: "23 of 30 classmates submitted")
- **Submission reminders**: auto-create a task X hours before assignment due if status is still `not_started`

### 10. Quick Actions / Shortcuts
The `CommandPalette.tsx` exists but could be more powerful:

- **Global keyboard shortcuts**: `G+T` → Tasks, `G+D` → Dashboard, `G+C` → Courses
- **Quick task creation from anywhere** with `Cmd+K` → type → enter
- **Quick search across all entities**: tasks, assignments, notes, announcements
- Pin the search bar to always be visible (not just in command palette)

### 11. Module Detail Page Improvements
Currently `/courses/[id]` exists with tabs. Potential enhancements:

- **Study Plan tab**: AI-generated weekly study plan for each module
- **Resources tab**: curated links, supplementary materials (user-added)
- **Performance timeline**: grade trend over the semester
- **Discussion summary**: AI-summarized recent Canvas discussions

### 12. Notes with Rich Markdown Support
The notes page is plain text. Users would benefit from:

- **Real Markdown editor** with toolbar (bold, italic, headings, code blocks, LaTeX)
- **Linking between notes** (wiki-style `[[note title]]` linking)
- **AI-assisted note taking**: "Summarize today's CS2103T lecture" → auto-populates note
- **Export to PDF** functionality

### 13. Meeting Finder Enhancements
The meeting/activity finder is functional but basic:

- **Calendar integration**: overlay meeting poll results on timetable view
- **Recurring meetings**: support for weekly standing meetings
- **Auto-detect conflicts** with timetable when creating meetings
- **Poll expiry** with auto-selection of best time

### 14. Mobile Responsiveness
The README explicitly says desktop-first, but basic mobile usability would help for quick checks:

- **Dashboard**: Stack columns on mobile, collapsible sections
- **Tasks**: Swipe-to-complete gestures
- **Brief**: Full-screen card view optimized for phone

---

## 🔧 Technical Debt & Robustness

### 15. Error Handling & Loading States
- Many pages have bare `catch {}` blocks (e.g., `refreshTasks` line 285)
- No retry logic for failed API calls
- No skeleton loading states — just plain "Loading..." text

**Fix**: Add shimmer/skeleton loaders, proper error boundaries, and toast notifications for failures.

### 16. Offline Support / Service Worker
Given this runs on a home server accessed via Cloudflare Tunnel:
- Cache the last-fetched dashboard data so it's visible even if the server is momentarily unreachable
- Show "offline" banner when backend is down
- Queue task creates/updates when offline, sync when reconnected

### 17. Data Export / Backup
No way for the user to export their data:
- **Export** all tasks, notes, grades as JSON/CSV
- **Automated DB backup** to a configured path (e.g., NAS)
- **Import** from other task managers (Todoist, Notion)

### 18. Better Test Coverage
- E2E tests exist but have been flaky (multiple hung Playwright processes visible)
- No unit tests for backend services (canvas_sync, brief_generator, ai_service)
- No API integration tests

**Fix**: Add pytest tests for critical services, fix hanging E2E test processes.

### 19. Performance
- Dashboard fetches 6 API calls in parallel on load — consider a single `/api/v1/dashboard` endpoint that returns all data in one request
- The timetable page re-renders frequently due to CSS transitions on all elements (`* { transition }` in globals.css)
- Large JS bundles from page-level components (1700-line task page)

---

## 📋 Priority Ranking

| Priority | Feature | Impact | Effort |
|----------|---------|--------|--------|
| 🔴 P0 | Shared types & sort dedup | Prevents bugs | Small |
| 🔴 P0 | Skeleton loaders & error toasts | UX polish | Small |
| 🟠 P1 | Study Analytics Dashboard | High user value | Medium |
| 🟠 P1 | Weekly Review page | Retention & motivation | Medium |
| 🟠 P1 | Component decomposition | Maintainability | Medium |
| 🟡 P2 | Exam countdown & mode | Seasonal but critical | Medium |
| 🟡 P2 | Notification center | Proactive UX | Medium |
| 🟡 P2 | Rich Markdown notes editor | Note-taking quality | Medium |
| 🟢 P3 | Data export/backup | Safety net | Small |
| 🟢 P3 | Dashboard API consolidation | Performance | Small |
| 🟢 P3 | Mobile responsiveness | Convenience | Large |
| 🟢 P3 | Offline support | Resilience | Large |

---

## 💡 Quick Wins (< 1 hour each)

1. **Consolidate types** into `lib/types.ts`
2. **Use `taskSort.ts`** on dashboard instead of inline sort
3. **Fix timezone inconsistency** in Assignment model (`utcnow` → SG timezone)
4. **Add skeleton loaders** to dashboard widgets
5. **Kill hung Playwright processes** and add timeouts to test config
6. **Add favicon** and proper `<title>` per page for better tab identification
