# Presentar credenciales — EUDIW

Cómo compartir credenciales con un Verifier de forma controlada y selectiva.

## Qué es la divulgación selectiva

Las credenciales en formato SD-JWT permiten **revelar solo los atributos necesarios** para cada solicitud, sin exponer el resto de la información de la credencial.

## Pasos

1. **El Verifier presenta una solicitud**: QR o URL.

    Para escanear el QR desde el wallet: pulsar la pestaña *Escaneo QR* en la barra de navegación inferior.

    ![Barra de navegación inferior del wallet](../../assets/img/users/wallet-eudiw/nav-bottom-qr.png){ width="240" }

    Apuntar la cámara al código QR. También es posible pegar la URL manualmente en el campo de texto.

    ![Pantalla de escaneo de QR en el wallet](../../assets/img/users/wallet-eudiw/wallet-scan.png){ width="240" }

2. **El Wallet muestra las credenciales que coincide con la solicitud**.

    ![Pantalla Solicitud de Credencial](../../assets/img/users/wallet-eudiw/vc-select.png){ width="240" }

3. **Seleccionar la credencial a presentar**: pulsar la tarjeta correspondiente.

4. **El wallet firma y envía la presentación**: el dispositivo solicita autenticación (passkey o biometría) como parte del proceso de firma.

    ![Diálogo de confirmación — ¿Enviar Credencial?](../../assets/img/users/wallet-eudiw/vc-select-confirm.png){ width="240" }

## Privacidad

- Solo se envía lo que se confirma.
- El wallet registra cada presentación en *Actividad*, accesible desde **Ajustes → Actividad**.
- Es posible denegar la presentación sin penalización. Si se desea reintentar, escanear de nuevo el QR o enlace del Verifier; en algunos casos puede ser necesario solicitar uno nuevo.
