# OID4VCI — OpenID for Verifiable Credential Issuance

OID4VCI es el protocolo estándar, basado en OAuth 2.0, que define el mecanismo mediante el cual un Issuer entrega una credencial verificable a un wallet. EUDIStack implementa este protocolo para la emisión de credenciales y expone la configuración pública del Issuer a través del endpoint de metadatos (`/.well-known/openid-credential-issuer`).

Los formatos de credencial soportados de forma nativa son `jwt_vc_json` y `dc+sd-jwt` (SD-JWT VC).

---

## Flujos de emisión

El estándar OID4VCI define dos flujos para la emisión de credenciales. La implementación de EUDIStack soporta ambos; la selección del flujo adecuado depende del caso de uso de cada organización.

### 1. Pre-Authorized Code Flow

Este flujo está orientado a escenarios en los que el Issuer dispone previamente de los datos del titular. El Issuer genera una oferta que contiene un código de autorización preautorizado. Para mitigar el riesgo asociado a la interceptación de la oferta, la implementación de EUDIStack aplica de forma sistemática un código de transacción (`tx_code`) de 6 dígitos en modo numérico.

**Comportamiento de la implementación:**

En el momento en que el wallet resuelve la URL de la oferta, el sistema envía automáticamente el `tx_code` al correo electrónico del titular. Dicho código deberá ser introducido por el titular en el wallet para validar el acceso al proceso de emisión. Tras la validación del código, el wallet canjea el `pre-authorized_code` junto con el `tx_code` para obtener el token de acceso.

```mermaid
sequenceDiagram
    autonumber
    participant W as EUDI Wallet
    participant I as EUDIStack Issuer

    Note over W, I: Pre-Authorized Code Flow
    W->>I: Resuelve Oferta (Credential Offer)
    Note right of I: Envía tx_code al email del titular
    W->>I: Solicita Token (pre_authorized_code + tx_code)
    I-->>W: Retorna access_token
    W->>I: Solicita Nonce
    I-->>W: Retorna c_nonce
    W->>I: Solicita Credencial (con Proof JWT)
    I-->>W: Retorna Credencial
```

### 2. Authorization Code Flow

Este flujo corresponde al mecanismo OAuth 2.0 estándar. Se aplica en escenarios de autoservicio, en los que el titular inicia el proceso de forma explícita a través de un portal y se autentica ante el Identity Provider (IdP) antes de recibir la credencial.

En este flujo no se generan códigos preautorizados ni se envían códigos de transacción por correo electrónico. El wallet abre un navegador integrado, redirige al titular hacia el IdP y, tras una autenticación exitosa, recibe el `authorization_code` para canjearlo por el token de acceso. La implementación de EUDIStack admite Pushed Authorization Requests (PAR) como mecanismo de inicio del flujo.

```mermaid
sequenceDiagram
    autonumber
    participant W as EUDI Wallet
    participant I as EUDIStack Issuer
    participant IDP as Identity Provider

    Note over W, I: Authorization Code Flow
    W->>I: Resuelve Oferta (Credential Offer)
    W->>IDP: Redirige al navegador para autenticación
    Note over W, IDP: El titular se autentica ante el IdP
    IDP-->>W: Retorna authorization_code
    W->>I: Solicita Token (authorization_code + PKCE)
    I-->>W: Retorna access_token
    W->>I: Solicita Nonce
    I-->>W: Retorna c_nonce
    W->>I: Solicita Credencial (con Proof JWT)
    I-->>W: Retorna Credencial
```

---

## Anatomía de una Oferta

Para iniciar cualquiera de los dos flujos, el wallet necesita resolver una URL de oferta (`credential-offer`). El JSON al que apunta dicha URL tiene la siguiente estructura (ejemplo para el flujo pre-autorizado):

```json
{
  "credential_issuer": "https://issuer.sandbox.eudistack.net",
  "credential_configuration_ids": [
    "learcredential.employee.w3c.4"
  ],
  "grants": {
    "urn:ietf:params:oauth:grant-type:pre-authorized_code": {
      "pre-authorized_code": "oaKazRN8I0IbtZ0C7JuMn5",
      "tx_code": {
        "length": 6,
        "input_mode": "numeric",
        "description": "Enter the activation code"
      }
    }
  }
}
```

---

## Referencias

* **Referencia API:** [Endpoints Issuer](../api-reference/issuer.md)
* **Especificación Oficial:** [OID4VCI 1.0](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)
* **Perfil HAIP:** [Estándares](../../reference/standards.md)