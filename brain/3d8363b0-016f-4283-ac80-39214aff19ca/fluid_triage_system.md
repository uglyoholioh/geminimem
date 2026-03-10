# Fluid Time & Triage: The Frictionless Core

If we combine **Proposal 2 (Fluid Time)** and **Proposal 3 (Inbox Zero Triage)**, we can create an organization system that takes less than 5 seconds a day to maintain, while still giving you complete control over your schedule. 

Here is how these two systems would work together to eliminate planning fatigue:

---

## 1. The "Triage Inbox" (Replacing the Sorting Gauntlet)

Currently, a core problem is that looking at a list of 15 unscheduled Canvas assignments is overwhelming, and dragging them one-by-one onto a calendar grid is tedious.

**The Solution: Rapid-Fire Triage**
When you enter the "Triage" mode, the complex calendar disappears. You are presented with **one task at a time** in the center of the screen, like flashcards (or Tinder for tasks).

You don't need to pick an exact hour. You only need to declare your *intent* using keyboard shortcuts:
*   `[1]` **Do Today** (AI finds the next open block today)
*   `[2]` **Do Tomorrow** (AI finds a block tomorrow)
*   `[3]` **Do This Weekend** (AI saves it for Saturday/Sunday)
*   `[Space]` **Auto-Schedule** (AI places it optimally before the deadline)
*   `[S]` **Snooze** (Hide it in the backlog for now)

**Why this works:** You can sort 10 tasks in 10 seconds. You are making high-level strategic decisions ("I'll do this today"), and the AI handles the low-level logistical execution ("It will fit at 2:00 PM").

---

## 2. "Fluid Time" (The Self-Healing Calendar)

If you tell the AI to slot a task for "Today", it might place it at 2:00 PM. But what happens if 2:00 PM comes and goes, and you played video games instead? In a normal planner, that task turns red, and you feel guilty and have to manually drag it to 4:00 PM.

**The Solution: Liquid Blocks vs. Rigid Blocks**
The calendar differentiates between two types of time:
1.  **Rigid Blocks:** Real-world commitments (e.g., a CS101 Lecture synced from Google Calendar, or a meeting). These *never* move.
2.  **Liquid Blocks:** Your study tasks and assignments. These represent your *queue* mapped onto time.

**How Fluidity Works:**
If a Liquid Block (e.g., "Write Essay") is scheduled from 2:00-3:00 PM, and the clock hits 3:15 PM but you haven't started playing the Focus Timer for it... **it silently slides down the timeline.** 

*   It behaves like water. It flows around your Rigid Blocks (classes) and pools into your empty free time.
*   If today runs out of free time, it spills over into tomorrow.
*   The Companion handles this gracefully: *(Byte: "essay block missed. shifting grid downward by 60 mins.")*

**Why this works:** The schedule is no longer a prison you fail to conform to; it's a dynamic reflection of reality. You never have to "fix" your schedule again, because the schedule fixes itself.

---

## The Workflow in Practice

1. **Intake:** You sync Canvas. 3 new assignments appear in the Triage Inbox.
2. **Sort:** You press `T` to open Triage.
    * *Read Chapter 4* -> Press `1` (Today)
    * *Math Pset* -> Press `Space` (Auto-schedule based on Friday deadline)
    * *Watch lecture video* -> Press `3` (Weekend)
3. **Execute:** You look at the Dashboard. It says "Up Next: Read Chapter 4". You hit Start when you're ready.
4. **Adapt:** If you decide to take a 2-hour nap instead, the grid silently shifts "Read Chapter 4" to after dinner. Zero guilt, zero dragging, zero chores.

How does this workflow feel? If you approve, I can completely refactor the `Planner` and `SortingGauntlet` components to build this Triage + Fluid engine.
