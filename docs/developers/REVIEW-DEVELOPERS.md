# Revisión sección Developer — Puntos de mejora

> Revisión realizada el 2026-05-18. Cubre los 18 ficheros bajo `docs/developers/`.

---

## Errores / Incorrecciones

### 1. `dpop-pkce.md` — Texto duplicado/mal ubicado

El párrafo introductorio de DPoP aparece en medio de la sección PKCE (líneas 21-22), partido en dos fragmentos distintos. El contenido aparece dos veces y en el lugar incorrecto, lo que hace la sección confusa.

### 2. `first-integration.md` — Numeración duplicada en Paso 2

El paso "Abrir la Credential Offer" tiene dos puntos numerados como `4.` (líneas 87 y 88). Uno de ellos debería ser el `5.`.

### 3. `first-integration.md` vs `api-reference/verifier.md` — Método HTTP contradictorio

El tutorial usa `GET /verifier/oid4vp/auth-request/{id}` para el polling de resultado, pero la referencia API declara ese mismo endpoint como `POST`. Uno de los dos está mal.

### 4.SOLVED `sd-jwt-vc.md` — Afirmación técnica incorrecta sobre JAdES

La página dice que "soporta firmas mediante JAdES". JAdES (ETSI EN 319 132) es un estándar de firma diferente de JWS/ES256. SD-JWT VC usa JWS estándar con ES256, no JAdES. Esta afirmación mezcla conceptos y puede inducir a error a los integradores.

### 5. `scim-provisioning.md` — Typo

Línea 163: `"Selecciona SCIM cmo modo de provisioning"` → debería ser `"como"`.

---

## Inconsistencias de URLs

### 6. Patrón de URL del Issuer inconsistente

- `oid4vci.md` y `dpop-pkce.md` (ejemplo DPoP) usan: `https://issuer.sandbox.eudistack.net`
- Todo el resto de la documentación usa: `https://sandbox-stg.eudistack.net/issuer`

El patrón canónico (consistente con `multi-tenant.md`) es `{tenant}-stg.eudistack.net`. La primera forma debería eliminarse.

### 7. Patrón de URL SCIM diferente del patrón multi-tenant

`scim-provisioning.md` declara `https://scim.<tenant>.eudistack.net/scim/v2` con un subdomain `scim.` por delante, que no encaja con el patrón `{tenant}-stg.eudistack.net` establecido en `multi-tenant.md`. Habría que aclarar cuál es el patrón real de producción/STG.

---

## Inconsistencias de paths de API

### 8. Tres paths distintos para "crear una oferta de credencial"

| Archivo | Path |
|---|---|
| `first-integration.md` | `POST /api/v1/issuances` |
| `api-direct-issuance.md` | `POST /credential-offer` |
| `api-reference/issuer.md` | `POST /oid4vci/v1/credential-offer` |

Si los tres son válidos (API de gestión vs. endpoint OID4VCI), debería explicarse la diferencia. Si no, hay que consolidar.

### 9. Path del token endpoint

`dpop-pkce.md` muestra `/oauth/token` pero `api-reference/issuer.md` declara el token endpoint en `/oid4vci/v1/token`. O son paths diferentes para propósitos distintos (habría que documentarlo) o uno está desactualizado.

---

## Contenido faltante / TODOs pendientes

### 10. TODOs que señalan contenido incompleto

Los siguientes archivos tienen placeholders visibles que no deberían estar en una documentación publicada:

- `getting-started/index.md` — `<!-- TODO: completar con datos reales del sandbox -->`
- `getting-started/sandbox.md` — `<!-- TODO: rellenar URLs reales del sandbox y proceso de alta -->`
- `getting-started/first-integration.md` — `<!-- TODO: completar con ejemplos curl reales -->`
- `api-reference/index.md` — TODO sobre render OpenAPI inline
- `api-reference/issuer.md` — TODO enlace Swagger funcional
- `api-reference/wallet-ebw.md` — TODO enlace Swagger funcional
- `api-direct-issuance.md` — TODO curl ejemplos + webhooks
- `troubleshooting.md` — `<!-- TODO: ampliar con incidencias reales -->`

