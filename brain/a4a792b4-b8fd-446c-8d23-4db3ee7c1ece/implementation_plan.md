# Onboarding Revamp — User Categorisation + Module Verification

The current onboarding is a fixed 3-step flow (Canvas → Craft → Telegram) where every user sees every step, with Canvas being skippable. This change:

1. Adds a **user type selector** (Standard vs Power User) at the start so non-technical users skip Craft/Telegram entirely
2. Makes **Canvas API mandatory** (no skip button)
3. After Canvas validation + sync, adds a **module verification step** — users see their synced courses with checkboxes to mark which are active
4. **Courses page** already filters by `is_active` and has an "Active only / Show all" toggle — this naturally reflects the onboarding choices

## Proposed Changes

### Backend — User Model

#### [MODIFY] [user.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/user.py)

Add a `user_type` field (default `"standard"`) to the User model. Values: `"standard"` | `"power"`.

```diff
+    user_type: str = Field(default="standard")  # "standard" or "power"
```

---

### Backend — Courses Router

#### [MODIFY] [courses.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/courses.py)

Add two new endpoints:

1. **`PATCH /courses/{course_id}/active`** — Toggle `is_active` for a single course
2. **`PATCH /courses/bulk-active`** — Accept a dict of `{course_id: bool}` to bulk-set active states (used by the onboarding module verification step)

```python
class BulkActiveUpdate(BaseModel):
    updates: dict[int, bool]  # course_id -> is_active

@router.patch("/bulk-active")
async def bulk_update_active(data, user, session):
    # Update is_active for each course belonging to user

@router.patch("/{course_id}/active")
async def toggle_course_active(course_id, data, user, session):
    # Toggle is_active for single course
```

---

### Backend — Setup Router

#### [MODIFY] [setup.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/setup.py)

Add a **`POST /setup/sync-and-list-courses`** endpoint that:
1. Saves the Canvas credentials to Settings
2. Triggers `sync_courses()` (courses only, not full sync)
3. Returns the list of synced courses for verification

This is a combined endpoint so the frontend doesn't need multiple sequential calls.

---

### Frontend — Setup Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/setup/page.tsx)

Complete rewrite of the setup flow. New step structure:

| Step | Name | Who sees it |
|------|------|-------------|
| 1 | **User Type** — Pick "Standard" or "Power User" | Everyone |
| 2 | **Canvas API** — Enter token (mandatory, no skip) | Everyone |
| 3 | **Module Verification** — Checkboxes next to each synced course | Everyone |
| 4 | **Craft + Telegram** — Optional integrations | Power User only |

**Step 1 — User Type Selection:**
- Two cards: "Standard" (just Canvas + modules) and "Power User" (all integrations)
- Standard description: *"I just want my courses and assignments synced"*
- Power User description: *"I want Craft docs sync, Telegram notifications, and more"*

**Step 2 — Canvas API (mandatory):**
- Same as current Step 1 but without the "Skip" button
- On validate success → save Canvas credentials → trigger course sync → advance

**Step 3 — Module Verification:**
- Show a loading spinner while courses sync
- Once loaded, display all synced courses with checkboxes (all checked by default)
- User can uncheck courses that aren't relevant (old/irrelevant modules)
- On "Confirm Modules" → call `PATCH /courses/bulk-active` with the selections

**Step 4 — Extensions (Power User only):**
- Craft API key + Telegram (same as current Steps 2+3 combined into one)
- This step is skipped entirely for Standard users

---

### Frontend — Courses Page

#### [MODIFY] [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/page.tsx)

The courses page already has the correct behavior:
- Default: shows only `is_active` courses
- Toggle to "Show all" includes archived/inactive courses

Only small change needed: rename "Archived" label to "Hidden" or "Inactive" to match the onboarding language. And possibly add a quick toggle button on each course card to re-activate hidden courses.

---

### Backend — Auth Router

#### [MODIFY] [auth.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/auth.py)

Update `/auth/me` response to include `user_type` field so the frontend knows the user's type.

---

## Verification Plan

### Browser Testing

Walk through the full flow in the browser at `http://localhost:3000`:

1. **Register a new account** → should redirect to `/setup`
2. **Step 1**: Verify two user type cards appear. Pick "Standard"
3. **Step 2**: Enter Canvas credentials. "Skip" button should NOT exist. Validate & continue
4. **Step 3**: Verify synced modules appear with checkboxes. Uncheck one module, confirm
5. **Finish**: Should redirect to dashboard
6. **Courses page**: Verify the unchecked module is hidden by default, visible when toggling "Show all"
7. **Repeat for "Power User"**: Steps 1-3 same, then Step 4 with Craft/Telegram should appear

### Manual Verification (user)

After implementation, please:
1. Go through onboarding as a **Standard** user — confirm you never see Craft/Telegram
2. Go through as **Power User** — confirm you get the extra step
3. Check courses page to confirm hidden modules are truly hidden but recoverable
