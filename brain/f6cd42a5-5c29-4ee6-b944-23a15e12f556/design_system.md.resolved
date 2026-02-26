# Academic Life OS — Design System & Frontend Structure

> Botanical minimal. Earth tones. Light + Dark. Clean.

## Design Mockups

````carousel
![Dashboard — Light Mode](/Users/oli/.gemini/antigravity/brain/f6cd42a5-5c29-4ee6-b944-23a15e12f556/designs/dashboard_light.png)
<!-- slide -->
![Dashboard — Dark Mode](/Users/oli/.gemini/antigravity/brain/f6cd42a5-5c29-4ee6-b944-23a15e12f556/designs/dashboard_dark.png)
<!-- slide -->
![Tasks Page — Light Mode](/Users/oli/.gemini/antigravity/brain/f6cd42a5-5c29-4ee6-b944-23a15e12f556/designs/tasks_light.png)
````

---

## Colour Palette

### Light Mode

| Token | Hex | Usage |
|---|---|---|
| `--bg-base` | `#FAFAF7` | Page background |
| `--bg-surface` | `#FFFFFF` | Cards, panels |
| `--bg-surface-hover` | `#F5F3EE` | Hover states |
| `--bg-sidebar` | `#F3F1EC` | Sidebar background |
| `--border` | `#E5E2DB` | Card/divider borders |
| `--border-subtle` | `#EDEBE6` | Very subtle dividers |
| `--text-primary` | `#2C2A25` | Headings, primary text |
| `--text-secondary` | `#6B6860` | Body text, descriptions |
| `--text-muted` | `#9C9889` | Timestamps, labels |
| `--accent` | `#7C8C6E` | Sage green — active states, links, focus |
| `--accent-hover` | `#6B7A5E` | Hover on sage elements |
| `--accent-subtle` | `#EEF1EA` | Sage-tinted backgrounds |
| `--urgency` | `#C4836A` | Terracotta — deadlines, high priority |
| `--urgency-subtle` | `#FDF0EB` | Terracotta-tinted backgrounds |
| `--gold` | `#B89B5E` | Star ratings, importance |

### Dark Mode

| Token | Hex | Usage |
|---|---|---|
| `--bg-base` | `#1A1A18` | Page background (warm charcoal) |
| `--bg-surface` | `#242422` | Cards, panels |
| `--bg-surface-hover` | `#2E2E2B` | Hover states |
| `--bg-sidebar` | `#1F1F1D` | Sidebar background |
| `--border` | `#333330` | Card/divider borders |
| `--border-subtle` | `#2A2A27` | Very subtle dividers |
| `--text-primary` | `#E8E4DD` | Headings, primary text |
| `--text-secondary` | `#A8A49B` | Body text |
| `--text-muted` | `#706D65` | Timestamps, labels |
| `--accent` | `#8B9F7B` | Sage green (slightly brighter) |
| `--accent-hover` | `#9DB08C` | Hover on sage elements |
| `--accent-subtle` | `#252B22` | Sage-tinted backgrounds |
| `--urgency` | `#D4916E` | Terracotta (slightly brighter) |
| `--urgency-subtle` | `#2D2320` | Terracotta-tinted backgrounds |
| `--gold` | `#C9A96E` | Star ratings |

### Module Colours

These are used for timetable blocks and module badge pills, consistent across both modes:

| Module | Light | Dark | Name |
|---|---|---|---|
| Slot 1 | `#7C8C6E` | `#8B9F7B` | Sage |
| Slot 2 | `#B87F5E` | `#C4916E` | Clay |
| Slot 3 | `#7A8B9C` | `#8A9BAC` | Stone Blue |
| Slot 4 | `#A8827A` | `#B8928A` | Dusty Rose |
| Slot 5 | `#8B8B6E` | `#9B9B7E` | Olive |
| Slot 6 | `#8C7A8E` | `#9C8A9E` | Mauve |
| Slot 7 | `#7A9082` | `#8AA092` | Eucalyptus |
| Slot 8 | `#9C8468` | `#AC9478` | Sienna |

---

## Typography

| Role | Font | Weight | Usage |
|---|---|---|---|
| Headings | **Instrument Serif** | 400 (italic for dates) | Page titles, card headers, brief date |
| Body | **Inter** | 400, 500, 600 | Body text, task titles, descriptions |
| Data/Labels | **DM Mono** | 400 | Times, dates, badges, status pills, code |

