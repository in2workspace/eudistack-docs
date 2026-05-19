# Verifier API

API for initiating credential verification and validating presentations.

??? info "OpenAPI Specification"
    The Verifier OpenAPI specification is automatically generated from the backend service (`springdoc-openapi`).

    - [Verifier API (OpenAPI JSON)](https://sandbox-stg.eudistack.net/verifier/v3/api-docs) *(requires the sandbox environment to be active)*.

---

## Endpoints

=== "OID4VP Presentation"

    | Method | Path | Description |
    |--------|------|-------------|
    | `POST` | `/api/v1/authorization-request` | Creates a new presentation session (returns `session_id` + URI/QR). |
    | `GET`  | `/oid4vp/auth-request/{id}` | The Wallet retrieves the presentation request JWT by session ID. |
    | `POST` | `/oid4vp/auth-response` | Endpoint where the Wallet sends the VP Token for verification. |

=== "SSE (Portal / events)"

    | Method | Path | Description |
    |--------|------|-------------|
    | `GET` | `/api/login/events` | SSE connection: the Portal subscribes to receive the presentation result (`?state=<session_state>`). |

=== "OIDC / Discovery"

    | Method | Path | Description |
    |--------|------|-------------|
    | `GET` | `/.well-known/openid-configuration` | OIDC Discovery for the Verifier. |
    | `GET` | `/oauth2/jwks` | Verifier public keys for cryptographic validation. |

---

## Response modes

=== "direct_post"
    The wallet sends the VP Token presentation directly via `POST` to the Verifier.

=== "direct_post.jwt"
    Response encrypted using the Verifier's public key. Provides stronger privacy on the channel.

---

??? info "Technical notes"
    - **Protocol:** OpenID4VP (OID4VP).
    - **Compatible with:** OAuth2 / OIDC.
    - **Supported formats:** JWT VP · SD-JWT VC (`dc+sd-jwt`).
    - **Cryptography:** ES256 (ECDSA P-256).
