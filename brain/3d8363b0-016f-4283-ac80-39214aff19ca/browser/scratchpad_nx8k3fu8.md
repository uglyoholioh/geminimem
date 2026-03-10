# Task Checklist

- [ ] Add task "Review lecture 5 tonight" using Cmd+K
    - [x] Focus page
    - [x] Press Meta+K
    - [x] Type "Review lecture 5 tonight"
    - [ ] Press Enter
    - [ ] Verify toast/update (FAILED: Cmd+K Search returns 500; Quick Add returns 500)
- [ ] Open Triage Inbox and cycle priority
    - [ ] Click "Sort Inbox" button (Searching for this; not found on Dashboard/Planner/Tasks)
    - [ ] Press 'P' to cycle priority
    - [ ] Verify change
- [/] Verify "Explainable AI" reasoning tag on Dashboard hero
- [/] Click "Reshuffle" and ensure task cycles
- [ ] Provide final report with screenshots/logs if needed

*Notes:*
- Command palette opens with Meta+K.
- Both Cmd+K search and Quick Add task form are returning 500 Internal Server Error when sending POST/GET requests to `/api/v1/tasks` or `/api/v1/search`.
- Triage Inbox might be inaccessible if its trigger or content also returns 500.
- "Reshuffle" clicked on Dashboard but state didn't change (likely due to "0 tasks" in system).
- Reasoning tag not visible.
