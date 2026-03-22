# Planner Page Redesign — Implementation Plan

> **Executor**: Gemini Flash. Every instruction must be explicit and unambiguous.
> **Scope**: Frontend-only. Zero backend changes. One page file rewritten, one component directory cleaned up.

---

## Overview

Rewrite [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx) to deliver a cleaner, more actionable planner. The page answers three questions in a single scroll: "Am I behind?", "What's today?", "What's next?". No new APIs are needed — all data comes from existing endpoints.

> [!IMPORTANT]
> **The app uses a CSS-variable theme system** (light/dark modes + color schemes). Never hardcode hex colours like `#050505`. Always use the theme's CSS variables via Tailwind classes: `bg-base`, `text-primary`, `text-secondary`, `text-muted`, `bg-surface`, `bg-surface-hover`, `border-border`, `text-accent`, `bg-accent`, etc. These are defined in [globals.css](file:///Users/oli/Desktop/CraftCanvas/frontend/app/globals.css).

---

## Proposed Changes

### Frontend Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/planner/page.tsx)

**Full rewrite** of this file. The new file should be approximately 400–500 lines. Below is the exact specification.

##### Imports

```tsx
'use client'

import React, { useEffect, useState, useMemo, useRef } from 'react'
import { api } from '@/lib/api'
import {
    Sparkles, Loader2, CheckCircle2, Calendar, ArrowRight,
    RefreshCw, Clock, ChevronDown, ChevronRight, Plus,
    MoreHorizontal, X, MapPin, AlertTriangle
} from 'lucide-react'
import { format, isToday, isTomorrow, addDays, parseISO, differenceInCalendarDays, startOfDay } from 'date-fns'
import clsx from 'clsx'
import { useUIStore } from '@/lib/uiStore'
import type { Task, Assignment, TimetableSlot } from '@/lib/types'
```

##### Types

Define a local `PlannerItem` union type at the top of the file:

```tsx
type PlannerItem =
  | { kind: 'class'; id: number; time: string; endTime: string; title: string; venue: string | null; courseCode: string; courseColour: string | null }
  | { kind: 'task'; id: number; time: string | null; title: string; courseCode: string | null; courseColour: string | null; priority: string; estimatedMinutes: number | null; status: string }
  | { kind: 'assignment'; id: number; time: string | null; title: string; courseCode: string | null; courseColour: string | null; dueAt: string | null; pointsPossible: number | null; userStatus: string; canvasUrl: string | null }
```

##### State Variables

```tsx
const [loading, setLoading] = useState(true)
const [isSyncing, setIsSyncing] = useState(false)
const [quickAddOpen, setQuickAddOpen] = useState(false)
const [quickAddText, setQuickAddText] = useState('')
const [quickAddLoading, setQuickAddLoading] = useState(false)

// Raw data from API
const [tasks, setTasks] = useState<Task[]>([])
const [assignments, setAssignments] = useState<Assignment[]>([])
const [todaySlots, setTodaySlots] = useState<TimetableSlot[]>([])

// Overdue overflow menu
const [overdueMenuId, setOverdueMenuId] = useState<string | null>(null)

// Horizon expand state
const [horizonExpanded, setHorizonExpanded] = useState(false) // for "Next Week" bucket
```

##### Data Fetching

One `fetchData` function called on mount. Uses `Promise.all` with three calls:

```tsx
const fetchData = async () => {
    setLoading(true)
    try {
        const [tasksRes, assignmentsRes, timetableRes] = await Promise.all([
            api.get('/tasks'),
            api.get('/assignments'),
            api.get(`/timetable/today`)
        ])
        setTasks(tasksRes.tasks || [])
        setAssignments(assignmentsRes.assignments || [])
        setTodaySlots(timetableRes.slots || [])
    } catch (err) {
        console.error("Failed to fetch planner data", err)
    } finally {
        setLoading(false)
    }
}
```

> [!IMPORTANT]
> Do NOT filter tasks/assignments in the fetch function. Filtering happens in `useMemo` hooks below. This keeps `fetchData` simple and lets multiple derived views share the same raw data.

##### Derived Data (useMemo)

###### 1. `overdueItems`

