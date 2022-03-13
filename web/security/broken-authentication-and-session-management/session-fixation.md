---
description: >-
  Session fixation happens when the application uses the same session token
  before and after the user authenticates.
---

# Session Fixation

Session Fixation is a type of application vulnerability where an application does not correctly renew session tokens when changing from a pre-login to post-login state. The same pre-login session token should not be used post-login, otherwise an attacker has the potential to steal authenticated sessions of legitimate users. When a session of one user is stolen by another, it is known as a "hijacked session".

### Fix

A combination of the following best practices could help to defend against Session Fixation attacks:

1. Ensure that only server-generated session values are accepted by the application.
2. Upon successful login, invalidate the original session token, and re-issue a new session token.
3. Prevent the application from accepting session tokens via GET or POST requests and instead store session values within HTTP cookies only.

### Code Fix



