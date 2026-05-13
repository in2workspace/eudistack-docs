# Verifier API

API para iniciar verificaciones de credenciales y validar presentaciones.

## OpenAPI Specification

La especificación OpenAPI del Verifier se genera automáticamente desde el servicio backend (`springdoc-openapi`).

- [Verifier API (Swagger UI)](https://sandbox-stg.eudistack.net/verifier/v3/api-docs).

## Endpoints principales (integración externa)

| Método | Path | Descripción                                                    |
|--------|---|----------------------------------------------------------------|
| POST   | `/oid4vp/auth-request/{id}` | Genera o recupera una solicitud de presentación (JWT para QR). |
| POST   | `/oid4vp/auth-response` | Donde el Wallet envía el VP Token para verificación.           |
| GET    | `/.well-known/openid-configuration` | Discovery OIDC del Verifier.                                   |
| GET    | `/oauth2/jwks` | Claves públicas del Verifier para validación criptográfica.    |

## Modos de respuesta

- **direct_post**: el wallet hace POST directo al Verifier con la presentación.
- **direct_post.jwt**: respuesta cifrada con la clave del Verifier.

## Notas técnicas
- Protocolo: OpenID4VP (OID4VP).
- OAuth2 / OIDC compatible.
- Formatos: 
  - JWT VP.
  - SD-JWT VC.
- Criptografía: ES256 (ECDSA P-256).