```tsx
const overdueItems = useMemo(() => {
    const todayStr = format(new Date(), 'yyyy-MM-dd')
    const overdueTasks: PlannerItem[] = tasks
        .filter(t => t.status !== 'completed' && t.status !== 'cancelled' && t.scheduled_date && t.scheduled_date < todayStr)
        .map(t => ({ kind: 'task', id: t.id, time: null, title: t.title, courseCode: t.course_code, courseColour: t.course_colour ?? null, priority: t.priority, estimatedMinutes: t.estimated_minutes, status: t.status }))

    const overdueAssignments: PlannerItem[] = assignments
        .filter(a => a.user_status !== 'submitted' && a.user_status !== 'graded' && a.due_at && a.due_at < todayStr)
        .map(a => ({ kind: 'assignment', id: a.id, time: null, title: a.title, courseCode: a.course_code ?? null, courseColour: a.course_colour ?? null, dueAt: a.due_at, pointsPossible: a.points_possible, userStatus: a.user_status, canvasUrl: a.canvas_url }))

    return [...overdueTasks, ...overdueAssignments]
}, [tasks, assignments])
```

Note: for assignments, compare `a.due_at` as a string — it's an ISO datetime string. Extract just the date portion: `a.due_at?.split('T')[0]` and compare that against `todayStr`.

###### 2. `todayTimeline`

Interleaves classes, tasks scheduled for today, and assignments due today into a single chronologically-sorted list.

```tsx
const todayTimeline = useMemo(() => {
    const todayStr = format(new Date(), 'yyyy-MM-dd')
    const items: PlannerItem[] = []

    // Classes
    todaySlots.forEach(s => {
        items.push({
            kind: 'class',
            id: s.id,
            time: s.start_time || (s.start?.split('T')[1]?.substring(0, 5)) || '00:00',
            endTime: s.end_time || (s.end?.split('T')[1]?.substring(0, 5)) || '00:00',
            title: `${s.module_code} ${s.lesson_type || ''}`.trim(),
            venue: s.venue,
            courseCode: s.module_code,
            courseColour: s.course_colour
        })
    })

    // Today's tasks (scheduled_date == today, not completed)
    tasks.filter(t => t.status !== 'completed' && t.status !== 'cancelled')
        .filter(t => t.scheduled_date === todayStr || t.due_date === todayStr)
        .forEach(t => {
            items.push({
                kind: 'task',
                id: t.id,
                time: t.scheduled_time || t.due_time || null,
                title: t.title,
                courseCode: t.course_code,
                courseColour: t.course_colour ?? null,
                priority: t.priority,
                estimatedMinutes: t.estimated_minutes,
                status: t.status
            })
        })

    // Today's assignments (due_at starts with today's date, not submitted)
    assignments.filter(a => a.user_status !== 'submitted' && a.user_status !== 'graded')
        .filter(a => a.due_at?.startsWith(todayStr))
        .forEach(a => {
            items.push({
                kind: 'assignment',
                id: a.id,
                time: a.due_at ? format(parseISO(a.due_at), 'HH:mm') : null,
                title: a.title,
                courseCode: a.course_code ?? null,
                courseColour: a.course_colour ?? null,
                dueAt: a.due_at,
                pointsPossible: a.points_possible,
                userStatus: a.user_status,
                canvasUrl: a.canvas_url
            })
        })

    // Sort: timed items first (by time), then untimed items at the end
    return items.sort((a, b) => {
        const timeA = a.time || 'ZZ:ZZ'
        const timeB = b.time || 'ZZ:ZZ'
        return timeA.localeCompare(timeB)
    })
}, [tasks, assignments, todaySlots])
```

> [!NOTE]
> `Task.scheduled_time` and `Task.due_time` are stored as `"HH:MM:SS"` or `"HH:MM"` strings. For `TimetableSlot`, `start_time` is `"HH:MM"`. Normalize to `"HH:MM"` for sorting.

###### 3. `horizonItems`

Group items by day for the next 7 days, plus a "next week" bucket for days 8–14.

