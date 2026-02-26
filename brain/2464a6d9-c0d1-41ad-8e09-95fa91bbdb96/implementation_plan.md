# Implementation Plan - Daily Brief Fullscreen Mode

The goal is to provide a dedicated, distraction-free page for the Daily Brief, allowing users to focus on their AI-generated summary and interact with the chat in a larger view.

## Proposed Changes

### Frontend Components

#### [NEW] [BriefPage.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/brief/page.tsx)
- Create a new page at `/brief` that adapts the Daily Brief and Chat logic from the main dashboard.
- This page will use the full screen width and height.
- It will include a "Back to Dashboard" button.
- It will maintain the same styling (glassmorphism, vibrant accents) as the dashboard.

#### [MODIFY] [DashboardPage](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- Add a "Maximize" or "Fullscreen" icon button to the "Daily Brief & Chat" section header.
- This button will link to `/brief`.

### Logic Extraction (Optional but Recommended)
- If the logic in `page.tsx` is too intertwined, I will extract the Daily Brief and Chat components into standalone components in `frontend/components/chat/` to avoid duplication.
- Component 1: `DailyBriefSummary.tsx`
- Component 2: `DailyBriefChat.tsx`

## Verification Plan

### Manual Verification
1. Navigate to the Dashboard.
2. Click the "Fullscreen" button in the Daily Brief widget.
3. Verify that the browser navigates to `/brief`.
4. Verify that the Daily Brief summary and chat history are correctly displayed in the fullscreen view.
5. Verify that sending a chat message works in the fullscreen view.
6. Click the "Back to Dashboard" button and verify return to the main page.

### Automated Tests
- I will run existing Playwright tests to ensure no regressions on the dashboard.
- `npx playwright test frontend/e2e`
