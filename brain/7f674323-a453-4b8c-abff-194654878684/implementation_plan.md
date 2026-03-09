# Restore Missing Grade Models and Fix Backend Startup

The backend is currently failing to start due to a `ModuleNotFoundError` for `models.grade`. This file is missing, but it is imported by `routers/brief.py`.

## Proposed Changes

### [Component: Models]
#### [NEW] [grade.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/grade.py)
Recreate the missing `GradeComponent` and `GradeEntry` models using `SQLModel`.

```python
from typing import Optional
from sqlmodel import SQLModel, Field, Relationship
from datetime import datetime

class GradeComponent(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    course_id: int = Field(foreign_key="course.id")
    name: str
    weight: float = 0.0
    max_score: float = 100.0
    
    # Relationships
    # course: "Course" = Relationship(back_populates="grade_components")

class GradeEntry(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    component_id: int = Field(foreign_key="gradecomponent.id")
    user_id: int = Field(foreign_key="user.id")
    score: Optional[float] = None
    notes: Optional[str] = None
    updated_at: datetime = Field(default_factory=datetime.utcnow)
```

### [Component: Routers]
#### [MODIFY] [brief.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/brief.py)
Ensure the imports are correct after restoring the model. (Currently it fails at the import line).

## Verification Plan

### Automated Tests
1. Start the backend server and check if it remains running without `ModuleNotFoundError`:
   ```bash
   cd backend
   export PYTHONPATH=$PYTHONPATH:.
   python main.py
   ```
2. Verify the `/api/v1/brief/today` endpoint (requires auth).

### Manual Verification
1. Check existing server logs (`latest_server.log`) to confirm successful startup and absence of the previous error.