```tsx
type HorizonGroup = { label: string; dateStr: string; items: PlannerItem[] }

const horizonGroups = useMemo(() => {
    const today = startOfDay(new Date())
    const todayStr = format(today, 'yyyy-MM-dd')
    const groups: HorizonGroup[] = []

    // Build date buckets for next 7 days
    for (let i = 1; i <= 7; i++) {
        const d = addDays(today, i)
        const dateStr = format(d, 'yyyy-MM-dd')
        const label = i === 1 ? 'Tomorrow' : format(d, 'EEEE, d MMM')
        groups.push({ label, dateStr, items: [] })
    }

    // "Next Week" bucket for days 8-14
    const nextWeekGroup: HorizonGroup = { label: 'Next Week', dateStr: 'next-week', items: [] }

    // Assignments
    assignments
        .filter(a => a.user_status !== 'submitted' && a.user_status !== 'graded' && a.due_at)
        .forEach(a => {
            const dueDate = a.due_at!.split('T')[0]
            if (dueDate <= todayStr) return // skip overdue/today
            const daysDiff = differenceInCalendarDays(parseISO(dueDate), today)

            const item: PlannerItem = {
                kind: 'assignment', id: a.id, time: a.due_at ? format(parseISO(a.due_at), 'HH:mm') : null,
                title: a.title, courseCode: a.course_code ?? null, courseColour: a.course_colour ?? null,
                dueAt: a.due_at, pointsPossible: a.points_possible, userStatus: a.user_status, canvasUrl: a.canvas_url
            }

            if (daysDiff >= 1 && daysDiff <= 7) {
                groups[daysDiff - 1].items.push(item)
            } else if (daysDiff >= 8 && daysDiff <= 14) {
                nextWeekGroup.items.push(item)
            }
        })

    // Tasks with due_date
    tasks
        .filter(t => t.status !== 'completed' && t.status !== 'cancelled' && t.due_date)
        .forEach(t => {
            const daysDiff = differenceInCalendarDays(parseISO(t.due_date!), today)
            if (daysDiff < 1) return // skip overdue/today

            const item: PlannerItem = {
                kind: 'task', id: t.id, time: t.due_time || null,
                title: t.title, courseCode: t.course_code, courseColour: t.course_colour ?? null,
                priority: t.priority, estimatedMinutes: t.estimated_minutes, status: t.status
            }

            if (daysDiff >= 1 && daysDiff <= 7) {
                groups[daysDiff - 1].items.push(item)
            } else if (daysDiff >= 8 && daysDiff <= 14) {
                nextWeekGroup.items.push(item)
            }
        })

    // Filter out empty day groups, keep nextWeekGroup if non-empty
    const result = groups.filter(g => g.items.length > 0)
    if (nextWeekGroup.items.length > 0) result.push(nextWeekGroup)
    return result
}, [tasks, assignments])
```

###### 4. `progressStats`

```tsx
const progressStats = useMemo(() => {
    const todayStr = format(new Date(), 'yyyy-MM-dd')
    const actionableToday = todayTimeline.filter(i => i.kind !== 'class')
    // We can't easily count completed items in the current data since completed tasks
    // are already filtered out of `tasks`. We'll show total actionable items count
    // and total estimated time.
    const totalMinutes = actionableToday.reduce((sum, item) => {
        if (item.kind === 'task') return sum + (item.estimatedMinutes || 30)
        return sum + 60 // default 1hr for assignments
    }, 0)
    return { totalItems: actionableToday.length, totalMinutes }
}, [todayTimeline])
```

##### Action Handlers

###### `markDone(item: PlannerItem)`

```tsx
const markDone = async (item: PlannerItem) => {
    try {
        if (item.kind === 'task') {
            await api.put(`/tasks/${item.id}`, { status: 'completed' })
        } else if (item.kind === 'assignment') {
            await api.put(`/assignments/${item.id}`, { user_status: 'submitted' })
        }
        // Optimistically remove from local state
        if (item.kind === 'task') {
            setTasks(prev => prev.map(t => t.id === item.id ? { ...t, status: 'completed' } : t))
        } else {
            setAssignments(prev => prev.map(a => a.id === item.id ? { ...a, user_status: 'submitted' } : a))
        }
        useUIStore.getState().addToast("Done!", "success")
    } catch (err) {
        useUIStore.getState().addToast("Failed to update", "error")
    }
}
```

