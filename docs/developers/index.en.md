# Developer Documentation

EUDIStack exposes a multi-tenant SaaS platform to issue, custody and verify digital credentials compliant with eIDAS 2.0. This section is aimed at integrators connecting EUDIStack with their own systems (IdP, ERP, corporate portals, etc.).

<div class="grid cards" markdown>

-   :material-rocket-launch: [**Getting started**](getting-started/index.en.md)

    Quickstart with the EUDIStack sandbox and your first working integration in under 30 minutes.

-   :material-book-open-page-variant: [**Concepts**](concepts/index.en.md)

    The key protocols: OID4VCI, OID4VP, SD-JWT VC, DPoP, multi-tenant. The minimum you need to understand before writing code.

-   :material-format-list-checks: [**Guides**](guides/index.en.md)

    Recipes for concrete use cases: OIDC IdP, SCIM, API Direct Issuance.

-   :material-api: [**API Reference**](api-reference/index.en.md)

    Endpoints, request/response examples and full OpenAPI specifications.

</div>

---

## Something not working?

Check the [developer troubleshooting](troubleshooting.en.md) section — it covers the most common errors with their root cause and resolution.

!!! eudistack "EUDIStack is SaaS"
    Nothing to deploy. We operate in managed mode: you integrate your systems with our APIs and we run the platform. If you need an isolated environment, we provision a **dedicated tenant** with its own URLs, database and configuration.

