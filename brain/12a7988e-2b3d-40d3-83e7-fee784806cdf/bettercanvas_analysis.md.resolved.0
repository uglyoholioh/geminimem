# Better Canvas Extension Analysis

After researching the popular "Better Canvas" (formerly BetterCampus) browser extension, I've identified several standout features that drive its popularity. Here's a breakdown of what we can learn and how we might adapt these features to power up the **Academic Life OS**:

## 1. The "What-If" GPA & Grade Simulator
*   **What it does:** Allows students to input hypothetical scores for upcoming assignments or finals to see exactly how it affects their total course grade and overall GPA.
*   **How it fits here:** Since we already sync assignment data from Canvas, adding an interactive "Grade Simulator" widget to the Module pages would be a killer feature. You could tweak sliders on pending assignments to see if you actually *need* to study hard for an upcoming quiz or if you've already secured your target grade.

## 2. Enhanced, Rich To-Do Lists
*   **What it does:** Reorganizes tasks with explicit priority levels, progress tracking, and inline assignment previews so students don't have to open the assignment page to read the brief.
*   **How it fits here:** Your new Planner Index is great for a "zero-maintenance" view, but we could add **inline Canvas descriptions** (fetched via the Canvas API) directly into a HoverCard or split-pane task preview. We could also introduce a feature that automatically sorts Canvas assignments by their percentage weight against the final grade.

## 3. Immediate "Smart Reminders"
*   **What it does:** Browser-wide popups for imminent learning deadlines (e.g., "Due in 2 hours").
*   **How it fits here:** Better Canvas uses browser popups; you have **Telegram**. Instead of relying solely on the morning Daily Brief, the backend scheduler could push an asynchronous "Imminent Deadline" Telegram alert exactly 24 or 12 hours before a major Canvas assignment is due.

## 4. Total Visual Customization (Course Cards)
*   **What it does:** Allows users to change course card colors, add custom header images, and apply custom themes, overriding Canvas defaults.
*   **How it fits here:** While your OS is strictly dark mode (per `DESIGN_SYSTEM.md`), allowing **custom cover images or dominant hex colors** for each Module mapping to timetable blocks, module page headers, and task tags would make the dashboard feel much more scannable and personalized.

## 5. Instant "Quick Views"
*   **What it does:** Adds quick-access dropdowns to hover over a class card and immediately see its announcements without fully loading a new page.
*   **How it fits here:** On the desktop web app, hovering over a module block in the Timetable or Dashboard could trigger a shadcn `HoverCard` that instantly displays the latest Canvas announcement, or the active syllabus link, avoiding unnecessary navigation clicks.

---
**Summary:**
The highest-value additions to adapt would arguably be the **"What-If" Grade Simulator** (giving users a massive psychological win in planning their study time) and the **Imminent Deadline Telegram pushes** (which maps perfectly to your existing bot infrastructure).
