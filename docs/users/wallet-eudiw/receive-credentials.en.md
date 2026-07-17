# Receive credentials

How to accept a verifiable credential issued by an Issuer.

---

## Available methods

- **QR on screen**: the Issuer portal shows a QR code on screen. Scan it with the wallet to add the credential.
- **By email**: you receive an email with a QR code and an *Open in Wallet* button. Scan the QR or tap the button to add the credential.

---

## Steps

??? "1. Receive the offer"
    From the Issuer portal (QR on screen) or by email (with QR and *Open in Wallet* button).

    === "QR on screen"
        ![The QR offer from the Issuer portal](../../assets/img/users/wallet-eudiw/issuer-qr-ui.png){ width="400" }

        To scan the QR from the wallet: tap the *QR Scan* tab in the bottom navigation bar.

        ![Bottom navigation bar of the wallet](../../assets/img/users/wallet-eudiw/nav-bottom-qr.png){ width="240" }

        Point the camera at the QR code. It is also possible to enter the URL manually in the text field.

        ![QR scanning screen in the wallet](../../assets/img/users/wallet-eudiw/wallet-scan.png){ width="240" }

        ??? warning "QR validity"
            - **QR on screen**: valid for **10 minutes** from when it appears on screen.
          
    === "By email"
        ![The QR offer and link from the email](../../assets/img/users/wallet-eudiw/email-qr.png){ width="400" }

        To scan the QR from the wallet: tap the *QR Scan* tab in the bottom navigation bar.

        ![Bottom navigation bar of the wallet](../../assets/img/users/wallet-eudiw/nav-bottom-qr.png){ width="240" }

        Point the camera at the QR code. It is also possible to enter the URL manually in the text field.

        ![QR scanning screen in the wallet](../../assets/img/users/wallet-eudiw/wallet-scan.png){ width="240" }

        ??? warning "QR validity"
            - **QR by email**: valid for **10 minutes** from the moment it is sent,
              not from when the email is opened. If the email takes time to arrive or is opened
              later, the available time may be shorter.

              If the QR has expired, use the **Request a new one** link in the email.

??? "2. The wallet shows the offer"
    The wallet shows the issuer, the credential type and the attributes included. At the bottom a countdown timer appears: tap **Accept** before it expires.

    ![New credential screen with timer — accept credential](../../assets/img/users/wallet-eudiw/new-vc-offer.png){ width="320" }


    ??? warning "The credential is not added if it is cancelled or the time expires"
        If **Cancel** is tapped or the timer reaches zero without confirming, the offer is rejected and the credential is not stored in the wallet.
        
        ![Timer screen and Cancel button in the credential offer](../../assets/img/users/wallet-eudiw/new-vc-timer-cancel.png){ width="320" }

        ![Rejected offer screen — credential not added](../../assets/img/users/wallet-eudiw/new-vc-reject.png){ width="320" }

        If the time expires, tap the **Request a new one** link in the email received: the system resends the offer email from which it is possible to **scan the QR** or tap **Open in Wallet** again.

        ![Temporary incident email with Request a new one link](../../assets/img/users/wallet-eudiw/email-resend.png){ width="320" }

        <div style="text-align: right" markdown>
          [Troubleshooting :material-arrow-right:](../troubleshooting.md){ .md-button }
        </div>

??? "3. Confirm with the device passkey"
    After confirming with the passkey (fingerprint, facial recognition or PIN), the credential is stored in the wallet.

??? "4. The credential appears in the *Credentials* tab"
    ![Credential added to the credentials tab](../../assets/img/users/wallet-eudiw/new-vc.png){ width="240" }

## Common errors

See [troubleshooting](../troubleshooting.md) if the offer does not open, the QR does not scan or an error appears after confirmation.
