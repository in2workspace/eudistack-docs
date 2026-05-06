# Verificación remota

Verificación online de credenciales: el verifier solicita una credencial al usuario, quien la presenta desde su wallet sin necesidad de estar en el mismo lugar.

## Pasos

1. **El verifier muestra un código QR** para que el usuario lo escanee con su wallet, o facilita el enlace para pegarlo directamente en el wallet.

    ![Pantalla del verifier con código QR](../../assets/img/users/verifire/remote.png){ width="240" }

2. **El usuario escanea el QR** o pega el enlace en su wallet. El wallet muestra automáticamente la credencial solicitada.

    ![Pantalla Solicitud de Credencial](../../assets/img/users/wallet-eudiw/vc-select.png){ width="240" }

3. **El usuario selecciona la credencial** y confirma el envío con su passkey.

    ![Diálogo de confirmación — ¿Enviar Credencial?](../../assets/img/users/wallet-eudiw/vc-select-confirm.png){ width="240" }

4. **El verifier recibe y comprueba la credencial** automáticamente y muestra el resultado.

## Casos típicos

- Registro de clientes online (KYC).
- Acceso a portales que exigen credencial corporativa.
- Validación de cualificación profesional online.