> [!IMPORTANT]
> Optimistic update: instead of removing the item from the array (which causes flicker issues), update the status field in the local state. The `useMemo` filters will then exclude it on the next render. This is the correct pattern.

###### `carryForward(item: PlannerItem)`

```tsx
const carryForward = async (item: PlannerItem) => {
    try {
        if (item.kind === 'task') {
            await api.post('/sweep/carry-forward', { task_ids: [item.id] })
            setTasks(prev => prev.filter(t => t.id !== item.id))
        }
        // For assignments, we can't really "carry forward" — they have fixed due dates.
        // Just mark for tomorrow's attention by updating scheduled_date if the backend supports it.
        // For now, only tasks support carry-forward.
        useUIStore.getState().addToast("Moved to tomorrow", "success")
        setOverdueMenuId(null)
    } catch (err) {
        useUIStore.getState().addToast("Failed to move", "error")
    }
}
```

###### `handleSync()`

Same as current — calls `api.post('/sync/all')` then `fetchData()`.

###### `handleQuickAdd()`

```tsx
const handleQuickAdd = async () => {
    if (!quickAddText.trim() || quickAddLoading) return
    setQuickAddLoading(true)
    try {
        await api.post('/fluid/parse-natural', { text: quickAddText })
        setQuickAddText('')
        setQuickAddOpen(false)
        await fetchData() // Refresh to pick up the new task
        useUIStore.getState().addToast("Task added", "success")
    } catch (err) {
        useUIStore.getState().addToast("Failed to add task", "error")
    } finally {
        setQuickAddLoading(false)
    }
}
```

##### JSX Structure

The full render tree. **Follow this structure exactly.**

```
<div className="min-h-screen bg-base text-primary selection:bg-accent/30">
  <div className="max-w-2xl mx-auto px-6 py-16 sm:py-24 space-y-16">

    {/* ── HEADER ── */}
    <header> ... </header>

    {/* ── SECTION 1: OVERDUE ── (conditional) */}
    {overdueItems.length > 0 && <section> ... </section>}

    {/* ── SECTION 2: TODAY TIMELINE ── */}
    <section> ... </section>

    {/* ── SECTION 3: HORIZON ── (conditional) */}
    {horizonGroups.length > 0 && <section> ... </section>}

  </div>
</div>
```

###### Header (exact markup)

```tsx
<header className="flex items-start justify-between gap-4">
    <div className="space-y-1.5">
        <h1 className="text-4xl sm:text-5xl font-serif font-medium tracking-tight leading-none">Today</h1>
        <p className="text-[10px] font-bold uppercase tracking-[0.4em] text-muted">
            {format(new Date(), 'EEEE, dd MMMM')}
        </p>
        {/* Progress info */}
        {progressStats.totalItems > 0 && (
            <p className="text-xs text-secondary mt-2">
                {progressStats.totalItems} items · ~{Math.round(progressStats.totalMinutes / 60 * 10) / 10}h estimated
            </p>
        )}
    </div>
    <div className="flex items-center gap-2 shrink-0">
        {/* Quick Add */}
        <button
            onClick={() => setQuickAddOpen(!quickAddOpen)}
            className="p-2.5 rounded-xl bg-surface border border-border hover:border-accent hover:text-accent transition-all active:scale-95"
        >
            <Plus className="h-4 w-4" />
        </button>
        {/* Sync */}
        <button
            onClick={handleSync}
            disabled={isSyncing}
            className="p-2.5 rounded-xl bg-surface border border-border hover:border-accent hover:text-accent transition-all active:scale-95"
        >
            <RefreshCw className={clsx("h-4 w-4", isSyncing && "animate-spin")} />
        </button>
    </div>
</header>

{/* Quick Add Input (expandable) */}
{quickAddOpen && (
    <div className="flex items-center gap-2 -mt-8">
        <input
            autoFocus
            value={quickAddText}
            onChange={e => setQuickAddText(e.target.value)}
            onKeyDown={e => e.key === 'Enter' && handleQuickAdd()}
            placeholder='e.g. "Read chapter 5 for CS2103T by Friday"'
            className="flex-1 bg-surface border border-border rounded-xl px-4 py-2.5 text-sm text-primary placeholder:text-muted focus:outline-none focus:border-accent"
        />
        <button
            onClick={handleQuickAdd}
            disabled={quickAddLoading}
            className="px-4 py-2.5 bg-accent text-white rounded-xl text-xs font-bold uppercase tracking-wider hover:bg-accent-hover transition-all active:scale-95 disabled:opacity-50"
        >
            {quickAddLoading ? <Loader2 className="h-4 w-4 animate-spin" /> : 'Add'}
        </button>
    </div>
)}
```

