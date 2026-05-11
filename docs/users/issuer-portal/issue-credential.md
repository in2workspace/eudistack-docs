# Emitir una credencial

Pasos para emitir una credencial desde el portal.

## Pasos

1. **Iniciar sesión** en el Portal Issuer con la credencial organizativa.

    El portal mostrará un código QR en pantalla o facilita un enlace para pegarlo directamente en el wallet.

    ![Pantalla de inicio de sesión del portal con código QR](../../assets/img/users/issuer-portal/verifier-login.png){ width="640" }

    Abrir el wallet, escanear el QR o copiar y pegar el contenido manualmente.

    ![Pantalla de escaneo de QR en el wallet](../../assets/img/users/issuer-portal/wallet-scan.png){ width="240" }

    !!! note "Más información sobre presentar credenciales desde el wallet"
        - [Presentar credenciales](../wallet-eudiw/present-credentials.md)

    Seleccionar la credencial organizativa y confirmar.

    ![Selección de credencial organizativa en el wallet](../../assets/img/users/issuer-portal/valid-credential.png){ width="320" }

    El portal dará acceso automáticamente.

2. **Pulsar *Nueva credencial***: tras el acceso, el portal muestra la tabla de credenciales emitidas. Pulsar el botón **Nueva credencial** para iniciar el proceso de emisión.

    ![Tabla de credenciales con el botón Nueva credencial](../../assets/img/users/issuer-portal/create-new-credential.png){ width="640" }

3. **Seleccionar el tipo de credencial** a emitir (depende del catálogo configurado para la organización).

    ![Selección del tipo de credencial](../../assets/img/users/issuer-portal/select-credential-type.png){ width="240" }

4. **Rellenar los atributos y seleccionar el método de entrega**: el portal muestra el formulario de emisión. Rellenar los atributos del destinatario (nombre, email y los campos específicos del tipo). En la esquina superior derecha se muestra el selector de **método de entrega**; seleccionar *por email* o *en pantalla* (QR para escaneo inmediato).

    ![Formulario de emisión con atributos y selector de método de entrega](../../assets/img/users/issuer-portal/type-deliver.png){ width="640" }

5. **Confirmar**: pulsar el botón **Crear credencial** para enviar el formulario.

    ![Botón Crear credencial en el formulario de emisión](../../assets/img/users/issuer-portal/click-create-credentail.png){ width="320" }

    El portal muestra un diálogo de confirmación. Pulsar **Aceptar** para completar la emisión.

    ![Diálogo de confirmación de creación de credencial](../../assets/img/users/issuer-portal/confirm-create-credential.png){ width="400" }

    El resultado varía según el método de entrega seleccionado:

    - **Corre electrónico**: el portal muestra una notificación de éxito y en breve el destinatario recibe el correo con la oferta de credencial.

        ![Notificación de credencial creada y enviada por email](../../assets/img/users/issuer-portal/credential-created.png){ width="400" }

    - **Código QR**: el portal muestra un diálogo con el código QR y un botón *Copiar enlace*. El destinatario escanea el QR con el wallet o pega el enlace copiado manualmente.

        ![Diálogo con código QR y enlace para recibir la credencial](../../assets/img/users/wallet-eudiw/issuer-qr-ui.png){ width="400" }

6. **El destinatario acepta la oferta** desde su wallet — momento en el que la credencial queda emitida.

##  Consultar el estado de las emisiones

Para consultar los posibles estados de una credencial, ver [Gestionar emisiones → Estados de las emisiones](manage-issuances.md#estados-de-las-emisiones).