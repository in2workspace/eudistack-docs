# OID4VP — OpenID for Verifiable Presentations

OID4VP es el protocolo estándar que define cómo un Verificador solicita y recibe presentaciones de credenciales verificables desde un wallet. La implementación de EUDIStack actúa como el componente verificador, permitiendo a aplicaciones de terceros consumir datos de identidad de forma segura y estandarizada.

Los formatos de credencial soportados para la verificación incluyen **jwt_vc_json** (W3C) y **dc+sd-jwt** (SD-JWT VC).

---

## Flujos de presentación

La implementación soporta flujos adaptados a diferentes contextos de interacción entre el titular y la entidad verificadora:

- **Cross-device (QR)** — El titular interactúa con una aplicación web en un dispositivo y utiliza el wallet en otro para escanear un código QR e iniciar la presentación.
- **Direct Post** — Mecanismo mediante el cual el wallet envía la presentación directamente al endpoint del verificador a través de una petición HTTP POST, garantizando la privacidad y seguridad del intercambio.

---

## Diagrama del flujo

El proceso de presentación se basa en una interacción coordinada entre la aplicación cliente, el componente verificador de EUDIStack y el wallet del usuario.
La secuencia comienza con la creación de una sesión de verificación y termina con la entrega de los claims validados a la aplicación solicitante tras una comprobación íntegra de firmas y estados de revocación.
```mermaid
sequenceDiagram
    autonumber
    participant W as EUDI Wallet
    participant V as EUDIStack Verifier
    participant C as Client App (RP)

    Note over W, C: Inicio de Presentación
    C->>V: Crea solicitud de presentación
    V-->>C: Retorna ID de sesión + QR/URI
    C->>W: Muestra QR o Deep Link⠀⠀⠀⠀⠀⠀⠀⠀
    W->>V: Recupera JWT de solicitud
    V-->>W: Retorna JWT firmado
    Note over W: El titular selecciona los claims
    W->>V: Envía Presentación (POST Direct Post)

    activate V
    V->>V: Valida firmas e integridad
    V->>V: Valida estado (Status List)
    deactivate V

    V-->>W: HTTP 200 OK
    V-->>C: Notifica vía SSE (redirect URL + auth code)
    C->>V: Intercambia auth code por tokens (OAuth 2.0)
```

---

## Anatomía de una Solicitud

El objeto central de OID4VP es la solicitud de presentación. A continuación se muestra un ejemplo de un objeto JSON que el wallet resuelve al iniciar el flujo para una credencial de empleado:
```json
{
  "iss": "did:key:z6Mk...",
  "aud": "https://self-issued.me/v2",
  "iat": 1746524400,
  "exp": 1746524700,
  "jti": "550e8400-e29b-41d4-a716-446655440000",
  "client_id": "did:key:z6Mk...",
  "nonce": "a4f8c2d1-3e7b-4a9d-b5f0-1c6e2a8d4f7b",
  "response_uri": "https://verifier.example.com/oid4vp/auth-response",
  "scope": "openid learcredential.employee",
  "state": "9f3b1c2a-7e4d-48a5-b0f2-3d6c8e1a5b9f",
  "response_type": "vp_token",
  "response_mode": "direct_post",
  "dcql_query": {
    "credentials": [
      {
        "id": "lear_employee_sd_jwt",
        "format": "dc+sd-jwt",
        "meta": {
          "vct_values": ["eu.europa.ec.eudi.lce.1"]
        }
      }
    ]
  },
  "client_metadata": {
    "vp_formats_supported": {
      "dc+sd-jwt": {
        "sd-jwt_alg_values": ["ES256"],
        "kb-jwt_alg_values": ["ES256"]
      },
      "jwt_vc_json": {
        "alg_values_supported": ["ES256"]
      }
    }
  }
}
```

---

## Referencias

* **Especificación Oficial:** [OID4VP 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)
* **Consulta de Credenciales:** [Digital Credential Query Language (DCQL)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#section-dcql)
* **Referencia API:** [Endpoints del Verificador](../api-reference/verifier.md)