###### Overdue Section

```tsx
{overdueItems.length > 0 && (
    <section className="space-y-4">
        {/* Section label */}
        <div className="flex items-center gap-3">
            <AlertTriangle className="h-3.5 w-3.5 text-urgency" />
            <span className="text-[10px] font-bold uppercase tracking-[0.4em] text-urgency">
                Needs Decision
            </span>
            <div className="h-px flex-1 bg-urgency/20" />
        </div>

        <div className="space-y-2">
            {overdueItems.map(item => {
                const itemKey = `overdue-${item.kind}-${item.id}`
                return (
                    <div key={itemKey}
                        className="group relative bg-urgency-subtle border border-urgency/10 rounded-2xl px-5 py-4 transition-all hover:border-urgency/25"
                    >
                        <div className="flex items-center justify-between gap-4">
                            <div className="flex items-center gap-3 min-w-0 flex-1">
                                <div className="h-2 w-2 rounded-full bg-urgency shrink-0" />
                                <div className="min-w-0">
                                    <h3 className="text-sm font-medium truncate">{item.title}</h3>
                                    {item.courseCode && (
                                        <span className="text-[10px] font-bold uppercase tracking-widest text-muted">{item.courseCode}</span>
                                    )}
                                </div>
                            </div>
                            <div className="flex items-center gap-2 shrink-0">
                                <button onClick={() => markDone(item)}
                                    className="text-[10px] font-bold uppercase tracking-widest text-urgency hover:text-primary transition-colors px-2 py-1"
                                >Done</button>
                                {item.kind === 'task' && (
                                    <button onClick={() => carryForward(item)}
                                        className="text-[10px] font-bold uppercase tracking-widest text-muted hover:text-primary transition-colors px-2 py-1"
                                    >→ Tmrw</button>
                                )}
                            </div>
                        </div>
                    </div>
                )
            })}
        </div>

        {/* Sweep CTA: only if 4+ items */}
        {overdueItems.length >= 4 && (
            <button
                onClick={() => window.location.href = '/planner/sweep'}
                className="w-full py-3.5 rounded-2xl bg-surface border border-border text-urgency text-[10px] font-bold uppercase tracking-[0.3em] hover:bg-surface-hover transition-all flex items-center justify-center gap-2"
            >
                Start Daily Sweep <ArrowRight className="h-3.5 w-3.5" />
            </button>
        )}
    </section>
)}
```

###### Today Timeline Section

```tsx
<section className="space-y-6">
    {/* Section label */}
    <div className="flex items-center gap-3">
        <span className="text-[10px] font-bold uppercase tracking-[0.4em] text-muted">Line of Sight</span>
        <div className="h-px flex-1 bg-border" />
    </div>

    {todayTimeline.length === 0 ? (
        <div className="py-16 flex flex-col items-center justify-center gap-3 text-muted">
            <Sparkles className="h-8 w-8" />
            <p className="text-[10px] font-bold uppercase tracking-[0.3em]">Nothing on today's agenda</p>
        </div>
    ) : (
        <div className="space-y-3">
            {todayTimeline.map((item, idx) => {
                if (item.kind === 'class') {
                    return <ClassBlock key={`class-${item.id}`} item={item} />
                }
                return <ActionableItem key={`${item.kind}-${item.id}`} item={item} onDone={() => markDone(item)} />
            })}
        </div>
    )}
</section>
```

###### ClassBlock (inline sub-component, defined above the main component or inside it)

