# Task: Debug Login Page Loading Issue

## Plan
- [x] Navigate to `http://localhost:3000/login`
- [x] Observe page state (screenshot/DOM)
- [x] Check console logs for errors
- [x] Report findings

## Findings
- Page eventually loads after a delay (stuck on "Loading..." for several seconds).
- Console logs show multiple **500 Internal Server Errors** for:
  - `http://localhost:3000/api/v1/settings`
  - `http://localhost:3000/api/v1/auth/status`
- Error message in console: `Failed to fetch timer durations Error: API error 500: Internal Server Error`.
- The "1 Issue" indicator in the bottom left corresponds to these errors.
- The failure of these API calls might be causing the prolonged "Loading..." state.
