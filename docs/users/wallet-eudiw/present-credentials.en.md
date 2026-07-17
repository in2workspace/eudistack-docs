# Present credentials

A Verifier is a service or portal that requests and validates credentials, for example, to verify a user's identity, allow them to log in or attest to certain attributes.

---

## Steps

??? "1. The Verifier presents a request (QR or URL)"
    The Verifier shows a QR code or a URL to start the verification process.

    **Example 1:**

    ![Verifier screen — remote verification](../../assets/img/users/wallet-eudiw/remote.png){ width="240" }

    **Example 2:**

    ![Verifier screen — proximity verification](../../assets/img/users/wallet-eudiw/proximity.png){ width="480" }

??? "2. Scan the QR with the wallet or use a URL"
    Tap the *QR Scan* tab in the bottom navigation bar.

    ![Bottom navigation bar of the wallet](../../assets/img/users/wallet-eudiw/nav-bottom-qr.png){ width="240" }

    **Option A — QR Scan:** point the camera at the Verifier's QR code.

    **Option B — Manual URL:** paste the URL directly into the text field on the same screen.

    ![QR scanning screen in the wallet](../../assets/img/users/wallet-eudiw/wallet-scan.png){ width="240" }

??? "3. The Wallet shows the credentials that match the request"
    ![Credential Request screen](../../assets/img/users/wallet-eudiw/vc-select.png){ width="240" }

??? "4. Select the credential to present"
    Tap the corresponding card.

    ![Confirmation dialog — Send Credential?](../../assets/img/users/wallet-eudiw/vc-select-confirm.png){ width="240" }

??? "5. The wallet signs and sends the presentation"
    Confirm with the device passkey (fingerprint, facial recognition or PIN).

---

## Privacy

- Only what is confirmed is sent.
- The wallet records each presentation in *Activity*, accessible from **[Settings → Activity](../wallet-eudiw/settings.md#actividad)**.
- It is possible to deny the presentation without penalty. If you want to retry, scan the Verifier's QR or link again; in some cases it may be necessary to request a new one.
