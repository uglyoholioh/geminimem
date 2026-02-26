# Dashboard & Task Progress Feature Walkthrough

The task functionality has been significantly updated with a highly requested "Percentage completion" feature!

## 1. Backend Updates
- The `tasks` SQL database table has been migrated to include a new `percentage_done` INTEGER field.
- The Python SQLModel definition (`backend/models/task.py`) was updated to include: `percentage_done: int = Field(default=0)`.
- Replaced backend API schemas in (`backend/routers/tasks.py`) to validate and return `percentage_done`. Creating, fetching, modifying, and completing tasks correctly maps this new field.

## 2. Frontend Board (Kanban) View
- Made the **Kanban Board** the default view for the tasks page!
- A brand new "**Group By**" toggle has been added to the top of the board. You can now seamlessly switch between organizing your Kanban board by:
  - **Status** (`Inbox`, `In Progress`, `Completed`)
  - **Priority** (`Urgent`, `High`, `Medium`, `Low`)
  - **Module** (Automatically generates columns for your `course_codes`!)
- Dragging and dropping tasks between columns automatically maps back to the database depending on the grouping context (e.g. moving a task to the `Urgent` column will instantly update its priority via API!)
- Small pill badges remain on task cards in the "In Progress" group within the Kanban Board view, displaying their percentage smoothly.

## 3. Frontend Dashboard Updates
- The Dashboard UI now drastically improves displaying critical info:
  1. `in_progress` tasks AND `Critical` tasks (High/Urgent priority + due within 3 days) are pulled securely to the absolute top of your queue.
  2. The remaining tasks fall back to a "closest deadlines" calculation.
- We have expanded the maximum number of tasks visible on the dashboard from 4 up to 6 to show more contextual information at a glance.
- The slider beneath `in_progress` tasks has been completely revamped into a **super-compact segmented level bar**.
  - It features 10 discrete, clickable levels (10% each) housed in a button-like minimalist pill.
  - This design provides a premium, "dashboard-ready" aesthetic that avoids generic glow or bulky sliders.
- **Enhanced Criticality Sorting**: The Dashboard now intelligently prioritizes **Critical Tasks** (Urgent/High priority + Due within 3 days) at the very top, even above regular in_progress tasks, ensuring you never miss a high-stakes deadline.
- **Dashboard Cleanup**: Removed redundant duplicate sections for both **Tasks** and **Today's Schedule**, streamlining the dashboard to show exactly one instance of each key component.
- **Kanban State Fix**: Dragging and dropping tasks between columns in the Kanban board now instantly and consistently saves the state to the backend, regardless of whether you're grouping by Status, Priority, or Module.
- **Premium Calendar View**: 
  - Upgraded the Tasks Calendar to support rich **Week** and **Day** time-grids alongside the standard Month view.
  - Events are elegantly styled with translucent backgrounds matching their module color, distinct priority-color dotted indicators, and smooth hover effects.
  - Completed tasks are displayed with a distinct dim, line-through appearance to keep the calendar uncluttered.

You can verify these enhancements by exploring the grouping in the tasks page or interacting with your refined dashboard!