### 11. Endpoint SSE no documentado en la referencia API

`oidc-idp.md` menciona explícitamente `GET /api/login/events?state=...` como la conexión SSE que usa el Portal, pero este endpoint no aparece en `api-reference/verifier.md`.

### 12. `/.well-known/jwt-issuer` no está en la tabla de well-known del Issuer

`troubleshooting.md` referencia este endpoint como causa de fallos de validación del wallet, pero no está listado en la tabla de well-known endpoints de `api-reference/issuer.md`.

### 13.SOLVED `oid4vp.md` — Tabs sin contenido

Las pestañas "Cross-device (QR)" y "Direct Post" tienen título pero el cuerpo está vacío. No aportan información al lector.

### 14. `api-direct-issuance.md` — Guía básicamente incompleta

Es la guía más usada en automatizaciones backend y apenas tiene contenido. Le faltan: ejemplo de autenticación M2M, ejemplo de llamada completa, modelo de webhooks y el campo `client_request_id` mencionado no aparece en ningún otro ejemplo de la documentación.

---

## Issues menores / estilo

### 15.SOLVED Identificadores de formato de credencial inconsistentes

La documentación mezcla tres formas diferentes para referirse al mismo formato:

| Forma usada | Aparece en |
|---|---|
| `SD-JWT VC` | `oid4vci.md` intro, `oid4vp.md` intro |
| `dc+sd-jwt` | `oid4vci.md` más abajo, `oid4vp.md` ejemplo JSON |
| `jwt_vc_json` | `oid4vci.md` intro, `oid4vp.md` intro |

El identificador normativo es `dc+sd-jwt` (el que va en los metadatos). Deberían usarse de forma coherente.

### 16. `credential_configuration_id` inconsistente entre páginas

| Archivo | Valor usado |
|---|---|
| `first-integration.md` | `learcredential.employee.sd.1` |
| `oid4vci.md` | `learcredential.employee.w3c.4` |
| `oid4vp.md` | `eu.europa.ec.eudi.lce.1` |

Si son formatos o versiones diferentes, debería explicarse la diferencia; si no, habría que consolidar.

### 17. `first-integration.md` — Opción B del Paso 5 sin etiqueta

"Opción B:" no tiene nombre. Debería ser "Opción B: Polling" o similar.

### 18.SOLVED `oid4vp.md` — Referencia DCQL apunta al spec OID4VP, no al spec DCQL

El link `[Digital Credential Query Language (DCQL)]` apunta a un fragmento de la spec OID4VP. DCQL tiene su propia especificación: `openid-4-verifiable-credential-query-language-1_0`.

### 19. `api-reference/verifier.md` — Link Swagger apunta a URL live del sandbox

El link al Swagger UI apunta directamente a `https://sandbox-stg.eudistack.net/verifier/v3/api-docs`. Si el entorno STG no está levantado, el link está roto. Sería preferible una URL estable o añadir una nota de que requiere el entorno activo.

### 20.SOLVED `oidc-idp.md` — Valor `acr: "0"` en el id_token de ejemplo

El claim `acr` en el ejemplo tiene valor `"0"`, que en el contexto OIDC/eIDAS suele indicar "sin nivel de confianza". Debería documentarse qué valores ACR emite EUDIStack y qué significan, especialmente porque eIDAS 2.0 usa LoA (substantial/high).

---

## Resumen de prioridades

| Prioridad | Issues |
|---|---|
| Alta — incorrecciones técnicas | #1, #2, #3, #4, #5 |
| Media — inconsistencias que confunden al integrador | #6, #7, #8, #9, #15, #16 |
| Media — contenido incompleto visible | #10, #13, #14 |
| Baja — mejoras de claridad/completitud | #11, #12, #17, #18, #19, #20 |