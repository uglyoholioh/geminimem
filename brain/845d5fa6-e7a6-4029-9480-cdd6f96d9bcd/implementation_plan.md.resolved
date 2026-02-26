# Dashboard Layout Redesign

Redesign the dashboard to show all critical information at a glance in fixed-size sections with no wasted space or scrolling.

## Current Problems

1. **Daily Brief + Chat takes 60% width and full height** — dominates the view; most of the time the chat is empty, wasting space
2. **Right column requires scrolling** — 5 stacked cards (Schedule, Tasks, Announcements, Assignments, Stats) overflow the viewport
3. **Empty sections waste space** — "No classes today" still takes a large card-sized block
4. **No content cutoffs** — list sections grow unbounded based on content
5. **Stats section is redundant** — duplicates counts already visible from the other cards

## Proposed Layout

A **3-row, 2-column grid** that fills the viewport with zero scroll. Every section has a **fixed height** determined by the grid, not its content. Lists are capped and overflow is hidden with a "View all →" link.

```
┌──────────────────────────────────────────────────────────────┐
│  Header: Greeting + Date + Quick Stats (inline)             │  ~48px
├────────────────────────────────┬─────────────────────────────┤
│                                │  Today's Schedule           │  ~40% vh
│  Daily Brief & Chat            │  (max 4 slots, scrollable)  │
│  (collapsible brief,           ├─────────────────────────────┤
│   chat interface)              │  Tasks                      │  ~30% vh
│                                │  (max 4 items)              │
├────────────────────────────────┼─────────────────────────────┤
│  Announcements (max 3)         │  Upcoming Assignments       │  ~30% vh
│                                │  (max 4 items)              │
└────────────────────────────────┴─────────────────────────────┘
```

### Key Design Decisions

| Decision | Rationale |
|---|---|
| **Quick stats move into header** | Eliminates a full card; stats are glanceable from the top bar |
| **Brief + Chat spans 2 rows** | Keeps the AI assistant prominent but shares width equally now (50/50) |
| **Announcements get a bottom-left card** | Moves them out of the sidebar scroll; visible at a glance |
| **Fixed grid rows via `grid-template-rows`** | Every section has an exact height — no content-driven expansion |
| **Max items per list** | Schedule: 4 slots, Tasks: 4, Announcements: 3, Assignments: 4 — with "View all →" |
| **Remove standalone Stats card** | Stats are inlined into the header as small pill badges |

## Proposed Changes

### Dashboard Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)

**Header area:**
- Merge Quick Stats inline into the header row as small badge pills (Open Tasks, Classes Today, Due Soon, Unread)
- Remove the standalone "This Week" stats card entirely

**Grid layout:**
- Switch from `grid-cols-5` (60/40 split) to a `grid-cols-2 grid-rows-[1fr_auto]` layout
- Row 1: Daily Brief+Chat (left, `row-span-2`) | Schedule (right top) + Tasks (right bottom) — using nested grid on right
- Row 2: Announcements (left) | Assignments (right) — only if Brief doesn't span both rows, otherwise a 3-row approach
- Use explicit `h-[calc(100vh-120px)]` on the grid container and distribute row heights via CSS grid fractions
- All sections get `overflow-hidden` with internal scroll only where needed (chat messages area)

**Section changes:**
- **Today's Schedule**: Cap at 4 slots, add `overflow-hidden`, show "View all →" link. Fixed height via grid row.
- **Tasks**: Cap at 4 items. Fixed height. Show "View all →" link.
- **Announcements**: Cap at 3 items. Fixed height. Show "View all →" link to a future announcements page or keep as-is.
- **Assignments**: Cap at 4 items. Fixed height. Show "View all →" link.
- **Daily Brief + Chat**: Keep collapsible brief + chat but constrain to its grid cell. Internal scrolling only.

**Empty states:**
- Compact single-line empty states instead of large centered blocks (e.g., a subtle italic line "No classes today" rather than a padded centered paragraph)

## Verification Plan

### Browser Testing
1. Open `http://localhost:3000/` in the browser
2. Verify the dashboard fits entirely within the viewport — **no page-level scrollbar**
3. Verify each section has a consistent fixed height regardless of content (check with both empty and populated state)
4. Verify list sections cap their items (Schedule ≤ 4, Tasks ≤ 4, Announcements ≤ 3, Assignments ≤ 4)
5. Verify "View all →" links are present on capped sections
6. Verify the chat interface still works (typing + sending a message)
7. Verify the brief regenerate button still works
8. Verify stat badges in the header display correct counts
