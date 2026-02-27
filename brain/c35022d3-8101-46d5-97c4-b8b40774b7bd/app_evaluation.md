# CraftCanvas — Critical UX & Architecture Evaluation

> **Evaluator perspective**: A user who's been using this daily for a semester, combined with a code reviewer doing a full audit.

---

## Executive Summary

CraftCanvas has impressive **breadth** — 10 sidebar links, AI chat, Canvas sync, focus timer, grades, notes, timetable, meetings. But it's suffering from a classic builder's trap: **the features are wide, but shallow and disconnected**. A user opening this for the first time sees a dashboard that screams "nothing is happening" and a assignments page that screams "everything is on fire." There's no middle ground.

---

## 🔴 Critical Issues (Fix These First)

### 1. The Dashboard Is Dead on Arrival

````carousel
![Dashboard showing mostly empty state with "No brief generated" and "Nothing scheduled today"](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/home_page_1772219689977.png)
<!-- slide -->
**What the user sees:**
- "No brief generated yet" — the centrepiece is blank
- "Nothing scheduled today" — the agenda is blank (it's recess week)
- Module Hub shows 2 cards with cryptic stats (24, 0, 17 — 24 what? 17 what?)
- The stats badges say **"15 due"** and **"10 unread"** — but none of this is visible anywhere on the page

**Root cause:** The dashboard is fundamentally designed around the **best case** (busy school day) and falls apart the other 60% of the time.
````

**Suggestions:**
- Show the **next 7 days** of upcoming deadlines/events by default, not just "today" 
- When no brief exists, show an auto-generated mini-summary instead of a placeholder
- The "15 due" badge in the header should link to a visible list ON the dashboard, not just be a number
- Module Hub stats need labels: "24 assignments", "0 tasks", "17 announcements"

### 2. Assignments Page Is a Wall of Red

![Assignments page showing 15+ overdue items all in red](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/assignments_page_1772219758786.png)

Every single item is **"overdue"** in red. This is because Canvas syncs every assignment regardless of whether it's been submitted on Canvas. The page is completely useless in its current state — it's panic-inducing and provides zero actionable information.

**Suggestions:**
- **Auto-mark as submitted**: Cross-reference with Canvas submission status on sync, don't just rely on manual marking
- **Default filter should be "Needs Attention"**, not "All" — hide submitted/graded items  
- **Group by week**, not flat chronological — "Week 3 Assignments" vs "16 Jan 2026"
- Date-only assignment titles like "16 Jan 2026" and "20 Jan 2026" are completely meaningless — pull actual assignment names from Canvas
- Show a **progress meter**: "3 of 8 due this week completed" — don't just show a flat list
- Items older than 2 weeks overdue should auto-collapse into an "Archive" section

### 3. The `page.tsx` Dashboard Is an 815-Line God Component

The entire dashboard — header, agenda, module hub, chat, brief, modals, all data fetching — lives in a single file. This is a maintainability red flag:
- Duplicated sort logic (lines 203-221 and 313-331 are near-identical)
- 17 useState hooks in one component
- Heavy `any` type casting throughout (`(item.data as any).venue`)
- The `refreshTasks` function is duplicated with the initial load sort  

**Suggestion:** Extract into at least 5 smaller components: `DashboardHeader`, `AgendaTimeline`, `ModuleHub`, `QuickStats`, and `ChatPanel`. Share the sort logic via a utility function.

---

## 🟡 Significant UX Problems

### 4. Navigation Has Too Many Items With No Hierarchy

10 sidebar icons plus settings and theme toggle — that's 12 clickable items in a narrow sidebar. For a student who uses this daily:
- **"Activity Finder"** (meetings) is rarely used but takes prime real estate
- **"History"** and **"Grades"** are marginal features buried in the same priority as Tasks
- **"Notes"** competes with the AI chat and brief for the same mental space

**Suggestion:** Group into tiers:
- **Top tier** (always visible): Dashboard, Tasks, Timetable, Assignments
- **Second tier** (collapsible): Courses, Notes, Focus
- **Archive/Utility** (bottom): Grades, History, Meetings, Settings

### 5. Timetable Page Is 90% Empty Space During Breaks

![Timetable showing Recess Week with nothing useful](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/timetable_page_1772219754121.png)

During recess/vacation weeks, the timetable is a nearly blank page with just "Enjoy your break!" That's nice but wastes the entire screen. This means for ~30% of the semester, this page is useless.

**Suggestions:**
- Show upcoming deadlines during break weeks
- Show a "Week at a glance" with tasks and assignments even without classes
- Let users add personal events/blocks to the timetable (gym, study groups, etc.)

### 6. Tasks Kanban Is Too Sparse

![Tasks kanban board with 1 task](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/tasks_page_1772219749443.png)

With only 1 task, the kanban board looks abandoned. Three giant dashed-border empty columns dominate the view. The "Drop tasks here" placeholder is poor — nobody is going to create tasks just to see the board work.

**Suggestions:**
- Default to **List view** when task count < 5, not Kanban
- Kanban cards should show more metadata: due date, module, priority badge
- Add **quick templates**: "Assignment Task", "Exam Prep", "Project Work" — pre-filled task types
- Canvas assignments should auto-generate companion tasks (this is mentioned in settings as `auto_tasks_from_assignments` but seems unused in the UI)

### 7. Focus Timer Feels Disconnected From Everything Else

![Focus page with Pomodoro timer](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/focus_page_1772219764012.png)

The focus timer looks cool (love the synthwave grid!) but:
- There's **no way to start a focus session from a task** — you have to navigate to `/focus`, then select the task
- Completed pomodoro sessions don't update `actual_minutes` on tasks automatically
- No daily/weekly focus stats visible on the dashboard
- The floating timer isn't prominent enough to be useful during browsing

**Suggestions:**
- Add a ▶️ "Focus" button on task cards and assignment rows
- Show daily focus time on the dashboard header (e.g. "2h 15m focused today")
- After finishing a pomodoro, auto-suggest the next task based on priority
- Track and display focus streaks

### 8. Settings Page Exposes Too Much Complexity

![Settings page showing profile section](/Users/oli/.gemini/antigravity/brain/c35022d3-8101-46d5-97c4-b8b40774b7bd/settings_page_1772219770368.png)

7 tabs of settings is a lot. The "AI Context" tab, Craft integration, and Telegram settings are power-user features exposed at the same level as basic profile settings. For a student, this is intimidating.

**Suggestions:**
- Merge Profile + Appearance into one "General" tab
- Hide Craft/Telegram/AI Context behind an "Advanced" toggle
- Show a guided setup wizard on first login instead of dumping everything in settings

---

## 🟢 Architecture & Code Suggestions

### 9. API Key Is Exposed Client-Side

```typescript
// lib/api.ts line 24
"X-API-Key": process.env.NEXT_PUBLIC_API_KEY || "dev-secret-key-change-in-production",
```

The `NEXT_PUBLIC_` prefix means this is bundled into the client JS. The design system explicitly says *"Never expose API key client-side"*, but the implementation does exactly that. The Next.js rewrite proxy should handle auth server-side.

### 10. Grades Page Uses LocalStorage — Not Synced

```typescript
// grades/page.tsx line 21
const GRADES_KEY = 'academic-os-grades'
```

Grades are stored in `localStorage` while every other feature uses the SQLite backend. This means:
- Grades are lost if you clear browser data
- Grades don't sync across devices
- No AI can use grade data for context (e.g. "What do I need on the final to get a B+?")

**Suggestion:** Move to a proper backend endpoint. The `grade` model already exists in `backend/models/grade.py`.

### 11. Massive File Sizes

| File | Lines | Concern |
|------|------:|---------|
| [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx) (Dashboard) | 815 | God component |
| [tasks/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/tasks/page.tsx) | 1,687 | Needs splitting |
| [timetable/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/timetable/page.tsx) | 1,032 | Complex but manageable |
| [settings/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/settings/page.tsx) | 762 | Each tab should be a component |
| [focus/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/focus/page.tsx) | 736 | Animation code bloats this |

**Suggestion:** Adopt a strict "300 lines per file" guideline. Extract subcomponents aggressively.

### 12. Design System Spec vs Reality

The `DESIGN_SYSTEM.md` describes a "dark, terminal-meets-editorial" aesthetic with:
- DM Mono, Instrument Serif, DM Sans
- Amber as the only accent
- No shadows, no rounded-everything

But the actual app uses:
- Inter (not DM Sans) as the body font
- Earthy green (`#7C8C6E`) as the accent, not amber
- `rounded-xl`, `rounded-2xl`, `rounded-3xl` everywhere
- `shadow-sm`, `shadow-lg`, `shadow-2xl` all over
- Multiple colour schemes (forest, sunset, midnight)

The design doc is now **actively misleading**. Either update it to match reality or realign the app.

---

## 💡 Big Change Suggestions (User Perspective)

### A. Weekly Planning View (The Missing Killer Feature)

There's no dedicated "plan my week" workflow. Students need to:
1. See all deadlines for the week
2. Block out study sessions
3. Break assignments into subtasks
4. Estimate time needed

**Proposal:** A `/plan` page with a week view that combines timetable blocks + deadline markers + draggable task allocation. Think of it as "Notion Calendar meets Things 3."

### B. Smart Notifications / Deadline Intelligence

Right now, the app is passive — you must come to it. For a tool that runs 24/7 on a home server, it should be proactive:
- "You have a CS2030 quiz in 48 hours and haven't started studying"
- "Your BT1101 assignment is due tonight — you're 60% done"
- Push these via Telegram automatically, not just on request

### C. Replace the Monolith Dashboard With a Customizable Widget Grid

The current 2-column dashboard is rigid. Different students have different needs:
- A student with 6 courses wants the Module Hub bigger
- A student doing thesis work wants Notes + Focus front and center
- During exam season, upcoming deadlines should dominate

**Proposal:** Let users drag/resize dashboard widgets. Even just 3-4 layout presets ("Exam Mode", "Normal", "Project Mode") would help.

### D. Mobile Experience

The app claims no mobile support, but students check schedules from their phones constantly. At minimum:
- The dashboard should stack into a single column on mobile
- The timetable should be viewable as a day view on mobile
- Tasks should have a quick-add interface that works on phone

### E. Unified Search (⌘K)

With 10+ pages of content, there's no global search. A student should be able to press `⌘K` and type:
- "CS2030" → see assignments, tasks, notes, timetable for that module
- "quiz" → find all quiz-related items across everything
- "due tomorrow" → filtered deadline view

---

## Summary Priority Matrix

| Priority | Issue | Impact |
|----------|-------|--------|
| 🔴 P0 | Assignments page is a wall of red | Makes the whole app feel broken |
| 🔴 P0 | Dashboard is dead during breaks/weekends | 50%+ of the time, the front page is empty |
| 🔴 P0 | API key client-side exposure | Security vulnerability |
| 🟡 P1 | Grades stored in localStorage | Data loss risk |
| 🟡 P1 | Focus timer not connected to tasks | Key workflow gap |
| 🟡 P1 | God component files (800-1700 lines) | Maintenance burden |
| 🟡 P1 | Design system doc is outdated | Misleads any future development |
| 🟢 P2 | Navigation hierarchy | Cognitive overload |
| 🟢 P2 | Weekly planning view | Missing flagship feature |
| 🟢 P2 | ⌘K global search | Power-user productivity |
| 🟢 P2 | Mobile responsiveness | Accessibility gap |
