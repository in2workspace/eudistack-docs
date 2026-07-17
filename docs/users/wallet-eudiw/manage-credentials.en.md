# Manage credentials

How to view and delete credentials from the wallet.

---

## How to access the credentials

Tap the *Credentials* tab in the bottom navigation bar.

![Bottom navigation bar — Credentials tab](../../assets/img/users/wallet-eudiw/nav-bottom-vc.png){ width="240" }

---

## Credentials List

??? "View listing"
    Lists all active credentials with:

    - Credential type.
    - Issuer.
    - Status:

        - **VALID**: the credential is current and can be used for presentations.
        - **EXPIRED**: the credential has passed its expiry date and cannot be presented.
        - **REVOKED**: the issuer has invalidated the credential; it cannot be used for presentations.

    - Issuance and expiry date.

    ![List of credentials in the Credentials tab](../../assets/img/users/wallet-eudiw/vc-list.png){ width="240" }

---

## Actions

??? "View details of a credential"
    Tap a credential card to show all its attributes.

    ![List of credentials in the Credentials tab](../../assets/img/users/wallet-eudiw/vc-list.png){ width="240" }

    ![Credential details](../../assets/img/users/wallet-eudiw/vc-detail.png){ width="240" }

??? "Verify credential"
    Checks the validity of the credential at that moment. Shows the issuer, the issuance date, the expiry date and the revocation status. The button is at the bottom of the credential details screen.

    ![Verify credential button on the details screen](../../assets/img/users/wallet-eudiw/vc-verification-button.png){ width="240" }

    ![Credential verification screen](../../assets/img/users/wallet-eudiw/vc-verification.png){ width="240" }

??? "Delete a credential"
    The **Delete** button is at the bottom of the details screen. It permanently deletes the credential from the wallet.

    ![Delete button on the details screen](../../assets/img/users/wallet-eudiw/vc-delete-button.png){ width="240" }

    !!! warning "This action cannot be undone"
        If you need to use it again, you will have to request it from the issuer again.

---

## Activity

The history of presentations and receptions is viewed in **[Settings → Activity](settings.md#actividad)**.
