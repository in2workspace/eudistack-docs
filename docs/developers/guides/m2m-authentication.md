# Autenticación M2M

Permite que un sistema backend obtenga un `access_token` del Verifier sin intervención de un usuario, presentando una **credencial de máquina** en lugar de un secreto compartido.

!!! note "No confundir con el client credentials del Issuer"
    La guía [API Direct Issuance](api-direct-issuance.md) usa `client_id` + `client_secret` contra el token endpoint del **Issuer**. Esta guía cubre el token endpoint del **Verifier**, que no usa `client_secret`: la identidad del servicio se prueba con una credencial verificable.

??? tip "¿Cuándo usar esta guía?"
    - Un servicio tuyo necesita llamar a APIs protegidas por EUDIStack sin usuario delante.
    - Quieres que la identidad de ese servicio sea una credencial verificable y revocable, no un secreto compartido.
    - Ya dispones de una credencial de máquina emitida a nombre del servicio.

??? info "Requisitos previos"
    - Una credencial de máquina (`LEARCredentialMachine`) emitida por un issuer que el Verifier reconozca como confiable.
    - La clave privada asociada al sujeto de la credencial, para firmar el `client_assertion`.
    - El tenant contra el que vas a operar.

---

## Endpoint

```txt
POST https://<tenant>.eudistack.net/verifier/oidc/token
```

En sandbox: `https://sandbox.stg.eudistack.net/verifier/oidc/token`.

---

## El flujo

1. Construyes una Verifiable Presentation (VP) con tu credencial de máquina y la codificas en Base64.
2. Construyes y firmas un `client_assertion` (JWT) que transporta esa VP.
3. Envías ambos al token endpoint con `grant_type=client_credentials`.
4. El Verifier valida la credencial (firma, issuer confiable, vigencia, revocación) y devuelve un `access_token`.

Los ejemplos siguientes usan el tenant sandbox y una máquina cuyo DID es
`did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy`. Sustituye cada valor por el tuyo.

=== "Paso 1: el vp_token"

    Envuelve tu credencial de máquina (una cadena JWT VC) en un objeto VP. Si la credencial está almacenada en Base64, decodifícala primero.

    ```json title="Objeto VP"
    {
      "@context": ["https://www.w3.org/ns/credentials/v2"],
      "type": ["VerifiablePresentation"],
      "verifiableCredential": ["eyJhbGci...ssw5c"]
    }
    ```

    Firma ese objeto VP como un JWT con la clave privada de la máquina. Usa `NumericDate` (segundos) en los claims de tiempo y mantén `exp` corto:

    ```json title="Payload del VP JWT"
    {
      "iss": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "sub": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "aud": "https://sandbox.stg.eudistack.net/verifier",
      "jti": "urn:uuid:3978344f-8596-4c3a-a978-8fcaba3903c5",
      "iat": 1731570000,
      "nbf": 1731570000,
      "exp": 1731570010,
      "vp": {
        "@context": ["https://www.w3.org/ns/credentials/v2"],
        "type": ["VerifiablePresentation"],
        "verifiableCredential": ["eyJhbGci...ssw5c"]
      }
    }
    ```

    Codifica en Base64 estándar la cadena del VP JWT resultante. Ese valor codificado es el `vp_token`:

    ```txt
    vp_token = base64( <VP en formato JWT> )
    ```

=== "Paso 2: el client_assertion"

    Un JWT firmado con la clave de tu servicio (cabecera `alg = ES256` o `RS256`, `kid` = la clave DID de la máquina). Claims obligatorios:

    | Claim | Valor |
    |---|---|
    | `iss` | tu `client_id` (el DID de la máquina) |
    | `sub` | tu `client_id` (idéntico a `iss`) |
    | `aud` | URL pública del Verifier: `https://<tenant>.eudistack.net/verifier` |
    | `jti` | identificador único, **de un solo uso** |
    | `iat` | hora actual (segundos) |
    | `exp` | expiración corta (p. ej. `iat + 10` segundos) |
    | `vp_token` | la VP codificada del paso 1 |

    ```json title="Payload del client_assertion"
    {
      "iss": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "sub": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "aud": "https://sandbox.stg.eudistack.net/verifier",
      "jti": "urn:uuid:9b7e2c14-1f8a-49c0-9b0e-6d1f7c2a5e4b",
      "iat": 1731570000,
      "exp": 1731570010,
      "vp_token": "eyJhbGci...<base64 del VP JWT>"
    }
    ```

=== "Paso 3: la petición"

    ```http
    POST /verifier/oidc/token HTTP/1.1
    Host: sandbox.stg.eudistack.net
    Content-Type: application/x-www-form-urlencoded

    grant_type=client_credentials&
    client_id=did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy&
    client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer&
    client_assertion=eyJhbGci...
    ```

    `client_id` debe viajar siempre en el cuerpo del formulario, también si tu cliente está pre-registrado.

