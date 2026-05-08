# Operativa diaria — Business Wallet

Las operaciones del día a día (recibir, presentar y gestionar credenciales) siguen el mismo flujo que en el  [Wallet EUDIW](../wallet-eudiw/index.md). La diferencia principal es que las credenciales se almacenan en el servidor de la organización y son accesibles desde cualquier dispositivo registrado en la cuenta.

## Recibir credenciales

Mismo flujo que en EUDIW. Consulta [Recibir credenciales (EUDIW)](../wallet-eudiw/receive-credentials.md).

## Presentar credenciales

Mismo flujo que en EUDIW. Consulta [Presentar credenciales (EUDIW)](../wallet-eudiw/present-credentials.md).

## Gestionar credenciales

Mismo flujo que en EUDIW. Consulta [Gestionar credenciales (EUDIW)](../wallet-eudiw/manage-credentials.md).

## Diferencias prácticas

- Cada confirmación pasa por una autenticación reforzada (passkey + servidor).
- Si tu passkey caduca o el servidor de claves no está accesible, el wallet mostrará un error explícito y no permitirá completar la operación.