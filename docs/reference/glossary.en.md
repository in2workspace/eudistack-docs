# Glossary

<!-- TODO: expand with EUDIStack-specific terms -->

## Identity and credentials

**Verifiable Credential (VC)**
: Digital document signed by an issuer that asserts properties about a subject (holder). In EUDIStack it is typically used in SD-JWT VC format.

**Verifiable Presentation (VP)**
: A set of one or more VCs (or selective portions) presented by the holder to a verifier, signed to prove possession.

**Holder**
: The person or organization that holds the credential in their wallet.

**Issuer**
: The entity that issues credentials (university, company, authority).

**Verifier (Relying Party)**
: The entity that requests and validates a presentation.

## Protocols

**OID4VCI**
: OpenID for Verifiable Credential Issuance — standard protocol for issuance.

**OID4VP**
: OpenID for Verifiable Presentations — standard protocol for presentation.

**DCQL**
: Digital Credentials Query Language — declarative language for a verifier to express which credentials/attributes it wants.

**DPoP**
: Demonstrating Proof-of-Possession — binds access tokens to a client key.

**PKCE**
: Proof Key for Code Exchange — protects the OAuth Authorization Code flow.

## Formats

**SD-JWT VC**
: Selective Disclosure JWT Verifiable Credential — format that allows the holder to reveal only the necessary claims.

**mdoc / mDL**
: ISO/IEC 18013-5 — binary format typically used for Mobile Driving Licence.

**W3C VC Data Model**
: The W3C's general specification for verifiable credentials; less used in the current EUDI ecosystem.

## Multi-tenant

**Tenant**
: Isolated customer within the EUDIStack platform. Each tenant has its own DB (schema), branding and URLs.

**Schema-per-tenant**
: Data isolation strategy in PostgreSQL: one schema per tenant within the same database.
