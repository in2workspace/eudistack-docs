# Issuer API

API for issuing verifiable credentials and managing their lifecycle using OpenID4VCI (OIDC for Verifiable Credential Issuance).

## OpenAPI Specification

The Issuer OpenAPI specification is automatically generated from the backend service (`springdoc-openapi`).

- [Issuer API (Swagger UI)](https://sandbox-stg.eudistack.net/issuer/springdoc/swagger-ui.html).

## Main Endpoints

### OID4VCI

| Method | Path | Description |
|--------|------|-------------|
| POST | `/oid4vci/v1/credential-offer` | Creates a credential issuance offer for the Wallet. |
| POST | `/oid4vci/v1/credential` | Endpoint where the Wallet requests credential issuance. |
| POST | `/oid4vci/v1/deferred-credential` | Retrieves credentials issued in a deferred manner. |
| POST | `/oid4vci/v1/notification` | Notifications for the issuance flow (status, events). |
| POST | `/oid4vci/v1/token` | OAuth2 endpoint for the OID4VCI flow. |

### Credential Revocation and Status

| Method | Path | Description |
|--------|------|-------------|
| GET | `/w3c/v1/credentials/status/{listId}` | Queries credential status (W3C BitString Status List). |
| POST | `/w3c/v1/credentials/status/revoke` | Revokes an issued credential. |
| GET | `/token/v1/credentials/status/{listId}` | Credential status based on OAuth Token Status List. |
| POST | `/token/v1/credentials/status/revoke` | Revocation via token status list. |

### Well-known Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/.well-known/openid-credential-issuer` | Issuer metadata for OID4VCI. |
| GET | `/.well-known/openid-configuration` | OAuth2 / OIDC configuration. |
| GET | `/.well-known/oauth-authorization-server` | Authorization Server metadata. |
| GET | `/.well-known/jwks.json` | Public keys (JWKS) for cryptographic verification. |

## Technical Notes

- Protocol: OpenID4VCI (OIDC for Verifiable Credential Issuance)
- Profile: eIDAS 2.0 / DOME compatible
- Formats:
    - JWT VC
    - SD-JWT VC
- Cryptography: ES256 (ECDSA P-256)
- Backend: Spring WebFlux (reactive)