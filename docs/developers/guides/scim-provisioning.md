# SCIM Provisioning

<!-- TODO: ejemplos concretos con Okta y Entra ID -->

EUDIStack soporta aprovisionamiento automático de usuarios mediante SCIM 2.0.
Cuando tu sistema corporativo de identidad o RRHH crea, actualiza o desactiva empleados, EUDIStack sincroniza automáticamente esos cambios y ejecuta el ciclo de vida asociado a las credenciales digitales.

Esto permite automatizar:

- Emisión de credenciales en altas de empleados.
- Renovación de credenciales cuando cambian de atributos.
- Revocación automática en bajas o desactivaciones.

EUDIStack actúa como **SCIM Service Provider** y recibe eventos push desde tu IdP o HRIS corporativo.

## Cuándo usar esta guía

Esta guía está pensada para organizaciones que:

- Gestionan empleados desde un sistema corporativo de identidad.
- Quieren automatizar el onboarding/offboarding.
- Necesitan emisión de credenciales digitales.
- Ya utilizan plataformas como Okta, Entra ID o Workday.

## Requisitos previos
Antes de configurar SCIM necesitas:

- Un tenant activo en EUDIStack.
- Acceso administrador a tu IdP o HRIS.
- Un token SCIM proporcionado por EUDIStack.
- Al menos una plantilla de credencial configurada en el Issuer.

## Base URL

<!-- TODO: validar URL -->

Cada tenant dispone de su propio endpoint SCIM.

Ejemplo:

```txt
https://scim.<tenant>.eudistack.net/scim/v2
```

## Endpoints expuestos

EUDIStack expone endpoints compatibles con SCIM 2.0.

| Resource | Endpoints |
|---|---|
| Users | `GET/POST /scim/v2/Users` |
| User by ID | `GET/PUT/PATCH/DELETE /scim/v2/Users/{id}` |
| Groups | `GET/POST /scim/v2/Groups` |

## Ciclo de vida de credenciales
EUDIStack interpreta los eventos SCIM como operaciones sobre el ciclo de vida de credenciales.

| Evento SCIM | Comportamiento |
|---|---|
| `POST /Users` | Provisiona el usuario y dispara emisión automática |
| `PUT /Users/{id}` | Renueva la credencial con atributos actualizados |
| `DELETE /Users/{id}` | Revoca la credencial activa |


<!-- TODO: detallar atributos requeridos vs opcionales y mapping a credenciales -->

## Configuración en tu IdP

1. **Crea una app SCIM** apuntando a `https://scim.<tenant>.eudistack.net/scim/v2`.
2. **Configura el bearer token** que te entregaremos.
3. **Define el mapping de atributos** entre tu directorio y EUDIStack.

## Comportamiento

- **Crear usuario** → se provisiona la cuenta y se programa la emisión de credencial.
- **Modificar atributos** → se renueva la credencial con los nuevos valores.
- **Desactivar/eliminar** → se revoca la credencial vigente.
