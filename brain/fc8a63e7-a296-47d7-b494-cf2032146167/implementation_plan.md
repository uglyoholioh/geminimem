# Remove Hover Animations from Dashboard Items

The user wants to remove animations when hovering over dashboard item sections. This includes the subtle lift effect on cards and any internal hover animations within the widgets.

## Proposed Changes

### [CSS Layer]

#### [MODIFY] [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css)
- Remove `transform: translateY(-2px)` from `.glass-card:hover`.
- Remove `transform 0.3s cubic-bezier(0.4, 0, 0.2, 1)` and `box-shadow 0.3s cubic-bezier(0.4, 0, 0.2, 1)` from the global transition rule if it's too aggressive, or specifically targeted. *Correction: I will just remove the hover transform and shadow change if they contribute to the "animation" feel.*

### [Dashboard Components]

#### [MODIFY] [ModuleHub.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/ModuleHub.tsx)
- Remove `hover:shadow-xl`, `hover:shadow-accent/5`, and `transition-all duration-300` from `ModuleCard`.
- Disable or adjust the `AnimatePresence` overlay if it's considered an "animation" the user wants gone (the prompt says "hovering dashboard item sections", which might imply the card-level lift/glow). I'll keep the overlay for now as it's functional, but remove the transform/shadow.

#### [MODIFY] [AgendaTimeline.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/AgendaTimeline.tsx)
- Remove `group-hover:scale-110` from the dot/icon.
- Remove `group-hover:shadow-md` and `group-hover:border-accent/30` from the content card to make it static.

#### [MODIFY] [UpcomingDeadlines.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/dashboard/widgets/UpcomingDeadlines.tsx)
- Remove `transition-all` and ensure `hover:bg-surface-hover` doesn't trigger a jittery animation.

## Verification Plan

### Manual Verification
1.  **Dashboard Hover**: Navigate to the dashboard and hover over:
    -   Module Hub cards.
    -   Agenda Timeline items.
    -   Upcoming Deadlines list items.
2.  Verify that there is no "lift" (translateY) or scale animation.
3.  Ensure the hover background color change remains (as it's usually not considered an "animation" but a state change, unless it's smoothly transitioned in a way the user dislikes). I will prioritize removing the *movement* and *scaling*.
