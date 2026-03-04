# Walkthrough: Restarting Web App

I have restarted the CraftCanvas web application by starting both the backend and frontend servers.

## Changes Made
- Started the backend FastAPI server using `uvicorn` on port 8000.
- Started the frontend Next.js server using `npm run dev` on port 3000.

## Verification
- **Backend**: Verified with `curl -I http://localhost:8000/docs` -> `200 OK`.
- **Frontend**: Verified with `curl -I http://localhost:3000` -> `200 OK`.

The application is now up and running!
