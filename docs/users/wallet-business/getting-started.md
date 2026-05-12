# Primeros pasos — Business Wallet

Primera activación del Business Wallet: verificación del email corporativo, creación del passkey y confirmación del acceso.

## Requisitos

- Dirección de email corporativa.
- Navegador moderno compatible con WebAuthn / Passkey (PRF).
- Conexión a internet.

## Pasos

1. **Acceder al Business Wallet**: navegar a la URL del wallet de la organización.

2. **Introducir el email corporativo**: el wallet solicita una dirección de correo para iniciar el alta.

    ![Pantalla de registro — introducción del email corporativo](../../assets/img/users/wallet-buisiness/wallet-register.png){ width="320" }

3. **Verificar el código OTP <small>*(One-Time Password)*</small>**: el sistema envía un código de 6 dígitos al email introducido. Introducir el código en el wallet para confirmar la identidad y completar el alta de cuenta.

    <figure markdown style="margin-left: 0;">
      ![Correo de verificación con código OTP de 6 dígitos](../../assets/img/users/wallet-buisiness/email-code.png){ width="400" }
      <figcaption> email - con código de 6 dígitos</figcaption>
    </figure>
    
    ![Pantalla de verificación — introducción del código OTP en el wallet](../../assets/img/users/wallet-buisiness/wallet-code-input.png){ width="320" }

4. **Crear el passkey**: el wallet solicita registrar un passkey con la biometría o PIN del dispositivo. El passkey vincula el dispositivo a la cuenta y permite el acceso futuro sin necesidad de repetir el proceso OTP <small>*(One-Time Password)*</small>.

    ![Pantalla de creación del passkey](../../assets/img/users/wallet-buisiness/create-passkey.png){ width="320" }

5. **Acceso confirmado**: ya es posible recibir y presentar credenciales desde el Business Wallet.

    ![Pantalla de confirmación de acceso al Business Wallet](../../assets/img/users/wallet-buisiness/access-confirmation.png){ width="320" }

## Siguiente paso

[Gestionar dispositivos →](devices.md)