=== "Paso 4: la respuesta"

    ```json
    {
      "access_token": "eyJhbGci...",
      "token_type": "Bearer",
      "expires_in": 900
    }
    ```

    La respuesta incluye `Cache-Control: no-store`. Este grant **no devuelve `id_token` ni `refresh_token`**: no hay usuario autenticado ni sesión que renovar. Cuando el token expire, repite el flujo con un `jti` nuevo.

    ??? example "access_token decodificado"

        ```json title="Cabecera"
        {
          "kid": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
          "typ": "JWT",
          "alg": "ES256"
        }
        ```

        ```json title="Payload"
        {
          "iss": "https://sandbox.stg.eudistack.net/verifier",
          "aud": "https://sandbox.stg.eudistack.net/verifier",
          "sub": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
          "iat": 1731570000,
          "exp": 1731573600,
          "jti": "1700c742-5457-4c02-8e6f-020a94054519",
          "client_id": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
          "scope": "machine learcredential",
          "vc": {
            "@context": ["https://www.w3.org/ns/credentials/v2"],
            "type": ["VerifiableCredential", "LEARCredentialMachine"],
            "issuer": { "id": "did:elsi:VATES-A12345678" },
            "credentialSubject": {
              "mandate": {
                "mandator": {
                  "organizationIdentifier": "VATES-B12345678",
                  "organization": "GOOD AIR, S.L.",
                  "country": "ES"
                },
                "mandatee": {
                  "id": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
                  "domain": "dpas.goodair.example",
                  "ipAddress": "195.70.63.244"
                },
                "power": [{
                  "type": "domain",
                  "domain": "DOME",
                  "function": "Onboarding",
                  "action": ["Execute"]
                }]
              }
            },
            "validFrom": "2025-09-15T06:11:19Z",
            "validUntil": "2026-09-15T06:11:19Z"
          }
        }
        ```

---

## Credenciales elegibles

Solo las credenciales de **máquina** pueden usar este grant. Las de empleado y el resto de tipos sirven únicamente para login de usuario con `authorization_code`.

| Tipo de credencial | M2M |
|---|---|
| `learcredential.machine.w3c.2` · `learcredential.machine.w3c.3` | Sí |
| `learcredential.machine.sd.1` | Sí |
| `learcredential.employee.*` | No |
| Resto de tipos | No |

> **Versiones legacy:** los formatos DOME antiguos pueden estar deshabilitados por configuración del tenant. Si tu credencial usa un formato retirado, el endpoint responde `410` aunque el tipo sea de máquina.

---

## client_id y tenant

No necesitas registrar tu servicio previamente en el Verifier. Si el `client_id` no está registrado, el Verifier deriva la identidad y el tenant de la propia credencial presentada.

Para que eso funcione, la credencial debe autorizar explícitamente el tenant al que llamas: al menos uno de los `mandate.power[].domain` de la credencial debe coincidir con el tenant del subdominio de la URL. Si la credencial no declara ningún dominio, o ninguno coincide, la petición se rechaza.

---

## Solución de problemas

Las respuestas de error del token endpoint son intencionadamente genéricas y no incluyen `error_description`: un mismo código cubre varias causas, para no revelar detalles sobre la credencial presentada. El motivo exacto queda registrado en el log de auditoría de tu tenant; pídelo a soporte si necesitas confirmarlo.

| Error | Causas posibles | Qué comprobar, por orden |
|---|---|---|
| `invalid_client` | La credencial no es de máquina · `client_assertion` mal formado o con claims incorrectos · `jti` ya utilizado · issuer no confiable · VP mal formada · el tenant no está autorizado por la credencial | Tipo de credencial → `iss`/`sub`/`aud`/`jti`/`exp` → codificación Base64 del `vp_token` → `power[].domain` frente al subdominio |
| `invalid_grant` | Credencial expirada, todavía no vigente, o revocada | Fechas de validez y estado de revocación de la credencial |
| `server_error` | El Verifier no pudo consultar el registro de issuers confiables o la Status List | Reintentar con backoff exponencial; si persiste, contactar con soporte |
| `401` sin cuerpo | Falta `client_id` o `client_assertion` en el formulario | Ambos parámetros son obligatorios |
| `410` · `503` | El formato de esa credencial está deshabilitado en el tenant | Emitir la credencial en un formato vigente |
| `429` | Límite de peticiones por minuto superado | Reducir la frecuencia y respetar la cabecera `Retry-After` |

??? bug "El token endpoint devuelve `invalid_grant`"

    - **Causa probable**: la credencial de máquina dentro de la VP está caducada (superó su `validUntil`), todavía no es válida (`validFrom` en el futuro) o ha sido revocada. El `exp` de tu `client_assertion` es un valor aparte, de vida corta — **no** es el periodo de validez de la credencial.
    - **Solución**: comprueba el `validFrom` / `validUntil` de la credencial y su estado de revocación (`credentialStatus`). Si ha caducado o ha sido revocada, emite una nueva `LEARCredentialMachine` y reconstruye la VP con ella. Ampliar el `exp` del `client_assertion` no arregla una credencial caducada.

??? bug "El relying party rechaza el token: `iss` no coincide con la URL del Verifier"

    - **Causa probable**: en el tenant **DOME** el Verifier acepta las peticiones de token con y sin el path `/verifier`, pero el token que emite **siempre** incluye `/verifier` en su `iss`. Si el relying party valida el issuer contra una URL base configurada **sin** `/verifier`, la comprobación falla. Por ejemplo, el `iss` del token es `https://verifier.dome-marketplace-sbx.org/verifier` mientras el relying party espera `https://verifier.dome-marketplace-sbx.org`.
    - **Solución**: configura el issuer esperado del relying party para que coincida con el `iss` del token **exactamente, incluyendo el path `/verifier`**. Usa la URL canónica con `/verifier` de forma consistente — tanto al pedir el token (`aud`) como al validarlo — para que la URL de la petición y el `iss` nunca diverjan.

??? warning "Consideraciones de seguridad"
    - El `jti` es de un solo uso: genera uno nuevo en cada petición. Reutilizarlo se rechaza como intento de replay.
    - Mantén `exp` corto. El `client_assertion` es un portador de identidad: cuanto menos viva, menor es la ventana de abuso.
    - Protege la clave privada de firma con el mismo cuidado que un `client_secret`.
    - Revocar la credencial de máquina corta el acceso del servicio en su siguiente petición de token. Es el mecanismo de baja: no hace falta rotar secretos.
    - El `access_token` es de vida corta y no renovable. No lo caches más allá de su `expires_in`.
