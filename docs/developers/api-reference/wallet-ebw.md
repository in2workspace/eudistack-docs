# Wallet (EBW) API

API server-side del European Business Wallet. 
Solo aplica al modo *Server* (Business Wallet); el modo *Browser* (EUDIW personal) opera 100% en el dispositivo del usuario.

## OpenAPI Specification

La API está documentada mediante OpenAPI 3.0 generado automáticamente: 

<!-- TODO: add a link to a functional Swagger documentation  -->
- Wallet API (Swagger UI) 


## Endpoints principales

| Método | Path | Descripción | Auth |
|--------|------|-------------|------|
| POST | `/api/v1/credentials` | Almacenar credencial en el wallet | JWT + DPoP |
| GET | `/api/v1/credentials` | Listar credenciales del usuario | JWT + DPoP |
| GET | `/api/v1/credentials/{id}` | Obtener credencial concreta | JWT + DPoP |
| PATCH | `/api/v1/credentials/{id}/status` | Actualizar estado (active/revoked) | JWT + DPoP |
| DELETE | `/api/v1/credentials/{id}` | Eliminar credencial | JWT + DPoP |
| POST | `/api/v1/openid-credential-offer/credential-response` | Finaliza el flujo OID4VCI (emisión de credencial) | JWT + DPoP |

## Notas técnicas

- Autenticación: OAuth2 + JWT + DPoP binding
- Credenciales almacenadas cifradas en backend seguro (HSM/KMS)
- El Wallet Server actúa solo en modo *Business Wallet*
- El modo EUDIW (browser) no expone APIs server-side
