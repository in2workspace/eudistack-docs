# Sandbox

EUDIStack provides a public **sandbox** enviroment so you can test the integration before moving to a production tenant.
It allows you to validate complete issuance and verification flows using OID4VCI and OID4VP with synthetic data and test credentials.

## What the sandbox includes
- `sandbox` tenant with operational Issuer, Verifier, and Wallet.
- Complete credential issuance and presentation flows.
- Test credential catalog (LEARCredentialEmployee, etc.).
- Synthetic data for functional testing.
- No SLA — intended for development purposes only.

## How to request access
Access to the sandbox is managed manually by the EUDIStack team.

1. Contact the team through the support page.
2. Provide:
    - Organization name.
    - Use case.
    - Environment you want to integrate with (Issuer, Verifier, or Wallet).
3. You will receive:
    - Access code or credentials for the sandbox environment.
    - Basic configuration information.
    - Required endpoints to start the integration.

## Sandbox enviroment URLs

| Service | URL |
|---|---|
| Issuer API | `https://sandbox-stg.eudistack.net/issuer` |
| Verifier API | `https://sandbox-stg.eudistack.net/verifier` |
| Wallet PWA | `https://sandbox-stg.eudistack.net/wallet` |

## Next steps

Once you have access to the sandbox, continue with [your first integration](./first-integration.md)