```tsx
function ClassBlock({ item }: { item: Extract<PlannerItem, { kind: 'class' }> }) {
    return (
        <div
            className="rounded-2xl border border-border/60 px-5 py-3.5 flex items-center justify-between"
            style={{ backgroundColor: item.courseColour ? `${item.courseColour}08` : undefined, borderLeftColor: item.courseColour || undefined, borderLeftWidth: '3px' }}
        >
            <div className="flex items-center gap-3">
                <span className="text-xs font-mono font-bold text-accent tabular-nums">{item.time}–{item.endTime}</span>
                <span className="text-sm font-medium">{item.title}</span>
            </div>
            {item.venue && (
                <div className="flex items-center gap-1 text-muted">
                    <MapPin className="h-3 w-3" />
                    <span className="text-[10px] font-mono">{item.venue}</span>
                </div>
            )}
        </div>
    )
}
```

###### ActionableItem (inline sub-component)

```tsx
function ActionableItem({ item, onDone }: { item: PlannerItem; onDone: () => void }) {
    if (item.kind === 'class') return null // shouldn't happen
    return (
        <div className="group flex items-center gap-3 rounded-2xl px-5 py-3.5 hover:bg-surface-hover transition-all">
            {/* Checkbox */}
            <button onClick={onDone}
                className="h-5 w-5 rounded-full border-2 border-border shrink-0 flex items-center justify-center
                           hover:border-accent hover:bg-accent/10 transition-all group-hover:border-accent/50"
            >
                <CheckCircle2 className="h-3.5 w-3.5 text-accent opacity-0 group-hover:opacity-40 transition-opacity" />
            </button>

            {/* Content */}
            <div className="flex-1 min-w-0">
                <div className="flex items-center gap-2">
                    {item.time && (
                        <span className="text-[11px] font-mono font-bold text-accent tabular-nums">{item.time}</span>
                    )}
                    <h3 className="text-sm font-medium truncate">{item.title}</h3>
                    {item.kind === 'assignment' && item.pointsPossible != null && item.pointsPossible > 0 && (
                        <span className="px-1.5 py-0.5 rounded text-[9px] font-bold bg-accent/15 text-accent uppercase tracking-wider shrink-0">
                            {item.pointsPossible}pts
                        </span>
                    )}
                </div>
                <div className="flex items-center gap-2 mt-0.5">
                    {item.courseCode && (
                        <span className="text-[10px] font-bold uppercase tracking-widest text-muted">{item.courseCode}</span>
                    )}
                    {item.kind === 'task' && item.estimatedMinutes && (
                        <span className="text-[10px] text-muted">~{item.estimatedMinutes}min</span>
                    )}
                </div>
            </div>

            {/* Focus button (hover only) */}
            <button
                onClick={() => window.location.href = `/focus?id=${item.id}&type=${item.kind}`}
                className="p-2 rounded-xl bg-accent text-white opacity-0 group-hover:opacity-100 transition-all hover:scale-105 active:scale-95 shrink-0"
            >
                <ArrowRight className="h-4 w-4" />
            </button>
        </div>
    )
}
```

###### Horizon Section

```tsx
{horizonGroups.length > 0 && (
    <section className="space-y-6 pb-24">
        {/* Section label */}
        <div className="flex items-center gap-3">
            <span className="text-[10px] font-bold uppercase tracking-[0.4em] text-muted">On the Horizon</span>
            <div className="h-px flex-1 bg-border" />
        </div>

        <div className="space-y-8">
            {horizonGroups.map(group => {
                const isNextWeek = group.dateStr === 'next-week'
                if (isNextWeek && !horizonExpanded) {
                    return (
                        <button key="next-week" onClick={() => setHorizonExpanded(true)}
                            className="w-full flex items-center justify-between py-3 text-muted hover:text-primary transition-colors"
                        >
                            <span className="text-[10px] font-bold uppercase tracking-[0.3em]">
                                Next Week ({group.items.length} items)
                            </span>
                            <ChevronDown className="h-4 w-4" />
                        </button>
                    )
                }

                return (
                    <div key={group.dateStr} className="space-y-2">
                        <h3 className="text-xs font-bold uppercase tracking-widest text-secondary">{group.label}</h3>
                        {group.items.map(item => (
                            <div key={`horizon-${item.kind}-${item.id}`}
                                className="flex items-center justify-between gap-3 rounded-xl px-4 py-3 border border-border/50 hover:border-border transition-all"
                            >
                                <div className="min-w-0 flex-1">
                                    <div className="flex items-center gap-2">
                                        <h4 className="text-sm font-medium truncate">{item.title}</h4>
                                    </div>
                                    <div className="flex items-center gap-2 mt-0.5">
                                        {item.courseCode && (
                                            <span className="text-[10px] font-bold uppercase tracking-widest text-muted">{item.courseCode}</span>
                                        )}
                                        {item.kind === 'assignment' && item.pointsPossible != null && item.pointsPossible > 0 && (
                                            <span className="text-[10px] text-accent font-bold">{item.pointsPossible}pts</span>
                                        )}
                                        {item.kind === 'assignment' && item.dueAt && (
                                            <span className="text-[10px] text-muted flex items-center gap-1">
                                                <Clock className="h-2.5 w-2.5" />
                                                {format(parseISO(item.dueAt), 'HH:mm')}
                                            </span>
                                        )}
                                    </div>
                                </div>
                            </div>
                        ))}
                    </div>
                )
            })}
        </div>
    </section>
)}
```