### Google Fonts import:
```
Inter:wght@400;500;600&family=Instrument+Serif:ital@0;1&family=DM+Mono:wght@400;500
```

### Scale

| Level | Size | Line Height | Font | Usage |
|---|---|---|---|---|
| `text-xs` | 11px | 16px | DM Mono | Labels, badge text |
| `text-sm` | 13px | 18px | Inter | Secondary info, timestamps |
| `text-base` | 15px | 22px | Inter | Body text, task titles |
| `text-lg` | 18px | 26px | Instrument Serif | Section headers |
| `text-xl` | 22px | 28px | Instrument Serif | Page card headings |
| `text-2xl` | 28px | 34px | Instrument Serif | Page titles |
| `text-date` | 16px | 22px | Instrument Serif italic | Brief date display |

---

## Layout Structure

```
┌──────────────────────────────────────────────────────┐
│ [Sidebar 56px] │         Main Content               │
│                │                                     │
│   ◉ Home       │  ┌─────────────────────────────────┐│
│   ○ Tasks      │  │  Content varies per page        ││
│   ○ Calendar   │  │                                  ││
│   ○ Courses    │  │  Dashboard: 3-column             ││
│   ○ AI         │  │  Tasks/Courses: single wide      ││
│   ○ Grades     │  │  Timetable: calendar view        ││
│   ━━━━━        │  │                                  ││
│   ○ Settings   │  └─────────────────────────────────┘│
└──────────────────────────────────────────────────────┘
```

### Sidebar
- Fixed 56px width, full height
- Icon buttons with tooltip labels on hover
- Active icon in `--accent` with `--accent-subtle` background pill
- Bottom-anchored settings icon

### Dashboard (3-column)
- **Left panel (300px)**: Daily Brief, Today's Schedule strip
- **Center (flex-1)**: Weekly timetable / calendar
- **Right panel (280px)**: AI Assistant chat

### Tasks / Assignments / Courses pages
- **Full width** content area (sidebar + single main column)
- Top bar with page title, search/add input, filter pills
- Card or list layout below

---

## Component Patterns

### Cards
```css
.card {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 16px;
}
.card:hover {
  border-color: var(--accent);
  background: var(--bg-surface-hover);
}
```

### Module Badge Pills
Small rounded pills with module-specific colour backgrounds at 15% opacity, text in the full colour:
```css
.badge-module {
  font-family: 'DM Mono', monospace;
  font-size: 11px;
  font-weight: 500;
  padding: 2px 8px;
  border-radius: 4px;
  background: color-mix(in srgb, var(--module-color) 15%, transparent);
  color: var(--module-color);
}
```

### Priority Indicators
- **Low**: no indicator (or `--text-muted` dot)
- **Medium**: `--text-secondary` dot
- **High**: `--urgency` dot
- **Urgent**: filled `--urgency` pill with white text

### Task Row
```
○  Task title here           [CS2103T]  ●  Due Mon 24
│  └── Inter 15px            └── badge  │  └── DM Mono 11px
│                                       └── priority dot
└── Circular checkbox (sage green when checked)
```

### Timetable Blocks
```css
.timetable-block {
  border-radius: 8px;
  padding: 8px 10px;
  background: var(--module-color);
  color: white;
  font-size: 12px;
}
.timetable-block .module-code {
  font-family: 'DM Mono', monospace;
  font-weight: 500;
  font-size: 11px;
  opacity: 0.9;
}
```

### AI Chat Bubbles
- User: `--accent-subtle` background, right-aligned
- Assistant: `--bg-surface` background, left-aligned
- Typing indicator: three sage dots pulsing

---

## CSS Custom Properties (globals.css)

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Instrument+Serif:ital@0;1&family=Inter:wght@400;500;600&display=swap');

:root {
  /* Light mode (default) */
  --bg-base: #FAFAF7;
  --bg-surface: #FFFFFF;
  --bg-surface-hover: #F5F3EE;
  --bg-sidebar: #F3F1EC;
  --border: #E5E2DB;
  --border-subtle: #EDEBE6;
  --text-primary: #2C2A25;
  --text-secondary: #6B6860;
  --text-muted: #9C9889;
  --accent: #7C8C6E;
  --accent-hover: #6B7A5E;
  --accent-subtle: #EEF1EA;
  --urgency: #C4836A;
  --urgency-subtle: #FDF0EB;
  --gold: #B89B5E;
  --radius: 12px;
  --radius-sm: 8px;
  --radius-xs: 4px;
}

