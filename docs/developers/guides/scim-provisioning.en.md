# SCIM Provisioning

EUDIStack supports automated user provisioning using SCIM 2.0 (RFC 7643 / RFC 7644). When your corporate identity provider or HR platform creates, updates or disables employees, EUDIStack synchronizes those changes and executes the associated credential lifecycle automatically.

SCIM provisioning enables organizations to automate:

- Credential issuance during employee onboarding.
- Credential renewal when employee attributes change.
- Credential revocation during offboarding.

EUDIStack acts as a **SCIM Service Provider** and receives push events from your corporate IdP or HRIS platform.

??? tip "When to use this guide"
    Use this integration if your organization:

    - Manages employee lifecycle through an IdP or HR system.
    - Wants to automate credential issuance workflows.
    - Needs onboarding and offboarding without manual intervention.
    - Uses platforms such as Okta, Microsoft Entra ID or Workday.

??? info "Prerequisites"
    Before configuring SCIM provisioning, ensure you have:

    - An active EUDIStack tenant.
    - Administrator access to your IdP or HRIS.
    - A SCIM provisioning token provided by EUDIStack.
    - At least one credential template configured in the Issuer.

---

## Base URL

Each tenant exposes an isolated SCIM endpoint.

```txt
https://scim.<tenant>.eudistack.net/scim/v2
```

> **Note on URL pattern:** The SCIM endpoint uses the `scim.<tenant>` subdomain, which differs from the standard multi-tenant pattern `{tenant}-stg.eudistack.net` used by the Issuer, Verifier and Wallet services. Contact the EUDIStack team to confirm the exact endpoint for your environment (STG or production) when requesting access.

SCIM requests must include **bearer token** authentication.

---

## Resources and lifecycle

=== "Supported resources"
    PATCH operations and Groups support may vary depending on tenant configuration and deployed version.

    | Resource | Endpoints |
    |---|---|
    | Users | `GET/POST /scim/v2/Users` |
    | User by ID | `GET/PUT/PATCH/DELETE /scim/v2/Users/{id}` |
    | Groups | `GET/POST /scim/v2/Groups` |


===  "Credential lifecycle behaviour"
    | SCIM event | Behaviour |
    |---|---|
    | `POST /Users` | Provisions the user and triggers automatic credential issuance |
    | `PUT /Users/{id}` | Renews the credential with updated attributes |
    | `DELETE /Users/{id}` | Revokes the active credential |

=== "Delivery modes"
    Credential delivery behaviour is configurable per tenant.

    | Mode | Description |
    |---|---|
    | Email | Sends credential offer link or onboarding email to the employee |
    | Direct | Returns the issued credential directly to the calling system |

---

## SCIM operations

=== "Create user"

        ```http
        POST /scim/v2/Users
        Authorization: Bearer <token>
        Content-Type: application/scim+json
        ```

        ```json
        {
          "userName": "ana.garcia",
          "active": true,
          "name": {
            "givenName": "Ana",
            "familyName": "Garcia"
          },
          "emails": [
            { "primary": true, "value": "ana.garcia@example.com" }
          ]
        }
        ```

    **Result:** user provisioned → automatic credential issuance triggered → delivery executed per tenant configuration → audit event registered.

=== "Update user"

        ```http
        PUT /scim/v2/Users/2819c223-7f76-453a-919d-413861904646
        Authorization: Bearer <token>
        Content-Type: application/scim+json
        ```

        ```json
        {
          "userName": "ana.garcia",
          "active": true,
          "name": {
            "givenName": "Ana",
            "familyName": "Garcia"
          },
          "emails": [
            { "primary": true, "value": "ana.garcia@new-domain.example" }
          ]
        }
        ```

    **Result:** previous credential revoked → new credential issued with updated attributes → audit event registered.

=== "Delete user"

        ```http
        DELETE /scim/v2/Users/2819c223-7f76-453a-919d-413861904646
        Authorization: Bearer <token>
        ```

    **Result:** active credential revoked → Status List updated automatically → audit event registered.

---

## Provider configuration

=== "Okta"

    1. Create a new SCIM application in Okta.
    2. Configure the SCIM Base URL for your tenant.
    3. Configure the bearer token provided by EUDIStack.
    4. Enable user provisioning.
    5. Configure attribute mappings.

=== "Microsoft Entra ID"

    1. Create a new Enterprise Application.
    2. Enable automatic provisioning.
    3. Select SCIM as provisioning mode.
    4. Configure the tenant SCIM Base URL.
    5. Configure the provisioning secret token.
    6. Configure attribute mappings.

---

??? question "Troubleshooting"

    | Symptom | Possible cause | Resolution |
    |---|---|---|
    | `401 Unauthorized` | Invalid or expired token | Verify bearer token configuration |
    | User not provisioned | Missing required mapping | Verify required attributes |
    | Credential not issued | Missing credential template | Verify Issuer configuration |
    | Revocation not visible | Status propagation delay | Wait for Status List update |

??? warning "Security considerations"
    - Always use HTTPS for SCIM endpoints.
    - Rotate provisioning tokens periodically.
    - Restrict SCIM access to trusted corporate systems.
    - Monitor audit events regularly.
