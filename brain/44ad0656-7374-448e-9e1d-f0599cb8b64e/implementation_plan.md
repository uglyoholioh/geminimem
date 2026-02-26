# Module Brief Knowledge Base

Add file upload, viewing, and AI extraction to the Module Brief tab so users can build a per-module knowledge base.

## Proposed Changes

### Backend

#### [NEW] [module_file.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/module_file.py)

```python
class ModuleFile(SQLModel, table=True):
    id, user_id, course_id, filename, content_type, size_bytes
    extracted_text: str  # plain text extracted from the file
    local_path: str      # path on disk in data/module_files/
    created_at: datetime
```

#### [NEW] [module_files.py (router)](file:///Users/oli/Desktop/CraftCanvas/backend/routers/module_files.py)

- `POST /courses/{course_id}/files` — upload file (PDF/TXT/MD/DOCX), extract text via PyMuPDF, store on disk
- `GET /courses/{course_id}/files` — list files for a course
- `GET /courses/{course_id}/files/{file_id}/download` — serve the original file
- `DELETE /courses/{course_id}/files/{file_id}` — delete file + disk cleanup
- `GET /courses/{course_id}/files/context` — return concatenated extracted text for AI context injection

#### [MODIFY] [main.py](file:///Users/oli/Desktop/CraftCanvas/backend/main.py)
- Register `module_files_router` at `/api/v1`

#### [MODIFY] [database.py](file:///Users/oli/Desktop/CraftCanvas/backend/database.py)
- Import `ModuleFile` in `init_db`

---

### Frontend

#### [MODIFY] [courses/\[id\]/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/courses/%5Bid%5D/page.tsx)

Add to the Module Brief tab:
- **Knowledge Base section** above the AI chat — file list with upload button
- Each file shows: name, size, type, date, download + delete actions
- Upload via drag-and-drop or click (PDF, TXT, MD accepted)
- Extracted text injected into the AI system prompt alongside assignments/grades/schedule

## Verification Plan

### Automated
- `npx tsc --noEmit` — TypeScript check
- `curl` upload/list/download/delete endpoints

### Manual
- Upload a PDF → verify text extraction → ask AI a question about its content
