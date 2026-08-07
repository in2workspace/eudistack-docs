# M2M Authentication

Lets a backend system obtain an `access_token` from the Verifier with no user involved, by presenting a **machine credential** instead of a shared secret.

!!! note "Not to be confused with the Issuer's client credentials"
    The [API Direct Issuance](api-direct-issuance.en.md) guide uses `client_id` + `client_secret` against the **Issuer**'s token endpoint. This guide covers the **Verifier**'s token endpoint, which uses no `client_secret`: the service proves its identity with a verifiable credential.

??? tip "When to use this guide"
    - One of your services needs to call EUDIStack-protected APIs with no user present.
    - You want that service's identity to be a revocable verifiable credential rather than a shared secret.
    - You already hold a machine credential issued in the service's name.

??? info "Prerequisites"
    - A machine credential (`LEARCredentialMachine`) issued by an issuer the Verifier recognises as trusted.
    - The private key bound to the credential's subject, to sign the `client_assertion`.
    - The tenant you will operate against.

---

## Endpoint

```txt
POST https://<tenant>.eudistack.net/verifier/oidc/token
```

On sandbox: `https://sandbox.stg.eudistack.net/verifier/oidc/token`.

---

## The flow

1. You build a Verifiable Presentation (VP) holding your machine credential and Base64-encode it.
2. You build and sign a `client_assertion` (JWT) carrying that VP.
3. You send both to the token endpoint with `grant_type=client_credentials`.
4. The Verifier validates the credential (signature, trusted issuer, validity, revocation) and returns an `access_token`.

The examples below use the sandbox tenant and a machine whose DID is
`did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy`. Replace every value with your own.

=== "Step 1: the vp_token"

    Wrap your machine credential (a JWT VC string) in a VP object. If the credential is stored Base64-encoded, decode it first.

    ```json title="VP object"
    {
      "@context": ["https://www.w3.org/ns/credentials/v2"],
      "type": ["VerifiablePresentation"],
      "verifiableCredential": ["eyJhbGci...ssw5c"]
    }
    ```

    Sign that VP object as a JWT with the machine's private key. Use `NumericDate` (seconds) for the time claims and keep `exp` short:

    ```json title="VP JWT payload"
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

    Base64-encode (standard alphabet) the resulting VP JWT string. That encoded value is the `vp_token`:

    ```txt
    vp_token = base64( <VP in JWT format> )
    ```

=== "Step 2: the client_assertion"

    A JWT signed with your service's key (header `alg = ES256` or `RS256`, `kid` = the machine's DID key). Required claims:

    | Claim | Value |
    |---|---|
    | `iss` | your `client_id` (the machine DID) |
    | `sub` | your `client_id` (identical to `iss`) |
    | `aud` | the Verifier's public URL: `https://<tenant>.eudistack.net/verifier` |
    | `jti` | unique identifier, **single use** |
    | `iat` | current time (seconds) |
    | `exp` | short expiry (e.g. `iat + 10` seconds) |
    | `vp_token` | the encoded VP from step 1 |

    ```json title="client_assertion payload"
    {
      "iss": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "sub": "did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy",
      "aud": "https://sandbox.stg.eudistack.net/verifier",
      "jti": "urn:uuid:9b7e2c14-1f8a-49c0-9b0e-6d1f7c2a5e4b",
      "iat": 1731570000,
      "exp": 1731570010,
      "vp_token": "eyJhbGci...<base64 of the VP JWT>"
    }
    ```

=== "Step 3: the request"

    ```http
    POST /verifier/oidc/token HTTP/1.1
    Host: sandbox.stg.eudistack.net
    Content-Type: application/x-www-form-urlencoded

    grant_type=client_credentials&
    client_id=did:key:zDnaeceoAgeqEmnaVrRhCUauPGCPMUCJqvhaZjD4wTfHrxGoy&
    client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer&
    client_assertion=eyJhbGci...
    ```

    `client_id` must always be sent in the form body, even if your client is pre-registered.

