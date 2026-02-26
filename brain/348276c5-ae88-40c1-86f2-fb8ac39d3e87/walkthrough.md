# Walkthrough - Include User Name in Dashboard Greeting

I have updated the dashboard greeting to include the user's display name, providing a more personalized experience.

## Changes Made

### Frontend

#### [page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/page.tsx)
- Integrated `useAuth` to access the current user's profile information.
- Updated the header greeting logic to append the user's `display_name` if available.

```tsx
<h1 className="font-serif text-2xl text-primary leading-tight">
  {today.getHours() < 12 ? 'Good morning' : today.getHours() < 17 ? 'Good afternoon' : 'Good evening'}
  {user?.display_name ? `, ${user.display_name}` : ''}
</h1>
```

## Verification Results

### Automated Tests
I ran the existing Playwright tests for authentication, which cover the dashboard redirection and user state, and they passed successfully.

```bash
npx playwright test e2e/auth.spec.ts
```

Output:
```
[chromium] › e2e/auth.spec.ts:29:9 › Authentication Flow › should login successfully and redirect to dashboard
[chromium] › e2e/auth.spec.ts:23:9 › Authentication Flow › should redirect to login if not authenticated
2 passed (3.7s)
```

## Deployment Note
The user's display name is fetched from the `/auth/me` endpoint. Ensure the backend is running for the name to appear.
