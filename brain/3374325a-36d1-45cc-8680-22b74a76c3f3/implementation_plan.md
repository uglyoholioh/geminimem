# Full Testing Plan for CraftCanvas

This document outlines a comprehensive testing strategy for the CraftCanvas (Academic Life OS) application to ensure reliability, performance, and a seamless user experience.

## Overview

The testing strategy follows the standard testing pyramid:
1.  **Unit Tests**: High volume, fast execution, testing individual functions and components.
2.  **Integration Tests**: Testing interactions between components (e.g., API endpoints and services).
3.  **End-to-End (E2E) Tests**: Testing critical user flows from the user's perspective.
4.  **Manual Verification**: Final QA and visual checks.

---

## 1. Backend Testing (Python/FastAPI)

### Unit & Integration Tests
We use **Pytest** with **SQLModel** and an in-memory SQLite database.

- **Tools**: `pytest`, `httpx` (for AsyncClient), `sqlmodel`.
- **Existing Coverage**: Routers (`auth`, `courses`, `tasks`, etc.) and Services (`ai`, `calendar_sync`).
- **Proposed Enhancements**:
    - Increase coverage for edge cases in AI processing.
    - Mock external APIs (Canvas, Telegram) more strictly to ensure stability.
    - Implement `pytest-cov` for coverage reporting.

**Execution Command**:
```bash
cd backend
pytest
```

---

## 2. Frontend Testing (Next.js/React)

### Unit Tests
We use **Vitest** (or Jest) for component-level testing.

- **Tools**: `vitest`, `@testing-library/react`.
- **Areas of Focus**:
    - Utility functions (date formatting, state transitions).
    - UI Components (reusability and accessibility).
    - Hook logic (Zustand stores, custom hooks).

**Execution Command**:
```bash
cd frontend
npm run test
```

### End-to-End (E2E) Tests
We use **Playwright** for high-level browser-based testing.

- **Tools**: `@playwright/test`.
- **Critical Flows to Test**:
    - User Authentication (Login/Signup/Password Reset).
    - Dashboard Widget rendering and interaction.
    - Module creation and color customization.
    - Task management (Add/Edit/Delete).
    - AI Chat interactions.

**Execution Command**:
```bash
cd frontend
npx playwright test
```

---

## 3. Integration & System Testing

### Real-world Sync Scenarios
- **Canvas Sync**: Verify that data from a mock Canvas instance correctly populates the local database.
- **File Uploads**: Test processing of PDF/DOCX files through the AI pipeline.

### Database Migrations
- Verify that SQLAlchemy/Alembic migrations (if used) work as expected on production-like databases.

---

## 4. Manual Verification (QA Checklist)

- [ ] **Visual Consistency**: Check dark mode, responsive layouts, and animations.
- [ ] **Onboarding Flow**: Walk through the first-time user experience.
- [ ] **Cross-browser Compatibility**: Verify on Chrome, Safari, and Firefox.
- [ ] **Mobile Responsiveness**: Test on mobile viewports.

---

## 5. CI/CD Integration

- **GitHub Actions**:
    - Run backend tests on every PR.
    - Run frontend unit tests on every PR.
    - Run E2E tests on a staging environment before deployment.
    - Fail builds if coverage drops below a certain threshold (e.g., 80%).

---

## 6. Performance & Security

- **Lighthouse Scores**: Aim for 90+ in Performance, Accessibility, and SEO.
- **Dependency Audit**: Regular `npm audit` and `pip audit` checks.
- **SQL Injection/XSS**: Basic security headers and parameterization (SQLModel handles most by default).
