# Start the Application

The goal is to start both the backend and frontend of the CraftCanvas application to ensure the development environment is ready for further work.

## Proposed Changes

### Backend
- Start the FastAPI backend using `uvicorn` in the `backend` directory.
- Ensure the virtual environment is activated.

### Frontend
- Verify that `npm run dev` is already running (as indicated by the user's terminal metadata).
- Ensure the frontend is accessible at `http://localhost:3000`.

## Verification Plan

### Automated Verification
- Check the status of the background command starting the backend.
- Use `curl` to verify the backend logic at `http://localhost:8000/docs`.

### Manual Verification
- The user can visit `http://localhost:3000` to ensure the frontend is responsive.
