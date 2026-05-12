# Verifier API

API for initiating credential verification and validating presentations.

## OpenAPI Specification

The Verifier OpenAPI specification is automatically generated from the backend service (`springdoc-openapi`).

- [Verifier API (Swagger UI)](https://sandbox-stg.eudistack.net/verifier/v3/api-docs).

## Main Endpoints (External Integration)

| Method | Path | Description |
|--------|------|-------------|
| POST | `/oid4vp/auth-request/{id}` | Generates or retrieves a presentation request (JWT for QR). |
| POST | `/oid4vp/auth-response` | Endpoint where the Wallet sends the VP Token for verification. |
| GET | `/.well-known/openid-configuration` | OIDC Discovery for the Verifier. |
| GET | `/oauth2/jwks` | Verifier public keys for cryptographic validation. |

## Response Modes

- **direct_post**: the wallet sends the presentation directly via POST to the Verifier.
- **direct_post.jwt**: response encrypted using the Verifier’s public key.

## Technical Notes

- Protocol: OpenID4VP (OID4VP).
- OAuth2 / OIDC compatible.
- Formats:
    - JWT VP.
    - SD-JWT VC.
- Cryptography: ES256 (ECDSA P-256).