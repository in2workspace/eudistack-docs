# Troubleshooting — Admins

This section covers the most frequent issues with the Issuer Portal and how to resolve them. If the issue does not appear here, see [support](../support.md).

---

## Issuer Portal

??? "Login error"
    - **Symptom**: login with the verifiable credential fails.
    - **Probable cause**: the corporate credential has expired or been revoked.
    - **Solution**: contact the organization admin to issue a new one.

??? "The recipient does not receive the offer"
    - **Symptom**: the credential is issued but the recipient does not receive it.
    - **Probable cause**: email filtered as spam or incorrect email address.
    - **Solution**:

        - **Email in spam**: ask the recipient to check their spam folder.
    
        - **Incorrect address**: in the Issuer Portal, open the issuance detail, use **Withdraw** to cancel it, and issue a new credential with the correct address.
        ![Credential in DRAFT status with Withdraw button](../assets/img/admin/issuer-portal/withdraw.png){ width="560" }
        
        → See [Manage issuances](issuer-portal/manage-issuances.md#statuses-during-the-issuance-flow)

---

## Still can't resolve it?

[Contact support](../support.md). Include:

- A screenshot of the error, if any.
- The approximate time of the incident.
- The organization (tenant).
