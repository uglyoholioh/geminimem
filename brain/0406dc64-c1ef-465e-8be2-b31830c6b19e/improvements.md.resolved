# Proposed Improvements for CraftCanvas

After reviewing the current implementation of the Academic Life OS, I've identified several areas where we can elevate the experience, focusing on academic success and the unique "Dense Terminal" aesthetic.

## 1. AI-Powered Academic Companion
Currently, the AI handles chat and daily briefs well. We can extend this into active learning:
- **Active Recall (Quiz Mode)**: Generate multiple-choice or short-answer questions directly from the synced Canvas course files and announcements.
- **Smart Flashcards**: Automatically generate flashcards from lecture notes. We could integrate with Anki or build a simple terminal-style flashcard viewer in the app.
- **Semantic File Search**: Enhance the `/ai` session to allow searching through the content of all downloaded PDFs/PPTs from Canvas using the existing RAG pipeline.

## 2. Immersive Focus Environment
The Focus Mode is a great start. We can make it a truly world-class productivity space:
- **Ambient Soundscape**: Integrate a built-in audio player with curated, desaturated soundscapes (Lo-Fi, Heavy Rain, Busy NUS Library).
- **Post-Session Reflection**: After a focus session ends, have the AI prompt a 30-second reflection: *"What was the most challenging part of this task?"* to help with metacognition.
- **Picture-in-Picture (PiP) Timer**: Allow the focus timer to pop out into a small PiP window so you can see your progress while working in other apps.

## 3. Academic Analytics & Visibility
Make the "Life OS" more proactive about your academic standing:
- **Grade Simulator / CAP Tracker**: Build a view that predicts your final grade/CAP based on current scores and weighted averages. *"What do I need in the final exam to get an A?"*
- **Workload Heatmap**: A visualization that combines your NUSMods timetable with upcoming Assignment deadlines to identify "crunch weeks" before they happen.

## 4. UI/UX "Dense Terminal" Polish
Lean harder into the Terminal aesthetic for a more premium feel:
- **Command Palette (Cmd+K)**: Implement a powerful keyboard-centric palette for jumping between modules, creating tasks, or triggering AI commands without touching the mouse.
- **CRT / Terminal Effects**: Add an optional toggle for subtle CRT scanlines, flicker, and a slight screen glow to match the "Dense Terminal" vibe.
- **Typographic Micro-animations**: Use subtle typing animations for AI responses and "data stream" effects when syncing with Canvas.

## 5. Telegram Bot 2.0 (Interactive)
The current bot is great for quick capture. We can make it an interactive command center:
- **Interactive Checklist**: Send `/tasks` and get a message with inline toggle buttons to mark tasks as done without typing.
- **Voice-to-Task**: Send a voice note to the bot; it uses Whisper (via local Ollama or API) to transcribe and then parses the task automatically.
- **Assignment Reminders**: Not just a daily brief, but a gentle nudge 4 hours before a deadline with a specific "Get Started" link.

## 6. Deeper Craft Ecosystem Integration
Move beyond just pushing a daily note:
- **Bi-directional Task Sync**: If you check a task in Craft, the backend detects the update (via Craft API polling) and marks it as done in SQLite.
- **Module Wiki Auto-Generation**: Automatically build a "Module Wiki" in Craft for each course, linking lecture summaries, assignments, and AI-generated study guides into a structured folder hierarchy.
- **Meeting Minutes to Tasks**: If you record a project meeting in Craft, the AI can scan the new content and suggest new tasks for your Academic OS inbox.

## 7. Intelligent Workflow Automation
Reduce the administrative burden of being a student:
- **Auto-Email Drafts**: When a new announcement is posted about a "Missing Grade" or "Project Submission", the AI drafts a polite inquiry email for you to review and send.
- **Conflict Resolver**: The system identifies when a task's estimated time exceeds your free blocks in the NUSMods timetable and suggests moving the task or "Focusing" during a specific gap.
- **Financial Tracker (Optional)**: A simple terminal-style ledger for tracking student expenses, tuition payments, and meal plan balances, keeping everything in one "Academic Life OS".

---

> [!TIP]
> **Focusing on the "Power User":** These Round 2 ideas are designed to make the system feel like a personal Chief of Staff. I'm ready to prototype any of these for you.
