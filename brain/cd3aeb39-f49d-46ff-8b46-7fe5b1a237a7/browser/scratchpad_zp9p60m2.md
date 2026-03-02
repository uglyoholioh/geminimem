# Task: Redesign Dashboard UI/UX for CraftCanvas

## Objectives
- [x] Open http://localhost:3000
- [x] Take screenshot of the full dashboard (at load)
- [x] Scroll down and take another screenshot if needed
- [x] Document:
    - [x] Header area (greeting, stats badges, customise button)
    - [x] Left panel (Command Center — chat)
    - [x] Right panel (widgets — Agenda, Module Hub, etc.)
    - [x] Sidebar navigation
    - [x] Overall spacing, alignment, typography
    - [x] Visual bugs or issues
    - [x] Color scheme observations

## Findings
### Header Area
- Greeting: "Good morning, Oli" in a serif typeface.
- Stats Badges: 5 badges (Tasks, Classes, Min Focused, Due, Unread). Uppercase mono font, a bit cluttered.
- Customise Button: Uppercase mono font with border, looks slightly disconnected from the greeting.

### Left Panel (Command Center)
- Title: "COMMAND CENTER" with a lightning bolt.
- UI: Chat interface with bubbles, prompt chips (emoji + text), and a textarea.
- Spacing: Chat bubbles feel a bit cramped. Consistency of corner radii could be improved.

### Right Panel (Widgets)
- Upcoming Agenda: Simple list view.
- Module Hub: Grid of cards. Contains a redundant "MODULE HUB" header (one serif, one mono). Cards show "NEXT UP" and overdue status.

### Sidebar Navigation
- Slim icons on the left. High density of icons.

### Spacing & Typography
- Mix of serif (headings) and mono (utility/labels) works but needs more consistent application.
- Overall layout is a 2-column "main + sidebar" within the dashboard area.

### Visual Issues
- Duplicate "Module Hub" header.
- Low contrast for secondary text (e.g., date).
- The "stats badges" in the header are very repetitive in style and take up a lot of horizontal space.
- The "Clear" button in chat is small and tucked away.

### Color Scheme
- Dark mode: Slate/Zinc background.
- Muted accents: Green for active/user, red/orange for due/overdue.
