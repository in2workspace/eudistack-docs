# Dispositivos y claves — Business Wallet

El Business Wallet permite operar desde varios dispositivos. Cada dispositivo se vincula con un passkey distinto.

## Sección Dispositivos

Disponible en **Ajustes → Mis Dispositivos**. Lista todos los passkeys vinculados a la cuenta con la siguiente información:

![Pantalla Mis Dispositivos — detalle de un dispositivo registrado](../../assets/img/users/wallet-buisiness/detail-device.png){ width="320" }

- Nombre del dispositivo.
- Sesiones activas.
- Fecha de registro.

## Acciones disponibles

Desde el detalle de cada dispositivo es posible:

- **Renombrar**: actualiza el nombre del dispositivo para facilitar su identificación.

    ![](../../assets/img/users/wallet-buisiness/rename-device.png){ width="160" }

- **Cerrar sesiones**: cierra todas las sesiones activas en ese dispositivo sin eliminar el passkey. El dispositivo seguirá registrado y podrá usarse para autenticarse de nuevo.

- **Eliminar**: borra el passkey y la vinculación del dispositivo. No será posible autenticarse desde ese dispositivo hasta volver a registrarlo.


!!! warning "Protección del último dispositivo"
    No es posible eliminar el último passkey registrado. Para dar de baja el dispositivo en uso, registrar primero un nuevo dispositivo desde **Ajustes → Mis Dispositivos**.

    ![Error al intentar eliminar el último passkey registrado](../../assets/img/users/wallet-buisiness/not-delete.png){ width="400" }

## Buenas prácticas

- Registrar al menos **dos dispositivos** (principal + respaldo) para evitar perder el acceso.
- Eliminar los dispositivos que ya no estén en uso (móvil antiguo, ordenador devuelto a la empresa).
