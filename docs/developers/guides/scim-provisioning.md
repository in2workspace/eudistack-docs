# SCIM Provisioning

EUDIStack soporta aprovisionamiento automático de usuarios mediante SCIM 2.0.
Cuando tu sistema corporativo de identidad o RRHH crea, actualiza o desactiva empleados, EUDIStack sincroniza automáticamente esos cambios y ejecuta el ciclo de vida asociado a las credenciales digitales.

SCIM provisioning permite automatizar:

- Emisión de credenciales durante el onboarding de empleados.
- Renovación de credenciales cuando cambian atributos del empleado.
- Revocación automática de credenciales durante el offboarding.

EUDIStack actúa como **SCIM Service Provider** y recibe eventos push desde tu IdP o HRIS corporativo.

??? tip "¿Cuándo usar esta guía?"
    Utiliza esta integración si tu organización:

    - Gestiona el ciclo de vida de empleados desde un IdP o sistema de RRHH.
    - Quiere automatizar flujos de emisión de credenciales.
    - Necesita onboarding y offboarding sin intervención manual.
    - Utiliza plataformas como Okta, Microsoft Entra ID o Workday.

??? info "Requisitos previos"
    Antes de configurar SCIM provisioning asegúrate de disponer de:

    - Un tenant activo en EUDIStack.
    - Acceso administrador a tu IdP o HRIS.
    - Un token SCIM proporcionado por EUDIStack.
    - Al menos una plantilla de credencial configurada en el Issuer.

---

## Base URL

Cada tenant dispone de su propio endpoint SCIM.

```txt
https://scim.<tenant>.eudistack.net/scim/v2
```

> **Nota sobre el patrón de URL:** El endpoint SCIM usa el subdominio `scim.<tenant>`, que difiere del patrón estándar multi-tenant `{tenant}-stg.eudistack.net` utilizado por los servicios Issuer, Verifier y Wallet. Consulta con el equipo de EUDIStack el endpoint exacto para tu entorno (STG o producción) al solicitar acceso.

Las peticiones SCIM deben autenticarse mediante **bearer token**.

---

## Recursos y ciclo de vida

=== "Recursos soportados"
    El soporte de operaciones PATCH y Groups puede variar según la configuración del tenant y la versión desplegada.

    | Resource | Endpoints |
    |---|---|
    | Users | `GET/POST /scim/v2/Users` |
    | User by ID | `GET/PUT/PATCH/DELETE /scim/v2/Users/{id}` |
    | Groups | `GET/POST /scim/v2/Groups` |


=== "Comportamiento del ciclo de vida de credenciales"
    | Evento SCIM | Comportamiento |
    |---|---|
    | `POST /Users` | Provisiona el usuario y dispara emisión automática |
    | `PUT /Users/{id}` | Renueva la credencial con atributos actualizados |
    | `DELETE /Users/{id}` | Revoca la credencial activa |

=== "Modos de entrega"
    El comportamiento de entrega de credenciales es configurable por tenant.

    | Modo | Descripción |
    |---|---|
    | Email | Envía un enlace de oferta de credencial o email de onboarding al empleado |
    | Direct | Devuelve la credencial emitida directamente al sistema llamante |

---

## Operaciones SCIM


=== "Crear usuario"

    ```http
    POST /scim/v2/Users
    Authorization: Bearer <token>
    Content-Type: application/scim+json
    ```

    ```json
    {
      "userName": "ana.garcia",
      "active": true,
      "name": {
        "givenName": "Ana",
        "familyName": "Garcia"
      },
      "emails": [
        { "primary": true, "value": "ana.garcia@example.com" }
      ]
    }
    ```

    **Resultado:** usuario provisionado → emisión automática de credencial → entrega según configuración del tenant → evento de auditoría registrado.

=== "Actualizar usuario"

    ```http
    PUT /scim/v2/Users/2819c223-7f76-453a-919d-413861904646
    Authorization: Bearer <token>
    Content-Type: application/scim+json
    ```

    ```json
    {
      "userName": "ana.garcia",
      "active": true,
      "name": {
        "givenName": "Ana",
        "familyName": "Garcia"
      },
      "emails": [
        { "primary": true, "value": "ana.garcia@new-domain.example" }
      ]
    }
    ```

    **Resultado:** credencial anterior revocada → nueva credencial emitida con atributos actualizados → evento de auditoría registrado.

=== "Eliminar usuario"

    ```http
    DELETE /scim/v2/Users/2819c223-7f76-453a-919d-413861904646
    Authorization: Bearer <token>
    ```

    **Resultado:** credencial activa revocada → Status List actualizada automáticamente → evento de auditoría registrado.

---

## Configuración de proveedores

=== "Okta"

    1. Crea una nueva aplicación SCIM en Okta.
    2. Configura la Base URL SCIM de tu tenant.
    3. Configura el bearer token proporcionado por EUDIStack.
    4. Activa el provisioning de usuarios.
    5. Configura los mappings de atributos.

=== "Microsoft Entra ID"

    1. Crea una nueva Enterprise Application.
    2. Activa automatic provisioning.
    3. Selecciona SCIM como modo de provisioning.
    4. Configura la Base URL SCIM del tenant.
    5. Configura el provisioning secret token.
    6. Configura los mappings de atributos.

---

??? question "Troubleshooting"

    | Síntoma | Posible causa | Resolución |
    |---|---|---|
    | `401 Unauthorized` | Token inválido o expirado | Verificar configuración del bearer token |
    | Usuario no provisionado | Mapping requerido ausente | Revisar atributos obligatorios |
    | Credential no emitida | No existe plantilla de credencial | Revisar configuración del Issuer |
    | Revocación no visible | Retraso de propagación | Esperar actualización de la Status List |

??? warning "Consideraciones de seguridad"
    - Utiliza siempre HTTPS para endpoints SCIM.
    - Rota periódicamente los tokens de provisioning.
    - Restringe acceso SCIM a sistemas corporativos confiables.
    - Monitoriza regularmente los eventos de auditoría.
