# Editable Dashboard Widgets

## Planning
- [x] Research current dashboard architecture
- [x] Draft implementation plan with widget ideas
- [x] User review & approval

## Execution
- [x] Create widget registry & config types
- [x] Build `WidgetShell` wrapper component (drag handle, remove, resize)
- [x] Create `DashboardLayoutManager` with grid + edit mode toggle
- [x] Persist layout to backend (user settings)
- [x] Build new widget components
  - [x] Grades Overview widget
  - [x] Pinned Notes widget
  - [x] Focus Timer mini widget
  - [x] Upcoming Deadlines widget
  - [x] Quick Add widget
  - [x] Announcements Feed widget
- [x] Migrate existing `AgendaTimeline` + `ModuleHub` into widget system
- [x] Add "Add Widget" picker panel
- [x] Wire up layout persistence via `/api/v1/settings`

## Verification
- [ ] Visual browser test: edit mode, add/remove/reorder widgets
- [ ] Persistence test: reload page and verify layout survives
- [ ] Manual verification of each new widget's data
