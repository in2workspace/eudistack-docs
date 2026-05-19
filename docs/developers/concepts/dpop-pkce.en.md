# DPoP & PKCE

EUDIStack applies **PKCE** and **DPoP** across all OAuth 2.0 and OID4VCI flows exposed by the Issuer, Verifier and Wallet EBW.

| Mechanism | Purpose |
|---|---|
| PKCE | Protects the `authorization_code`. |
| DPoP | Protects the `access_token` against theft or replay attacks. |

---

## PKCE

**Proof Key for Code Exchange** (PKCE) protects the OAuth 2.0 Authorization Code flow.

The client generates a temporary secret (`code_verifier`) and sends only its hash (`code_challenge`) during the initial authorization request. When redeeming the `authorization_code`, the client must prove possession of the original `code_verifier`. If an attacker intercepts the code but does not know the verifier, they cannot obtain tokens.

EUDIStack requires PKCE for all Authorization Code flows, including confidential clients.

??? info "Important considerations"
    - `code_challenge_method=S256` is mandatory. The `plain` method is **not supported**.
    - The `code_verifier` must remain only in the client that initiated the flow.
    - PKCE is enforced in: OID4VCI Authorization Code flows, wallet login flows, and protected OAuth/OIDC integrations.

=== "Flow diagram"

    ```mermaid
    sequenceDiagram
        participant W as Wallet EBW
        participant I as Issuer Authorization Server

        W->>W: Generate code_verifier
        W->>W: Compute SHA256(code_verifier) → code_challenge

        W->>I: Authorization request + code_challenge
        I-->>W: authorization_code

        W->>I: authorization_code + code_verifier
        I->>I: Validate verifier

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

**Demonstrating Proof-of-Possession at the Application Layer** (DPoP) binds access tokens to a cryptographic key controlled by the client.

Instead of using a reusable bearer token, the client must prove possession of the private key in every request using a `DPoP` header. Even if an attacker steals the token, they cannot use it without the associated private key.

DPoP is mandatory in EUDIStack to prevent replay attacks and ensure that issued tokens are cryptographically bound to the wallet that initiated the flow.

??? info "Where does DPoP apply?"
    - Issuer token endpoints.
    - Credential endpoints (OID4VCI exchange).
    - Protected Wallet EBW management endpoints.

=== "Flow diagram"

    ```mermaid
    sequenceDiagram
        participant W as Wallet EBW
        participant I as Issuer
        participant API as Credential API

        W->>W: Generate key pair
        W->>I: Token request + DPoP proof
        I->>I: Bind token to public key
        I-->>W: DPoP-bound access_token

        W->>API: Credential request + DPoP proof + access_token
        API->>API: Validate proof and key binding
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

=== "DPoP Payload"

    ```json
    {
      "jti": "52ede3f4-8c13-4c6f-9b41-1f5d2c11d8c7",
      "htm": "POST",
      "htu": "https://sandbox-stg.eudistack.net/issuer/oid4vci/v1/token",
      "iat": 1710000000
    }
    ```

---

## References

- [PKCE (RFC 7636)](https://datatracker.ietf.org/doc/rfc7636/)
- [DPoP (RFC 9449)](https://datatracker.ietf.org/doc/rfc9449/)
- [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)
