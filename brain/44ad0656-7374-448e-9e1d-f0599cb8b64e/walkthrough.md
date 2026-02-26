# Module Brief Knowledge Base — Walkthrough

## Summary

Added a per-module knowledge base to the Module Brief tab. Users can upload PDFs, TXT, or MD files that are stored on disk, have their text extracted, and the extracted content is injected into the AI assistant's context for module-specific Q&A.

---

## Changes

### Backend (3 new/modified files)

| File | Change |
|------|--------|
| [module_file.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/module_file.py) | **[NEW]** `ModuleFile` SQLModel — stores file metadata, extracted text, and on-disk path |
| [module_files.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/module_files.py) | **[NEW]** CRUD router: upload (with PDF extraction via PyMuPDF), list, download, delete, and `/context` endpoint |
| [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py) | Registered `module_files_router` |
| [database.py](file:///Users/oli/Desktop/CraftCanvas/backend/database.py) | Imported `ModuleFile` in `init_db` |

### Frontend (1 modified file)

| File | Change |
|------|--------|
| [courses/\[id\]/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/%5Bid%5D/page.tsx) | Added Knowledge Base UI, file state/handlers, AI context injection |

---

## How It Works

1. **Upload** — User uploads a PDF/TXT/MD via the Upload button
2. **Extract** — Backend uses PyMuPDF to extract plain text from PDFs (capped at 50k chars)
3. **Store** — File saved to `data/module_files/{user_id}/{course_id}/`, metadata + extracted text in SQLite
4. **View/Delete** — File list shows name, size, extraction status; download & delete buttons
5. **AI Context** — When the user sends a chat message, all extracted file text is appended to the AI system prompt under a "Knowledge Base" section

---

## Verification

| Check | Result |
|-------|--------|
| `npx tsc --noEmit` | ✅ Clean |
| Backend imports | ✅ OK |
| Backend health | ✅ `{"status":"healthy"}` |
| Browser — Module Brief tab | ✅ Knowledge Base section renders with Upload button |

![Module Brief tab with Knowledge Base section](/Users/oli/.gemini/antigravity/brain/44ad0656-7374-448e-9e1d-f0599cb8b64e/module_brief_verified_final_1772013395502.png)

![Browser verification recording](/Users/oli/.gemini/antigravity/brain/44ad0656-7374-448e-9e1d-f0599cb8b64e/knowledge_base_verify_1772013135028.webp)
