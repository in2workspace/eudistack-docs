# Verifier API

API para iniciar verificaciones de credenciales y validar presentaciones.

??? info "Especificación OpenAPI"
    La especificación OpenAPI del Verifier se genera automáticamente desde el servicio backend (`springdoc-openapi`).

    - [Verifier API (OpenAPI spec)](https://sandbox-stg.eudistack.net/verifier/v3/api-docs) *(requiere el entorno sandbox activo)*.

---

## Endpoints

=== "Presentación OID4VP"

    | Método | Path | Descripción |
    |--------|------|-------------|
    | `POST` | `/api/v1/authorization-request` | Crea una nueva sesión de presentación (devuelve `session_id` + URI/QR). |
    | `GET`  | `/oid4vp/auth-request/{id}` | El Wallet recupera el JWT de solicitud de presentación por ID de sesión. |
    | `POST` | `/oid4vp/auth-response` | Donde el Wallet envía el VP Token para verificación. |

=== "SSE (Portal / eventos)"

    | Método | Path | Descripción |
    |--------|------|-------------|
    | `GET` | `/api/login/events` | Conexión SSE: el Portal se suscribe para recibir el resultado de la presentación (`?state=<session_state>`). |

=== "OIDC / Discovery"

    | Método | Path | Descripción |
    |--------|------|-------------|
    | `GET` | `/.well-known/openid-configuration` | Discovery OIDC del Verifier. |
    | `GET` | `/oauth2/jwks` | Claves públicas del Verifier para validación criptográfica. |

---

## Modos de respuesta

=== "direct_post"
    El wallet hace `POST` directo al Verifier con la presentación VP Token.

=== "direct_post.jwt"
    Respuesta cifrada con la clave pública del Verifier. Ofrece mayor privacidad en el canal.

---

??? info "Notas técnicas"
    - **Protocolo:** OpenID4VP (OID4VP).
    - **Compatible con:** OAuth2 / OIDC.
    - **Formatos soportados:** JWT VP · SD-JWT VC (`dc+sd-jwt`).
    - **Criptografía:** ES256 (ECDSA P-256).
