# Issuer API

API para emitir credenciales verificables y gestionar su ciclo de vida mediante OpenID4VCI (OIDC for Verifiable Credential Issuance).

## OpenAPI Specification

La especificación OpenAPI del Issuer se genera automáticamente desde el servicio backend (`springdoc-openapi`).

- [Issuer API (OpenAPI specification)](https://sandbox.stg.eudistack.net/issuer/v3/api-docs) *(requiere el entorno sandbox activo)*.

## Endpoints principales

=== "Management API"

    Endpoints destinados a sistemas backend para disparar el ciclo de vida de una credencial. Requieren autenticación OAuth 2.0 con `access_token` de tipo client credentials.

    > **Nota:** Ésta es la API que usan integraciones externas (SCIM, webhooks, portales internos). No forma parte del protocolo OID4VCI estándar; es una capa de gestión propia de EUDIStack.

    | Método | Path | Descripción |
    |--------|------|-------------|
    | POST | `/api/v1/issuances` | Crea una nueva oferta de credencial y dispara el flujo de emisión. Devuelve `credential_offer_uri`. |
    | GET | `/api/v1/issuances/{id}` | Consulta el estado de una emisión por su `issuance_id`. |

=== "OID4VCI"

    Endpoints del protocolo OID4VCI estándar. Son invocados por el wallet durante el flujo de emisión.

    | Método | Path | Descripción |
    |--------|------|-------------|
    | POST | `/oid4vci/v1/credential-offer` | Crea una oferta de emisión de credencial para el Wallet. |
    | POST | `/oid4vci/v1/credential` | Endpoint donde el Wallet solicita la emisión de la credencial. |
    | POST | `/oid4vci/v1/deferred-credential` | Recupera credenciales emitidas de forma diferida. |
    | POST | `/oid4vci/v1/notification` | Notificaciones del flujo de emisión (estado, eventos). |
    | POST | `/oid4vci/v1/token` | Endpoint OAuth2 para el flujo OID4VCI. |

=== "Estado y Revocación"

    | Método | Path | Descripción |
    |--------|------|-------------|
    | GET | `/w3c/v1/credentials/status/{listId}` | Consulta estado de credenciales (W3C BitString Status List). |
    | POST | `/w3c/v1/credentials/status/revoke` | Revoca una credencial emitida. |
    | GET | `/token/v1/credentials/status/{listId}` | Estado de credenciales basado en OAuth Token Status List. |
    | POST | `/token/v1/credentials/status/revoke` | Revocación vía token status list. |

=== "Well-known"

    | Método | Path | Descripción |
    |--------|------|-------------|
    | GET | `/.well-known/openid-credential-issuer` | Metadata del Issuer para OID4VCI. |
    | GET | `/.well-known/openid-configuration` | Configuración OAuth2 / OIDC. |
    | GET | `/.well-known/oauth-authorization-server` | Metadata del Authorization Server. |
    | GET | `/.well-known/jwks.json` | Claves públicas (JWKS) para validación criptográfica. |
    | GET | `/.well-known/jwt-issuer` | Metadata del JWT Issuer; requerido por wallets para validar la firma del `vct`. |

??? info "Notas técnicas"

    - Protocolo: OpenID4VCI (OIDC for Verifiable Credential Issuance)
    - Perfil: eIDAS 2.0 / DOME compatible
    - Formatos: JWT VC · SD-JWT VC (`dc+sd-jwt`)
    - Criptografía: ES256 (ECDSA P-256)
    - Backend: Spring WebFlux (reactivo)