---

### Component Cleanup

#### [DELETE] Unused planner components

The following files in `frontend/components/planner/` are legacy views that are no longer used by the new planner page. **Delete them:**

- [DynamicListView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/DynamicListView.tsx)
- [FocusCardsView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/FocusCardsView.tsx)
- [GhostSlot.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/GhostSlot.tsx)
- [LinearFlowView.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/LinearFlowView.tsx)
- [SortingGauntlet.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/SortingGauntlet.tsx)
- [Vortex.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/Vortex.tsx)

> [!WARNING]
> **Keep** [TriageInbox.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/planner/TriageInbox.tsx) — it is still imported by the dashboard page (`app/page.tsx`).

---

### Files NOT Modified

| File | Reason |
|------|--------|
| `app/planner/sweep/page.tsx` | Sweep page is independent, still linked from overdue section |
| `backend/**` | Zero backend changes. All APIs already exist |
| `lib/types.ts` | All needed types already exist |
| `lib/api.ts` | Client already has all methods needed |
| `globals.css` | No new CSS needed — all styling via Tailwind + existing CSS vars |

---

## Verification Plan

### Automated Tests

#### 1. Frontend build check

Ensures the new page compiles without TypeScript errors:

```bash
cd /Users/oli/Desktop/CraftCanvas/frontend && npm run build
```

**Expected**: Build succeeds with no errors. Warnings about unused variables are acceptable.

#### 2. Existing unit tests still pass

```bash
cd /Users/oli/Desktop/CraftCanvas/frontend && npx vitest run
```

**Expected**: All existing tests pass. The planner page had no dedicated tests, so no tests should break.

### Browser Testing

#### 3. Visual verification

After starting the dev servers:

```bash
cd /Users/oli/Desktop/CraftCanvas/frontend && npm run dev
```

Open `http://localhost:3000/planner` in the browser and verify:

1. **Header**: "Today" heading is visible, date is correct, sync and plus buttons render
2. **Quick Add**: clicking the `+` button reveals the text input
3. **Overdue**: if there are overdue tasks, the red-tinted section appears with "Done" and "→ Tmrw" buttons
4. **Today Timeline**: classes show as distinct blocks with venue, tasks/assignments show as items with checkboxes
5. **Horizon**: future items grouped by day with proper date headers
6. **Theming**: page uses `bg-base` and `text-primary` — works in both light and dark mode (toggle via settings or browser)
7. **Sweep link**: if 4+ overdue items exist, the "Start Daily Sweep" button appears and navigates to `/planner/sweep`

#### 4. Verify deleted components don't break other imports

```bash
cd /Users/oli/Desktop/CraftCanvas/frontend && grep -r "DynamicListView\|FocusCardsView\|GhostSlot\|LinearFlowView\|SortingGauntlet\|Vortex" --include="*.tsx" --include="*.ts" app/ components/ lib/ | grep -v "node_modules"
```

**Expected**: No results, or only results from within `components/planner/` itself (which we're deleting). If any imports exist *outside* the planner directory, those files need updating too.
