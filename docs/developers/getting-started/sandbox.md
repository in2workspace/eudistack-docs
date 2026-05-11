# Sandbox

<!-- TODO: rellenar URLs reales del sandbox y proceso de alta -->

EUDIStack ofrece un entorno **sandbox** público para que pruebes la integración antes de pasar a un tenant productivo.
Permite validar flujos completos de emisión y verificación utilizando OID4VCI y OID4VP con datos sintéticos y credenciales de prueba.

## Qué incluye el sandbox

- Tenant `sandbox` con Issuer, Verifier y Wallet operativos.
- Flujo completos de emisión y presentación de credenciales.
- Catálogo de credenciales de prueba (LEARCredentialEmployee, etc.).
- Datos sintéticos para pruebas funcionales.
- Sin SLA — pensado solo para desarrollo.

## Cómo solicitar acceso
El acceso al sandbox se gestiona manualmente por el equipo de EUDIStack.

1. Contacta con el equipo a través de la página de soporte.
2. Indica:
    - Nombre de la organización.
    - Caso de uso.
    - Entorno que quieres integrar (Issuer, Verifier o Wallet).
3. Recibirás:
    - Código o credenciales de acceso al entorno sandbox.
    - Información básica de configuración.
    - Endpoints necesarios para comenzar la integración.

## URLs del entorno de sandbox

| Servicio | URL |
|---|---|
| Issuer API | `https://sandbox-stg.eudistack.net/issuer` |
| Verifier API | `https://sandbox-stg.eudistack.net/verifier` |
| Wallet PWA | `https://sandbox-stg.eudistack.net/wallet` |

## Próximos pasos

Una vez tengas acceso al sandbox, continúa con [tu primera integración](./first-integration.md)
