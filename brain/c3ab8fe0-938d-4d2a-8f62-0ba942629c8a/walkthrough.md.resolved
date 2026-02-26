# Walkthrough - AI Chat Filtering and Settings Updates

I have implemented AI-powered filtering and settings updates through the chat interface. Users can now use natural language to filter their views on the Tasks, Assignments, and Announcements pages, as well as update application settings directly via chat commands.

## Changes Made

### AI Service & Backend
- **[ai_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/ai_service.py)**: Updated the system prompt to instruct the AI on generating `filter_view` and `update_setting` actions.
- **[ActionCard.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/components/chat/ActionCard.tsx)**: Implemented handling for the new action types, including the correct API call for settings updates.

### Frontend Integrations
- **Tasks Page**: Added an "AI Filter" button that opens a chat drawer. The chat can now parse and apply filters locally (e.g., "Show high priority tasks").
- **Assignments Page**: Integrated the same AI chat functionality for filtering assignments.
- **Announcements Page**: Added AI-powered filtering for announcements, allowing users to find specific notifications more easily.

## Verification Results

### Manual Verification Steps
1. **Tasks Page**:
   - Clicked "AI Filter".
   - Asked "Show urgent tasks".
   - AI suggested a filter action; clicking "Apply" updated the task list correctly.
2. **Assignments Page**:
   - Filtered for specific modules via chat (e.g., "Filter for CS2103").
   - Verified the list updated to only show matching assignments.
3. **Announcements Page**:
   - Used "Show unread" command in chat.
   - Verified the view switched correctly.
4. **Settings Update**:
   - Asked chat to "Change my color scheme to blue".
   - AI generated an `update_setting` action; clicking "Update" correctly called the backend API.
