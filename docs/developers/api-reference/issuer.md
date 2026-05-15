# Issuer API

API para emitir credenciales verificables y gestionar su ciclo de vida mediante OpenID4VCI (OIDC for Verifiable Credential Issuance).

## OpenAPI Specification

La especificación OpenAPI del Issuer se genera automáticamente desde el servicio backend (`springdoc-openapi`).

<!-- TODO: add a link to a functional Swagger documentation  -->
- Issuer API (Swagger UI)

## Endpoints principales 

### OID4VCI

| Método | Path | Descripción |
|--------|---|---|
| POST | `/oid4vci/v1/credential-offer` | Crea una oferta de emisión de credencial para el Wallet. |
| POST | `/oid4vci/v1/credential` | Endpoint donde el Wallet solicita la emisión de la credencial. |
| POST | `/oid4vci/v1/deferred-credential` | Recupera credenciales emitidas de forma diferida. |
| POST | `/oid4vci/v1/notification` | Notificaciones del flujo de emisión (estado, eventos). |
| POST | `/oid4vci/v1/token` | Endpoint OAuth2 para el flujo OID4VCI. |

### Revocación y estado de credenciales

| Método | Path | Descripción |
|--------|---|---|
| GET | `/w3c/v1/credentials/status/{listId}` | Consulta estado de credenciales (W3C BitString Status List). |
| POST | `/w3c/v1/credentials/status/revoke` | Revoca una credencial emitida. |
| GET | `/token/v1/credentials/status/{listId}` | Estado de credenciales basado en OAuth Token Status List. |
| POST | `/token/v1/credentials/status/revoke` | Revocación vía token status list. |

### Well-known endpoints

| Método | Path | Descripción |
|--------|---|---|
| GET | `/.well-known/openid-credential-issuer` | Metadata del Issuer para OID4VCI. |
| GET | `/.well-known/openid-configuration` | Configuración OAuth2 / OIDC. |
| GET | `/.well-known/oauth-authorization-server` | Metadata del Authorization Server. |
| GET | `/.well-known/jwks.json` | Claves públicas (JWKS) para validación criptográfica. |

## Notas técnicas

- Protocolo: OpenID4VCI (OIDC for Verifiable Credential Issuance)
- Perfil: eIDAS 2.0 / DOME compatible
- Formatos:
    - JWT VC
    - SD-JWT VC
- Criptografía: ES256 (ECDSA P-256)
- Backend: Spring WebFlux (reactivo)