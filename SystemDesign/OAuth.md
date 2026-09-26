```js
React
  │
  │ 1. Login with Google
  ▼
Your Backend
https://api.myapp.com/auth/google
  │
  │ 2. Redirect browser
  ▼
Google
  │
  │ 3. Login + Consent
  │
  │ 4. Redirect browser to
  │    https://api.myapp.com/auth/google/callback?code=ABC
  ▼
Your Backend
https://api.myapp.com/auth/google/callback
  │
  │ 5. Code → Access Token
  │
  │ 6. Get Google user
  ▼
Your DB
  │
  │ 7. Find/Create user
  ▼
Your Backend
  │
  │ 8. Set session cookie
  │
  │ 9. Redirect browser
  ▼
React
https://myapp.com/dashboard
```