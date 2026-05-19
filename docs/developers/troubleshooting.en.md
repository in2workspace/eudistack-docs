# Troubleshooting — Developers

Common errors during integration and how to resolve them.

=== "OID4VCI"

    ??? bug "`invalid_grant` when redeeming the pre-authorized code"

        - **Likely cause**: the code was already consumed or expired (typical TTL: 5 minutes).
        - **Solution**: request a new offer from the Issuer. The pre-authorized code is single-use.

    ??? bug "Wallet rejects the credential after issuance"

        - **Likely cause**: mismatch in the `vct` (Verifiable Credential Type) or missing public verification keys at `/.well-known/jwt-issuer`.
        - **Solution**: verify that the `vct` declared in the Issuer metadata matches the one in the issued credential.

=== "OID4VP"

    ??? bug "Verifier does not receive the presentation"

        - **Likely cause**: the `response_uri` is not publicly accessible or has an invalid certificate.
        - **Solution**: the wallet performs `direct_post` from the user's device; the endpoint must be a public HTTPS endpoint with a valid certificate.

    ??? bug "`invalid_presentation_submission`"

        - **Likely cause**: the DCQL query requests claims that the credential does not contain, or the Key Binding JWT signature does not validate.
        - **Solution**: verify that the presented credential satisfies the query and that the `nonce` in the KB-JWT matches the one in the request.

=== "DPoP"

    ??? bug "`invalid_dpop_proof`"

        - **Likely cause**: the `htu` or `htm` in the DPoP does not match the actual URL/method, or the `iat` is outside the acceptable window.
        - **Solution**: verify that you regenerate the DPoP proof on every request; do not reuse one across calls.

=== "Multi-tenant"

    ??? bug "`404` or `tenant not found` with correct domain"

        - **Likely cause**: the subdomain is not provisioned or the session points to a different tenant.
        - **Solution**: confirm with support that the tenant is active and that your OAuth client is registered for it.

---

## Still stuck?

[Contact support](../support.md). Include:

- Exact endpoint and method.
- Complete request (sanitized of secrets).
- Response received.
- `trace-id` if included in the response.

