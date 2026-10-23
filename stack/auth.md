# Tech stack: auth

For apps that log people in. Machine-to-machine auth (API tokens, device keys) is not covered here.

## Who owns login, in order

1. **Delegated**: an existing SSO/OIDC provider or reverse-proxy auth (Authelia, Authentik, Pocket ID, Keycloak). The app checks the token or the proxy header; passkeys are the provider's job.
2. **Multi-user** → **Better Auth** with the `@better-auth/passkey` plugin. Keep it current: it has had security fixes.
3. **Single admin** → own login: argon2id, session cookie (`HttpOnly`, `Secure`, `SameSite=Lax`), CSRF token, plus passkeys.

## Passkeys (WebAuthn)

- Only in a secure context: HTTPS or `localhost`. The RP ID is a domain, never an IP. Device on a LAN IP (ESP32, router) → no passkeys: delegate, or password + argon2id.
- The RP ID is bound to the domain: moving domains invalidates every passkey. Fix the domain before the first registration.
- Every account has a recovery path: a second passkey, recovery codes, or a password. Single admin: a CLI command that resets the login (`<app> reset-admin`), needing shell access.
- Libraries: **SimpleWebAuthn** (TS), **go-webauthn/webauthn** (Go), **webauthn-rs** (Rust). Store the credential ID, public key, sign counter and transports; keep the challenge in the server session, one use.
- Own flow = four endpoints: registration options/verify, authentication options/verify.
- Tests: Playwright virtual authenticator (CDP `WebAuthn.addVirtualAuthenticator`, Chromium) covers the full flow in CI.
- Tauri on Linux (WebKitGTK) and extension origins don't support WebAuthn reliably; they rarely need their own login.

## Passwords

- argon2id with the library's recommended parameters; compare in constant time (the library does).
- Rate-limit login per account and per IP. Same error for unknown user and wrong password.
