# Agent Maven Patches

## 2026-07-07

### Dashboard Basic Authentication Fix

Hermes Version:
v0.18.0

Problem:

When BasicAuthProvider is the only configured authentication provider,
the dashboard incorrectly redirects users into the OAuth login flow.

This results in:

/auth/login?provider=basic

Since BasicAuthProvider does not implement OAuth,
Hermes throws a NotImplementedError which returns an HTTP 500.

Changes:

### middleware.py

Skip OAuth redirects for password-only providers.

Added:

if getattr(provider, "supports_password", False):
    return None

### routes.py

Catch NotImplementedError and return HTTP 400 instead of HTTP 500.

Reason:

Allows the standard Basic Authentication login page to render correctly.

Status:

Applied to Agent Maven fork.

Upstream:

Waiting for official Hermes fix.
