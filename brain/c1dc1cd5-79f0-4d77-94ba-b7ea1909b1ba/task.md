# Kanban Default View and Dashboard Updates

## 1. Tasks Page Update
- [x] Set default visualization to `kanban` instead of `list`.
- [x] Add a `groupBy` state to KanbanBoard (Status, Module, Priority) to allow users to switch how the board is organized.
- [x] Pass the `groupBy` state or let KanbanBoard manage its own grouping.

## 2. Kanban Board Component Update
- [x] Accept `groupBy` config (or manage it internally).
- [x] Group by Status: Columns = Inbox, In Progress, Completed.
- [x] Group by Priority: Columns = Urgent, High, Medium, Low.
- [x] Group by Module: Columns = dynamically generated based on task `course_code`.
- [x] Disable drag-and-drop between columns if grouping by something other than `status` (or implement the update logic for Priority/Module too). If grouping by Status, update `status`. If Priority, update `priority`. If Module, update `course_code`.

## 3. Dashboard Updates
- [x] Increase maximum tasks shown or ensure critical (urgent/high priority + due soon) show up.
- [x] Implement a segmented "bar levels" compact progress widget (10 discrete segments).
- [x] Make widget horizontally compact and button-like.
- [x] Prioritize criticality in task sorting (High/Urgent + Due Soon).
- [x] Remove duplicate Task and Today's Schedule sections on the dashboard.
