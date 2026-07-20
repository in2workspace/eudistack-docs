# Wallet (EBW) API

API server-side del European Business Wallet.

??? info "Modos de operación"
    | Modo | Descripción |
    |---|---|
    | **Server (Business Wallet)** | Opera en backend; esta API aplica a este modo. |
    | **Browser (EUDIW personal)** | Opera 100% en el dispositivo del usuario; no expone APIs server-side. |

??? info "Especificación OpenAPI"
    La API está documentada mediante OpenAPI 3.0 generado automáticamente:

    - [Wallet API (OpenAPI specification)](https://sandbox.stg.eudistack.net/wallet/v3/api-docs) *(requiere el entorno sandbox activo)*.

---

## Endpoints

=== "Gestión de credenciales"

    | Método | Path | Descripción | Auth |
    |--------|------|-------------|------|
    | `POST`   | `/api/v1/credentials` | Almacenar credencial en el wallet | JWT + DPoP |
    | `GET`    | `/api/v1/credentials` | Listar credenciales del usuario | JWT + DPoP |
    | `GET`    | `/api/v1/credentials/{id}` | Obtener credencial concreta | JWT + DPoP |
    | `PATCH`  | `/api/v1/credentials/{id}/status` | Actualizar estado (`active` / `revoked`) | JWT + DPoP |
    | `DELETE` | `/api/v1/credentials/{id}` | Eliminar credencial | JWT + DPoP |

=== "Emisión OID4VCI"

    | Método | Path | Descripción | Auth |
    |--------|------|-------------|------|
    | `POST` | `/api/v1/openid-credential-offer/credential-response` | Finaliza el flujo OID4VCI (canje de credencial) | JWT + DPoP |

---

??? info "Notas técnicas"
    - **Autenticación:** OAuth2 + JWT + DPoP binding.
    - **Almacenamiento:** credenciales cifradas en backend seguro (HSM/KMS).
    - Solo aplica al modo *Business Wallet* (Server); el modo EUDIW (browser) no expone APIs server-side.
