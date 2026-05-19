# Referencia API

Las APIs públicas de EUDIStack están organizadas por servicio. Cada una se expone vía OpenAPI 3.0 y dispone de un Swagger UI accesible cuando el entorno está activo (ver enlace en cada página de servicio).

<div class="grid cards" markdown>

-   :material-certificate-outline: [**Issuer API**](issuer.md)

    Emisión de credenciales (OID4VCI), gestión de ofertas, revocación.

-   :material-shield-check: [**Verifier API**](verifier.md)

    Solicitudes de presentación (OID4VP), validación de credenciales.

-   :material-wallet-outline: [**Wallet (EBW) API**](wallet-ebw.md)

    APIs server-side del European Business Wallet (gestión de claves, dispositivos, etc.).

</div>

## Convenciones comunes

??? info "Convenciones que aplican a todos los endpoints"

    - **Autenticación**: OAuth 2.0 client credentials. Tokens vinculados con DPoP.
    - **Tenant**: derivado del subdominio de la URL.
    - **Errores**: respuestas RFC 7807 (`application/problem+json`).
    - **Paginación**: cursor-based (`page_token` / `next_page_token`).
    - **Versionado**: en el path (`/v1/...`); cambios breaking incrementan la versión.
