# Wallet (EBW) API

Server-side API of the European Business Wallet.

??? info "Operation modes"
    | Mode | Description |
    |---|---|
    | **Server (Business Wallet)** | Runs in backend; this API applies to this mode. |
    | **Browser (personal EUDIW)** | Runs 100% on the user's device; does not expose server-side APIs. |

??? info "OpenAPI Specification"
    The API is documented using OpenAPI 3.0, automatically generated:

    - [Wallet API (OpenAPI specification)](https://sandbox-stg.eudistack.net/wallet/v3/api-docs) *(requires the sandbox environment to be active)*.

---

## Endpoints

=== "Credential management"

    | Method | Path | Description | Auth |
    |--------|------|-------------|------|
    | `POST`   | `/api/v1/credentials` | Store a credential in the wallet | JWT + DPoP |
    | `GET`    | `/api/v1/credentials` | List user credentials | JWT + DPoP |
    | `GET`    | `/api/v1/credentials/{id}` | Retrieve a specific credential | JWT + DPoP |
    | `PATCH`  | `/api/v1/credentials/{id}/status` | Update credential status (`active` / `revoked`) | JWT + DPoP |
    | `DELETE` | `/api/v1/credentials/{id}` | Delete a credential | JWT + DPoP |

=== "OID4VCI issuance"

    | Method | Path | Description | Auth |
    |--------|------|-------------|------|
    | `POST` | `/api/v1/openid-credential-offer/credential-response` | Completes the OID4VCI flow (credential issuance exchange) | JWT + DPoP |

---

??? info "Technical notes"
    - **Authentication:** OAuth2 + JWT + DPoP binding.
    - **Storage:** credentials stored encrypted in a secure backend (HSM/KMS).
    - Only applies to *Business Wallet* (Server) mode; EUDIW (browser) mode does not expose server-side APIs.
