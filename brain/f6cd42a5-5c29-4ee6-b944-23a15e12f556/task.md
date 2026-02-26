# Academic Life OS — Opus Planning Session

## Deliverables

- [x] Read all documentation files
- [x] Read all existing backend and frontend code
- [x] Code audit + structural issues list
- [x] Phase 2 Fix Plan (ICS parser, timetable router, frontend)
- [x] Revised BUILD_PLAN.md
- [x] Phase 3 readiness check
- [x] Frontend architecture review

## Implementation

- [x] Fix `requirements.txt` — add python-dateutil
- [x] Fix `backend/services/ics_parser.py` — timezone, RRULE, summary parsing
- [x] Fix `backend/routers/timetable.py` — RRULE expansion, FullCalendar format
- [x] Fix `backend/database.py` — use config for DB URL
- [x] Fix `backend/main.py` — wire API key auth globally

## Frontend Implementation

- [x] `globals.css` — CSS variables, fonts, design tokens
- [x] `tailwind.config.ts` — extend colours/fonts from variables
- [x] `layout.tsx` — root layout with sidebar, theme provider, fonts
- [x] Sidebar component
- [x] Theme toggle (light/dark)
- [x] API proxy route
- [x] Dashboard page (3-column)
- [x] Tasks page
- [x] Timetable page

## Phase 5 — Daily Brief + Scheduled Jobs

- [x] AI service routing (local/external LLM)
- [x] Brief generator service
- [x] Brief router (today, history, regenerate)
- [x] APScheduler (daily brief + canvas sync)
- [x] Brief card in dashboard
- [x] Verification

## Task System Upgrade

- [x] Fix SGT timestamps in Task model + router
- [x] Rebuild tasks page with sidebar views
- [x] Build task detail slide-out panel
- [x] Manual input for module code, date, priority, tags, notes
- [x] Assignment → task auto-creation toggle

## Auth & Settings System

- [x] Backend: User model + bcrypt hashing
- [x] Backend: Auth router (register/login/logout/me)
- [x] Backend: Session-based dependency + update main.py
- [x] Backend: Bulk settings endpoint
- [x] Frontend: AuthProvider context
- [x] Frontend: Login page (glassmorphic)
- [x] Frontend: Settings page rebuild (tabbed, save button)
- [x] Frontend: Sidebar user avatar + logout
- [x] Verification

## Phase 10: Assignments, History & Module Detail Pages

- [x] Frontend: Assignments page (sidebar filters + list + detail panel)
- [x] Frontend: Brief History page (timeline + content viewer)
- [x] Frontend: Module Detail page (tabbed hub)
- [x] Backend: Fix assignments router (JSON body), enhance courses router
- [x] Verification

## Phase 11: Task System Polish (Due Gooder Inspiration)

- [x] Backend: Add `subtasks`, `recurrence_rule`, and `position` to Task model.
- [x] Backend: Update tasks router to support recurring task generation on completion.
- [x] Frontend: Add Subtasks checklist UI to Task Detail panel.
- [x] Frontend: Add Recurrence selector to Task Detail panel.
- [x] Frontend: Implement Kanban Board view toggle.
- [x] Frontend: Implement Calendar View toggle.
- [x] Frontend: Implement Drag-and-Drop reordering on list views.
- [x] Frontend: Due Gooder aesthetic polish (progress rings, playful completion animations).

## Phase 7: Multi-User Architecture Upgrade

- [x] Backend: Add `user_id` to all models + composite unique constraints
- [x] Backend: Add `api_key` to User model
- [x] Backend: Refactor `verify_auth` to return `User`
- [x] Backend: Update all routers to scope queries by `user_id`
- [x] Backend: Refactor Auth router (allow multi-registration)
- [x] Backend: Update background jobs to loop over users
- [x] DB: Wipe `data/db.sqlite` and restart
- [x] Verification (Data Isolation Test)

## Phase 8: User Onboarding Flow (Config Migration & Validation)

- [x] Backend: Create `routers/setup.py` with validation endpoints (`/validate-canvas`, `/validate-llm`, `/validate-telegram`)
- [x] Backend: Update background jobs and services to remove `.env` reliance (use `Settings`)
- [x] Frontend: Upgrade `/setup` UI into a 3-step wizard (Canvas -> AI -> Extensions)
- [x] Frontend: Wire up live validation calls before allowing users to proceed to next steps
- [x] Verification

## Phase 9: Local-Only AI Migration

- [x] Frontend: Remove Cloud LLM toggle from `/setup` wizard (default to local Ollama)
- [x] Frontend: Remove External LLM fields from the Settings page
- [x] Backend: Update `/validate-llm` to only ping the local Ollama instance
- [x] Backend: Refactor `ai_service.py` to strip out external routing and API key handling

## Phase 12: Activity & Meeting Finder (when2meet style)

- [x] Backend: Create `Meeting` model (title, share_token, user_id).
- [x] Backend: Create `MeetingParticipant` model (name, availability JSON array, meeting_id).
- [x] Backend: Create `routers/meetings.py` for CRUD and anonymous participant access via token.
- [x] Frontend: Create `/meetings` hub page to list user's created meetings.
- [x] Frontend: Create `/meetings/[token]` public page for anyone to participate.
- [x] Frontend: Build drag-to-highlight `AvailabilityGrid` component to compute overlaps.
- [x] Verification

## Phase 13: Advanced Meeting Features (LMS Manager Parity)

- [x] Backend: Update `Meeting` model with `is_locked` and `decision_mode`.
- [x] Backend: Create `MeetingProposal` and `MeetingVote` models.
- [x] Backend: Update meetings router for proposals, votes, settings, and returning `is_owner` flag.
- [x] Frontend: Make `/meetings` card clickable to route to the meeting page directly.
- [x] Frontend: Update `/[token]` page with Owner "Settings" modal to lock/unlock and switch modes.
- [x] Frontend: Implement Proposals and Voting sidebar inside `/[token]`.
- [x] Verification

## Phase 14: Craft API Integration
- [x] Backend: Replace `/validate-llm` with `/validate-craft` mapped to connect.craft.do.
- [x] Backend: Hardcode AI engine configuration limits to the application singleton and remove dynamic database lookups.
- [x] Frontend: Remove AI configuration input elements from Setup Wizard.
- [x] Frontend: Remove AI configuration tab from `/settings`.
- [x] Frontend: Implement Craft Bearer token validation and saving into the Setup route.
- [x] Frontend: Add Craft Workspace tab to the app Settings layout.
- [x] Verification
