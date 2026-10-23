# Tech stack: auth

For apps that log people in. Machine-to-machine auth (API tokens, device keys) is not covered here.

## Who owns login, in order

1. **Delegated**: an existing SSO/OIDC provider or reverse-proxy auth (Authelia, Authentik, Pocket ID, Keycloak). The app validates the token (issuer, audience, expiry, signature), or trusts proxy identity headers only when the proxy strips client copies and the backend is unreachable except through it. Passkeys are the provider's job.
2. **Multi-user** → **Better Auth** with the `@better-auth/passkey` plugin. Keep it current: it has had security fixes.
3. **Single admin** → own login: argon2id, session cookie (`HttpOnly`, `Secure`, `SameSite=Lax`), CSRF token, plus passkeys. New session ID on login and privilege change; the old one is invalidated.

## Passkeys (WebAuthn)

- Only in a secure context: HTTPS or `localhost`. The RP ID is a domain, never an IP. Device on a LAN IP (ESP32, router) → no passkeys: delegate, or password + argon2id.
- Changing the RP ID invalidates every passkey. Set it to the stable registrable domain (`example.com` also covers `app.example.com`) before the first registration.
- Every account has a recovery path: a second passkey, recovery codes, or a password. Single admin: a CLI command that resets the login (`<app> reset-admin`), needing shell access.
- Libraries: **SimpleWebAuthn** (TS), **go-webauthn/webauthn** (Go), **webauthn-rs** (Rust). Store the credential ID, public key, sign counter and transports; keep the challenge in the server session, one use.
- Own flow = four endpoints: registration options/verify, authentication options/verify. Every verify call gets the expected origin, RP ID and the session's challenge; the library checks them.
- Tests: Playwright virtual authenticator (CDP `WebAuthn.addVirtualAuthenticator`, Chromium) covers the full flow in CI.
- Tauri on Linux (WebKitGTK) lacks reliable WebAuthn; extension support differs per browser, test each target. Both rarely need their own login.

## Passwords

- Own login: argon2id with the library's recommended parameters; compare in constant time (the library does). Better Auth keeps its default hashing.
- Rate-limit login per account and per IP. Unknown user and wrong password: same error, same timing (verify against a dummy hash).
