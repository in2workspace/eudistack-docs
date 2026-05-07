# Primeros pasos — EUDIW

Primera configuración del Wallet EUDIW: instalación como PWA <small>*(aplicación web que funciona como una app instalada)*</small>, registro del passkey y verificación de acceso.

## Requisitos

- Navegador moderno (Chrome, Edge, Safari, Firefox actualizado).
- Dispositivo compatible con WebAuthn / Passkey (PRF).
- Conexión a internet para el alta inicial.

## Pasos

1. **Abrir el wallet**: navegar a la URL del wallet de la organización.

2. **Instalar como PWA** *(opcional)*: el wallet ofrece instalar la aplicación en el dispositivo. La instalación permite acceder al wallet como una app nativa, sin necesidad de abrir el navegador cada vez. Es posible continuar sin instalar.

     - **iOS (Safari)**: al abrir el wallet en Safari, el sistema redirige automáticamente a la pantalla de instalación. Seguir el flujo indicado en pantalla para completar la instalación.

          ![Pantalla de instalación automática de la PWA en iOS](../../assets/img/users/wallet-eudiw/ios-pwa-flow.png){ width="320" }

          ![Confirmación de instalación de la PWA en iOS](../../assets/img/users/wallet-eudiw/ios-pwa-icon.png){ width="80" }

        !!! warning "instalar la app antes de registrarse"
            Apple aísla el almacenamiento (credenciales, sesión) de las apps instaladas respecto a Safari. Si el alta se realiza en Safari antes de instalar la app, los datos se perderán al instalar y será necesario registrarse de nuevo. Iniciar siempre el alta desde la app ya instalada evita este problema.

    - **Android (Chrome)**: al abrir el wallet, aparece un botón de instalación en pantalla. Pulsarlo e instalar la aplicación.

        ![Botón de instalación de la PWA en Android](../../assets/img/users/wallet-eudiw/android-pwa-install.png){ width="320" }

        ![Pantalla de confirmación de instalación en Android](../../assets/img/users/wallet-eudiw/android-install-confirm.png){ width="320" }

        ![Icono de la app instalada en Android](../../assets/img/users/wallet-eudiw/android-pwa-icon.png){ width="80" }

    - **Desktop (Chrome)**: al abrir el wallet, aparece un botón de instalación en pantalla. Pulsarlo e instalar la aplicación; se abrirá automáticamente como app independiente del navegador.

        ![Icono de instalación de la PWA en la barra de direcciones](../../assets/img/users/wallet-eudiw/desktop-pwa-install.png){ width="320" }

        ![Pantalla de confirmación de instalación en escritorio](../../assets/img/users/wallet-eudiw/desktop-install-confirm.png){ width="480" }

        ![App del wallet abierta como aplicación independiente](../../assets/img/users/wallet-eudiw/desktop-open-app.png){ width="480" }

    !!! note "Instalar desde Ajustes"
        También es posible instalar la PWA desde **Ajustes** en cualquier momento.
        
        ![Opción de instalación de la PWA en Ajustes](../../assets/img/users/wallet-eudiw/install-app.png){ width="160" }

3. **Crear el passkey**: el wallet solicita registrar un passkey con la biometría o PIN del dispositivo.

    ![Pantalla Crear passkey — Crear Wallet](../../assets/img/users/wallet-eudiw/create-passkey.png){ width="320" }

4. **Acceso confirmado**: ya es posible recibir y presentar credenciales desde el wallet.

!!! note "Ajustes opcionales"
    Desde **Ajustes** es posible configurar el idioma, el modo oscuro y el desenfoque de privacidad en cualquier momento, sin necesidad de repetir el alta.

    ![Ajustes de idioma y privacidad](../../assets/img/users/wallet-eudiw/confirm-config.png){ width="320" }

## Siguiente paso

[Recibir la primera credencial →](receive-credentials.md)
