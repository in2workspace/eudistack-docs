# SCIM Provisioning

EUDIStack supports automated user provisioning using SCIM 2.0 (RFC 7643 / RFC 7644). When your corporate identity provider or HR platform creates, updates or disables employees, EUDIStack synchronizes those changes and executes the associated credential lifecycle automatically.

SCIM provisioning enables organizations to automate:

- Credential issuance during employee onboarding
- Credential renewal when employee attributes change
- Credential revocation during offboarding

EUDIStack acts as a **SCIM Service Provider** and receives push events from your corporate IdP or HRIS platform.

## When to use this guide

Use this integration if your organization:

- Manages employee lifecycle through an IdP or HR system
- Wants to automate credential issuance workflows
- Needs onboarding and offboarding without manual intervention
- Uses platforms such as Okta, Microsoft Entra ID or Workday

## Prerequisites

Before configuring SCIM provisioning, ensure you have:

- An active EUDIStack tenant
- Administrator access to your IdP or HRIS
- A SCIM provisioning token provided by EUDIStack
- At least one credential template configured in the Issuer

## Base URL

Each tenant exposes an isolated SCIM endpoint.

Example:

```txt
https://scim.<tenant>.eudistack.net/scim/v2
```

## Authentication

SCIM requests must include bearer token authentication.

## Supported resources

EUDIStack exposes SCIM-compatible endpoints for user lifecycle management.

| Resource | Endpoints |
|---|---|
| Users | `GET/POST /scim/v2/Users` |
| User by ID | `GET/PUT/PATCH/DELETE /scim/v2/Users/{id}` |
| Groups | `GET/POST /scim/v2/Groups` |

## Delivery modes

Credential delivery behavior is configurable per tenant.

Supported delivery modes include:

| Mode | Description |
|---|---|
| Email | Sends credential offer link or onboarding email to the employee |
| Direct | Returns the issued credential directly to the calling system |

## Example: create user

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
    {
      "primary": true,
      "value": "ana.garcia@example.com"
    }
  ]
}
```

Expected behavior:

- The user is provisioned in EUDIStack
- The Issuer starts automatic credential issuance
- Credential delivery is executed according to tenant configuration
- An audit event is registered

## Example: update user

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
    {
      "primary": true,
      "value": "ana.garcia@new-domain.example"
    }
  ]
}
```

Expected behavior:

- The previous credential is revoked
- A new credential is issued with updated attributes
- An audit event is registered

## Example: disable or delete user

```http
DELETE /scim/v2/Users/2819c223-7f76-453a-919d-413861904646
Authorization: Bearer <token>
```

Expected behavior:

- The active credential is revoked
- Status List information is updated automatically
- An audit event is registered

## Okta configuration

To configure SCIM provisioning with Okta:

1. Create a new SCIM application in Okta.
2. Configure the SCIM Base URL for your tenant.
3. Configure the bearer token provided by EUDIStack.
4. Enable user provisioning.
5. Configure attribute mappings.

## Microsoft Entra ID configuration

To configure SCIM provisioning with Microsoft Entra ID:

1. Create a new Enterprise Application.
2. Enable automatic provisioning.
3. Select SCIM as provisioning mode.
4. Configure the tenant SCIM Base URL.
5. Configure the provisioning secret token.
6. Configure attribute mappings.

## Troubleshooting

| Symptom | Possible cause | Resolution |
|---|---|---|
| `401 Unauthorized` | Invalid or expired token | Verify bearer token configuration |
| User not provisioned | Missing required mapping | Verify required attributes |
| Credential not issued | Missing credential template | Verify Issuer configuration |
| Revocation not visible | Status propagation delay | Wait for Status List update |

## Security considerations

- Always use HTTPS for SCIM endpoints
- Rotate provisioning tokens periodically
- Restrict SCIM access to trusted corporate systems
- Monitor audit events regularly
