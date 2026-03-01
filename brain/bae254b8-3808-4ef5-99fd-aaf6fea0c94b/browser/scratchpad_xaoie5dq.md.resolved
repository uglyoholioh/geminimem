# Debugging Login Issue Checklist

- [x] Open http://localhost:3000/auth/login
    - [!] FOUND BUILD ERROR: Module not found: Can't resolve '@/components/layout/Header'
- [ ] Attempt login with test@example.com / password123
- [ ] Check for session_token cookie
- [ ] Monitor network requests for /me or /settings (check for 401s)
- [ ] Navigate to Dashboard and verify access
- [ ] Report findings (cookie name, request status)

## Findings
- Login page is currently unreachable due to Next.js build error.
- Error location: `./components/ClientLayout.tsx:13:1`
- Missing module: `@/components/layout/Header`
