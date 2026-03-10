# Phase 4: Ecosystem Synergy Implementation

We are interconnecting isolated features to create compounding value loops across the app. This phase focuses on connecting Tasks with Canvas Resources, Focus time with Fluid Scheduling, and External Data with the Triage Inbox.

## 1. Contextual Execution Loop (Tasks ↔ Canvas)
- [ ] Connect `fluid/next` backend to query `canvas_files` based on task title/course_code
- [ ] Update `Up Next` hero card frontend to render a "Resources" drawer or chips
- [ ] Implement click handler to download/view the Canvas file directly from the Dashboard

## 2. Reality-Correction Loop (Focus ↔ Fluid Schedule)
- [ ] Update frontend Timer completion state to present an "Overtime / Need More Time?" modal (+15m, +30m)
- [ ] Implement backend `add_overtime` API endpoint
- [ ] Ensure adding overtime triggers the `rebalance_schedule` logic to push back subsequent fluid tasks
- [ ] *Optional:* Update AI inference `parse-natural` to adjust time estimates based on historical overtime

## 3. Ingestion-Action Loop (External Sources ↔ Triage)
- [ ] Update Canvas Syncer: Pass new announcements to LLM to extract action items
- [ ] Automatically create tasks from these action items with `status="inbox"`
- [ ] Update `lectvideoanalyser` Telegram Bot: Ensure any action items generated via voice notes or chats are pushed to the CraftCanvas database via the new `/fluid/parse-natural` endpoint

## 4. Verification & UI Updates
- [ ] Test the Canvas resource attachment on a dummy task
- [ ] Test the Overtime modal in Focus mode and confirm the scheduler pushes back later tasks
- [ ] Test sending an email update/announcement and verifying the task lands in the Triage Inbox
- [ ] Update `walkthrough.md` with new features