=== "Step 4: the response"

    ```json
    {
      "access_token": "eyJhbGci...",
      "token_type": "Bearer",
      "expires_in": 900
    }
    ```

    The response carries `Cache-Control: no-store`. This grant returns **no `id_token` and no `refresh_token`**: there is no authenticated user and no session to renew. When the token expires, repeat the flow with a fresh `jti`.

    ??? example "Decoded access_token"

        ```json title="Header"
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

## Eligible credentials

Only **machine** credentials can use this grant. Employee credentials and every other type are limited to user login via `authorization_code`.

| Credential type | M2M |
|---|---|
| `learcredential.machine.w3c.2` · `learcredential.machine.w3c.3` | Yes |
| `learcredential.machine.sd.1` | Yes |
| `learcredential.employee.*` | No |
| Any other type | No |

> **Legacy versions:** older DOME format "LEARCredentialMachine" is temporarily supported for tenant DOME. In other tenants it may be disabled by tenant configuration. If your credential uses a retired format, the endpoint answers `410` even though the type is a machine one.

---

## client_id and tenant

You do not need to pre-register your service with the Verifier. If the `client_id` is not registered, the Verifier derives both the identity and the tenant from the presented credential itself.

For that to work, the credential must explicitly authorise the tenant you are calling: at least one of the credential's `mandate.power[].domain` values must match the tenant in the URL subdomain. If the credential declares no domain, or none matches, the request is rejected.

---

## Troubleshooting

Token endpoint error responses are deliberately generic and carry no `error_description`: one code covers several causes, so that no detail about the presented credential leaks. The precise reason is recorded in your tenant's audit log — ask support if you need it confirmed.

| Error | Possible causes | What to check, in order |
|---|---|---|
| `invalid_client` | The credential is not a machine one · malformed `client_assertion` or wrong claims · `jti` already used · untrusted issuer · malformed VP · the tenant is not authorised by the credential | Credential type → `iss`/`sub`/`aud`/`jti`/`exp` → Base64 encoding of `vp_token` → `power[].domain` against the subdomain |
| `invalid_grant` | Credential expired, not yet valid, or revoked | The credential's validity dates and revocation status |
| `server_error` | The Verifier could not reach the trusted issuers registry or the Status List | Retry with exponential backoff; if it persists, contact support |
| `401` with no body | `client_id` or `client_assertion` missing from the form | Both parameters are mandatory |
| `410` · `503` | That credential format is disabled on the tenant | Re-issue the credential in a current format |
| `429` | Per-minute request limit exceeded | Lower the request rate and honour the `Retry-After` header |

??? bug "The token endpoint returns `invalid_grant`"

    - **Probable cause**: the machine credential inside the VP is expired (past its `validUntil`), not yet valid (`validFrom` in the future), or has been revoked. The `exp` of your `client_assertion` is a separate, short-lived value — it is **not** the credential's validity period.
    - **Solution**: check the credential's `validFrom` / `validUntil` and its revocation status (`credentialStatus`). If it has expired or been revoked, issue a new `LEARCredentialMachine` and rebuild the VP with it. Extending the `client_assertion` `exp` will not fix an expired credential.

??? bug "The relying party rejects the token: `iss` does not match the Verifier URL"

    - **Probable cause**: on tenant **DOME** the Verifier accepts token requests both with and without the `/verifier` path, but the token it issues **always** carries `/verifier` in its `iss`. If the relying party validates the issuer against a base URL configured **without** `/verifier`, the check fails. For example, the token's `iss` is `https://verifier.dome-marketplace-sbx.org/verifier` while the relying party expects `https://verifier.dome-marketplace-sbx.org`.
    - **Solution**: configure the relying party's expected issuer to match the token's `iss` **exactly, including the `/verifier` path**. Use the canonical URL with `/verifier` consistently — both when requesting the token (`aud`) and when validating it — so the request URL and the `iss` never diverge.

??? warning "Security considerations"
    - The `jti` is single use: generate a new one per request. Reusing it is rejected as a replay attempt.
    - Keep `exp` short. The `client_assertion` is an identity bearer: the shorter it lives, the smaller the abuse window.
    - Protect the signing private key with the same care as a `client_secret`.
    - Revoking the machine credential cuts the service's access on its next token request. That is the offboarding mechanism — no secret rotation needed.
    - The `access_token` is short-lived and non-renewable. Do not cache it beyond its `expires_in`.