.dark {
  --bg-base: #1A1A18;
  --bg-surface: #242422;
  --bg-surface-hover: #2E2E2B;
  --bg-sidebar: #1F1F1D;
  --border: #333330;
  --border-subtle: #2A2A27;
  --text-primary: #E8E4DD;
  --text-secondary: #A8A49B;
  --text-muted: #706D65;
  --accent: #8B9F7B;
  --accent-hover: #9DB08C;
  --accent-subtle: #252B22;
  --urgency: #D4916E;
  --urgency-subtle: #2D2320;
  --gold: #C9A96E;
}

body {
  font-family: 'Inter', system-ui, sans-serif;
  background: var(--bg-base);
  color: var(--text-primary);
  -webkit-font-smoothing: antialiased;
}

/* Scrollbar */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb {
  background: var(--border);
  border-radius: 3px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--text-muted);
}

/* Focus ring */
*:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
  border-radius: var(--radius-xs);
}
```

---

## Frontend File Structure

```
frontend/
├── app/
│   ├── layout.tsx          — Root layout, sidebar, font imports, theme provider
│   ├── globals.css         — CSS variables (above), Tailwind imports
│   ├── page.tsx            — Dashboard (3-column)
│   ├── tasks/page.tsx      — Tasks page
│   ├── timetable/page.tsx  — Full timetable view (FullCalendar)
│   ├── courses/page.tsx    — Courses overview
│   ├── ai/page.tsx         — AI chat (standalone page)
│   ├── grades/page.tsx     — Grades tracker
│   ├── settings/page.tsx   — Settings
│   └── api/[...path]/
│       └── route.ts        — Proxy to backend (injects X-API-Key)
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx     — 56px icon navigation
│   │   ├── ThemeToggle.tsx  — Light/dark mode switch
│   │   └── PageShell.tsx   — Page wrapper with consistent padding
│   ├── ui/                 — Base components (shadcn/ui customised)
│   ├── dashboard/
│   │   ├── DailyBrief.tsx
│   │   ├── TodaySchedule.tsx
│   │   └── QuickStats.tsx
│   ├── tasks/
│   │   ├── TaskList.tsx
│   │   ├── TaskRow.tsx
│   │   └── QuickAdd.tsx
│   ├── timetable/
│   │   └── CalendarView.tsx  — FullCalendar wrapper
│   └── ai/
│       └── ChatPanel.tsx
├── lib/
│   ├── api.ts              — Backend API client (fetch + key)
│   ├── utils.ts            — Tailwind cn() helper
│   └── theme.ts            — Theme context provider
├── hooks/
│   └── use-theme.ts        — Light/dark mode hook
├── tailwind.config.ts
└── package.json
```

---

## Tailwind Config Extensions

```ts
// tailwind.config.ts
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        base: 'var(--bg-base)',
        surface: 'var(--bg-surface)',
        'surface-hover': 'var(--bg-surface-hover)',
        sidebar: 'var(--bg-sidebar)',
        border: 'var(--border)',
        'border-subtle': 'var(--border-subtle)',
        primary: 'var(--text-primary)',
        secondary: 'var(--text-secondary)',
        muted: 'var(--text-muted)',
        accent: 'var(--accent)',
        'accent-hover': 'var(--accent-hover)',
        'accent-subtle': 'var(--accent-subtle)',
        urgency: 'var(--urgency)',
        'urgency-subtle': 'var(--urgency-subtle)',
        gold: 'var(--gold)',
      },
      fontFamily: {
        serif: ['Instrument Serif', 'Georgia', 'serif'],
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['DM Mono', 'Consolas', 'monospace'],
      },
      borderRadius: {
        DEFAULT: 'var(--radius)',
        sm: 'var(--radius-sm)',
        xs: 'var(--radius-xs)',
      },
    },
  },
}
```

---

## Theme Toggle Behaviour

- Store preference in `localStorage` key `theme`
- On load: check `localStorage`, fall back to `prefers-color-scheme`
- Toggle adds/removes `.dark` class on `<html>` element
- All colours switch automatically via CSS variables
