# OID4VCI — OpenID for Verifiable Credential Issuance

OID4VCI is the standard protocol (based on OAuth 2.0) that defines the mechanism through which an Issuer delivers a verifiable credential to a wallet. EUDIStack implements this protocol for credential issuance.

The natively supported credential formats are **jwt_vc_json** and **SD-JWT VC**.

---

## Issuance Flows

The OID4VCI standard defines two flows for credential issuance. The EUDIStack implementation supports both; selecting the appropriate flow depends on each organization's use case.

=== "Pre-Authorized Code Flow"
    This flow is oriented towards scenarios where the Issuer already has the holder's data beforehand.
    The Issuer generates an offer that contains a pre-authorized authorization code. To mitigate the risk associated with intercepting the offer, the EUDIStack implementation systematically applies a 6-digit transaction code in numeric mode.

    At the moment the wallet resolves the offer URL, the system automatically sends the **transaction code** to the holder's email. This code must be entered by the holder into the wallet to validate access to the issuance process.
    After validating the code, the wallet exchanges the **pre-authorized_code** along with the **tx_code** to obtain the access token.

    ```mermaid
    sequenceDiagram
        autonumber
        participant W as EUDI Wallet
        participant I as EUDIStack Issuer

        Note over W, I: Pre-Authorized Code Flow
        W->>I: Resolves Offer (Credential Offer)
        Note right of I: Sends tx_code to the holder's email
        W->>I: Requests Token (pre-authorized_code + tx_code)
        I-->>W: Returns access_token
        W->>I: Requests Nonce
        I-->>W: Returns c_nonce
        W->>I: Requests Credential (with Proof JWT)
        I-->>W: Returns Credential
    ```

=== "Authorization Code Flow"
    This flow corresponds to the standard OAuth 2.0 mechanism. It is applied in self-service scenarios, where the holder explicitly initiates the process through a portal and authenticates with the Identity Provider (IdP) before receiving the credential.

    In this flow, the wallet opens an integrated browser, redirects the holder to the **IdP**, and, after successful authentication, receives the **authorization_code** to exchange it for the access token.

    ```mermaid
    sequenceDiagram
        autonumber
        participant W as EUDI Wallet
        participant I as EUDIStack Issuer
        participant IDP as Identity Provider

        Note over W, I: Authorization Code Flow
        W->>I: Resolves Offer (Credential Offer)
        W->>IDP: Redirects to browser for authentication
        Note over W, IDP: The holder authenticates with the IdP
        IDP-->>W: Returns authorization_code
        W->>I: Requests Token (authorization_code + PKCE)
        I-->>W: Returns access_token
        W->>I: Requests Nonce
        I-->>W: Returns c_nonce
        W->>I: Requests Credential (with Proof JWT)
        I-->>W: Returns Credential
    ```

---

## Anatomy of an Offer

??? note "Credential offer JSON structure"
    To initiate either of the two flows, the wallet needs to resolve an offer URL. The JSON pointed to by this URL has the following structure:

    === "Pre-Authorized Offer"
        The Issuer emits the offer directly with a pre-authorized code. The `tx_code` field indicates that a 6-digit transaction code sent to the holder's email will be required:

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

    === "Authorization Offer"
        The holder actively initiates the flow. The `issuer_state` field is a signed JWT that references the issuance context on the server:

        ```json
        {
          "credential_issuer": "https://issuer.sandbox.eudistack.net",
          "credential_configuration_ids": [
            "learcredential.employee.w3c.4"
          ],
          "grants": {
            "authorization_code": {
              "issuer_state": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
            }
          }
        }
        ```

---

## References

* **API Reference:** [Issuer Endpoints](../api-reference/issuer.md)
* **Official Specification:** [OID4VCI 1.0](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)
* **HAIP Profile:** [Standards](../../reference/standards.md)