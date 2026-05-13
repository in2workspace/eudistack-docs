# Wallet (EBW) API

Server-side API of the European Business Wallet.  
It only applies to *Server* mode (Business Wallet); *Browser* mode (personal EUDIW) operates 100% on the user's device.

## OpenAPI Specification

The API is documented using OpenAPI 3.0, automatically generated:  

- [Wallet API (Swagger UI)](https://sandbox-stg.eudistack.net/wallet/springdoc/swagger-ui.html).

## Main Endpoints

| Method | Path | Description | Auth |
|--------|------|-------------|------|
| POST | `/api/v1/credentials` | Store a credential in the wallet | JWT + DPoP |
| GET | `/api/v1/credentials` | List user credentials | JWT + DPoP |
| GET | `/api/v1/credentials/{id}` | Retrieve a specific credential | JWT + DPoP |
| PATCH | `/api/v1/credentials/{id}/status` | Update credential status (active/revoked) | JWT + DPoP |
| DELETE | `/api/v1/credentials/{id}` | Delete a credential | JWT + DPoP |
| POST | `/api/v1/openid-credential-offer/credential-response` | Completes the OID4VCI flow (credential issuance) | JWT + DPoP |

## Technical Notes

- Authentication: OAuth2 + JWT + DPoP binding
- Credentials are stored encrypted in a secure backend (HSM/KMS)
- The Wallet Server only operates in *Business Wallet* mode
- EUDIW (browser) mode does not expose server-side APIs