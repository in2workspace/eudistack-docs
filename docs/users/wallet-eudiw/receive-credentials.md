# Recibir credenciales — EUDIW

Cómo aceptar una credencial verificable emitida por un Issuer.

## Métodos disponibles

- **Escaneo de QR**: el Issuer presenta un QR en pantalla; escanearlo desde el wallet.
- **Por email**: se recibe un correo con un código QR y un botón *Abrir en Wallet*; escanear el QR o pulsar el botón para añadir la credencial al wallet.

## Pasos

1. **Recibir la oferta**: desde el portal del Issuer (QR en pantalla) o por email (con QR y botón *Abrir en Wallet*).

    <figure markdown style="display: table; margin-left: 0;">
      ![La oferta de QR por el portal del Issuer](../../assets/img/users/wallet-eudiw/issuer-qr-ui.png){ width="400" }
      <figcaption>Portal del Issuer — QR en pantalla</figcaption>
    </figure>

    <figure markdown style="display: table; margin-left: 0;">
      ![La oferta de QR y enlace por el correo](../../assets/img/users/wallet-eudiw/email-qr.png){ width="400" }
      <figcaption>Correo — QR y botón Abrir en Wallet</figcaption>
    </figure>

2. **El wallet muestra la oferta**: escanear el QR o pulsar el botón del email; el wallet muestra el emisor, el tipo de credencial y los atributos incluidos.

    ![Pantalla Nueva credencial — aceptar credencial](../../assets/img/users/wallet-eudiw/new-vc-offer.png){ width="320" }

3. **Confirmar con el passkey del dispositivo**: tras la confirmación, la credencial se almacena en el wallet.

4. **La credencial aparece en la pestaña *Credenciales***.

    ![Credencial añadida a la pestaña credenciales](../../assets/img/users/wallet-eudiw/new-vc.png){ width="320" }

## Errores comunes

Consulta [solución de problemas](../troubleshooting.md) si la oferta no se abre, el QR no escanea o aparece un error tras la confirmación.
