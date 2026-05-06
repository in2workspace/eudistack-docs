# Solución de problemas — Usuarios

Esta sección recoge los problemas más frecuentes y cómo resolverlos. Si el problema no aparece, consulta [soporte](../support.md).

## Wallet

### El QR de la oferta no se abre

- **Síntoma**: al escanear el QR no ocurre ninguna acción, o aparece un error.
- **Causa probable**: el QR ha caducado, la oferta ya se consumió o el wallet no es del tenant correcto.
- **Solución**: solicitar una nueva oferta al emisor; verificar que el wallet pertenece al mismo tenant que el emisor (mismo dominio).

### Error al crear el passkey

- **Síntoma**: al activar el wallet, la creación del passkey falla.
- **Causa probable**: el navegador o sistema operativo no soporta WebAuthn con extensión PRF.
- **Solución**: actualiza el navegador y SO a las últimas versiones. En iOS exige iOS 17+; en Android, Chrome 122+.

### La credencial aparece como revocada

- **Síntoma**: al presentarla a un Verifier, este informa de que la credencial está revocada.
- **Causa probable**: el emisor revocó la credencial (cambio de rol, baja en la organización).
- **Solución**: antes de contactar con el emisor, comprobar el estado actual de la credencial: abrir el detalle de la credencial en el wallet y pulsar **Verificar credencial**.

    ![Pantalla de verificación de credencial](../../assets/img/users/troubleshooting/revoke.png){ width="240" }

    Si la credencial aparece como revocada, contactar con el emisor; es necesario solicitar una nueva credencial.

## Portal Issuer

### Error al iniciar sesión

- **Síntoma**: el login con credencial verificable falla.
- **Causa probable**:  credencial corporativa ha caducado o ha sido revocada.
- **Solución**: contactar con el administrador de la organización para que emita una nueva.

### El destinatario no recibe la oferta

- **Síntoma**: la credencial se emite pero el destinatario no la recibe.
- **Causa probable**: email filtrado como spam o dirección de correo incorrecta.
- **Solución**:
    - **Email en spam**: pedir al destinatario que revise la carpeta de spam.
    - **Dirección incorrecta**: en el portal Issuer, abrir el detalle de la emisión, usar **Retirar** para cancelar y emitir una nueva credencial con la dirección correcta.

        ![Credencial en estado BORRADOR con botón Retirar](../../assets/img/users/issuer-portal/withdraw.png){ width="560" }

## Verifier

### La presentación no llega

- **Síntoma**: el usuario confirma en su wallet pero el verifier no recibe nada.
- **Causa probable**: timeout de la sesión o problemas de red.
- **Solución**: reiniciar la solicitud. Si persiste, abrir un ticket con el ID de sesión.

---

## ¿Sigues sin resolverlo?

[Contacta con soporte](../support.md). Incluye:

- Wallet/aplicación afectada.
- Captura del error si la hay.
- Hora aproximada del incidente.
- La organización (tenant).
