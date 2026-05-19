# DPoP & PKCE

EUDIStack aplica **PKCE** y **DPoP** en todos los flujos OAuth 2.0 y OID4VCI expuestos por el Issuer, Verifier y Wallet EBW.

| Mecanismo | Objetivo |
|---|---|
| PKCE | Protección del `authorization_code`. |
| DPoP | Protección del `access_token` frente a robo o replay. |

---

## PKCE

**Proof Key for Code Exchange** (PKCE) protege el flujo OAuth 2.0 Authorization Code.

El cliente genera un secreto temporal (`code_verifier`) y envía únicamente su hash (`code_challenge`) durante la autorización inicial. Para canjear el `authorization_code`, debe demostrar que conoce el `code_verifier` original. Si un atacante intercepta el código pero no conoce el verifier, no puede obtener tokens.

EUDIStack exige PKCE en todos los flujos Authorization Code, incluidos clientes confidenciales.

??? info "Consideraciones importantes"
    - `code_challenge_method=S256` es obligatorio. El método `plain` **no está soportado**.
    - El `code_verifier` debe mantenerse únicamente en el cliente que inició el flujo.
    - PKCE se aplica en: OID4VCI Authorization Code Flow, wallet login flows e integraciones OAuth/OIDC protegidas.

=== "Diagrama de flujo"

    ```mermaid
    sequenceDiagram
        participant W as Wallet EBW
        participant I as Issuer Authorization Server

        W->>W: Genera code_verifier
        W->>W: Calcula SHA256(code_verifier) → code_challenge

        W->>I: Authorization request + code_challenge
        I-->>W: authorization_code

        W->>I: authorization_code + code_verifier
        I->>I: Valida el verifier

        I-->W: access_token
    ```

=== "Authorization Request"

    ```http
    GET /oauth/authorize?
      response_type=code&
      client_id=wallet-dev&
      code_challenge_method=S256&
      code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-c&
      redirect_uri=https://wallet.example/callback
    ```

=== "Token Exchange"

    ```http
    POST /oid4vci/v1/token HTTP/1.1
    Host: sandbox-stg.eudistack.net
    Content-Type: application/x-www-form-urlencoded

    grant_type=authorization_code&
    code=SplxlOBeZQQYbYS6WxSbIA&
    code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk&
    redirect_uri=https://wallet.example/callback
    ```

---

## DPoP

**Demonstrating Proof-of-Possession at the Application Layer** (DPoP) vincula los access tokens a una clave criptográfica controlada por el cliente.

En lugar de usar un bearer token reutilizable, el cliente debe demostrar posesión de la clave privada en cada request mediante una cabecera `DPoP`. Aunque un atacante robe el token, no podrá utilizarlo sin la clave privada asociada.

DPoP es obligatorio en EUDIStack para evitar replay attacks y garantizar que los tokens emitidos estén vinculados criptográficamente al wallet que inició el flujo.

??? info "¿Dónde aplica DPoP?"
    - Token endpoint del Issuer.
    - Credential endpoint (canje OID4VCI).
    - Endpoints de gestión protegidos del Wallet EBW.

=== "Diagrama de flujo"

    ```mermaid
    sequenceDiagram
        participant W as Wallet EBW
        participant I as Issuer
        participant API as Credential API

        W->>W: Genera key pair
        W->>I: Token request + DPoP proof
        I->>I: Vincula token a la clave pública
        I-->>W: DPoP-bound access_token

        W->>API: Credential request + DPoP proof + access_token
        API->>API: Valida proof y key binding
        API-->>W: Credential response
    ```

=== "Token Request"

    ```http
    POST /oid4vci/v1/token HTTP/1.1
    Host: sandbox-stg.eudistack.net
    Authorization: Basic d2FsbGV0LWRldjpzZWNyZXQ=
    DPoP: eyJhbGciOiJFUzI1NiIsImp3ayI6eyJrdHkiOiJFQyIsImNydiI6IlAtMjU2In19...
    Content-Type: application/x-www-form-urlencoded

    grant_type=authorization_code&
    code=SplxlOBeZQQYbYS6WxSbIA&
    code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
    ```

=== "Payload DPoP"

    ```json
    {
      "jti": "52ede3f4-8c13-4c6f-9b41-1f5d2c11d8c7",
      "htm": "POST",
      "htu": "https://sandbox-stg.eudistack.net/issuer/oid4vci/v1/token",
      "iat": 1710000000
    }
    ```

---

## Referencias

- [PKCE (RFC 7636)](https://datatracker.ietf.org/doc/rfc7636/)
- [DPoP (RFC 9449)](https://datatracker.ietf.org/doc/rfc9449/)
- [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)
