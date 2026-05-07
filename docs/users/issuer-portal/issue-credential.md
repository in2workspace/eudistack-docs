# Emitir una credencial

Pasos para emitir una credencial desde el portal.

## Pasos

1. **Iniciar sesión** en el Portal Issuer con la credencial organizativa.

    El portal mostrará un código QR en pantalla o facilita un enlace para pegarlo directamente en el wallet.

    ![Pantalla de inicio de sesión del portal con código QR](../../assets/img/users/issuer-portal/verifier-login.png){ width="560" }

    Abrir el wallet, escanear el QR o copiar y pegar el contenido manualmente.

    ![Pantalla de escaneo de QR en el wallet](../../assets/img/users/issuer-portal/wallet-scan.png){ width="240" }

    Seleccionar la credencial organizativa y confirmar.

    ![Selección de credencial organizativa en el wallet](../../assets/img/users/issuer-portal/valid-credential.png){ width="320" }

    El portal dará acceso automáticamente.

2. **Seleccionar el tipo de credencial** a emitir (depende del catálogo configurado para la organización).

    ![Selección del tipo de credencial](../../assets/img/users/issuer-portal/create-credential.png){ width="240" }

3. **Rellenar los atributos** del destinatario (nombre, email, atributos específicos del tipo). Seleccionar también el método de entrega: *por email* o *en pantalla* (QR para escaneo inmediato).

    ![Selección del método de entrega](../../assets/img/users/issuer-portal/type-deliver.png){ width="240" }

4. **Confirmar**: el portal genera una oferta y la entrega según el método seleccionado.

5. **El destinatario acepta la oferta** desde su wallet — momento en el que la credencial queda emitida.

## Estados durante el flujo

- **BORRADOR**: oferta enviada, todavía no aceptada por el destinatario.
- **VÁLIDO**: el destinatario ha aceptado la credencial y está en su wallet en periodo de validez.
- **EXPIRADO**: la credencial ha superado su fecha de validez.
- **RETIRADO**: el emisor ha cancelado la oferta antes de que el destinatario la aceptara.
- **REVOCADO**: el emisor ha revocado la credencial explícitamente; deja de ser válida aunque no haya alcanzado su fecha de caducidad.