# Solución de problemas — Administrador

Esta sección recoge los problemas más frecuentes del Portal Issuer y cómo resolverlos. Si el problema no aparece, consulta [soporte](../support.md).

## Portal Issuer

### Error al iniciar sesión

- **Síntoma**: el login con credencial verificable falla.
- **Causa probable**: la credencial corporativa ha caducado o ha sido revocada.
- **Solución**: contactar con el administrador de la organización para que emita una nueva.

### El destinatario no recibe la oferta

- **Síntoma**: la credencial se emite pero el destinatario no la recibe.
- **Causa probable**: email filtrado como spam o dirección de correo incorrecta.
- **Solución**:
    - **Email en spam**: pedir al destinatario que revise la carpeta de spam.
    - **Dirección incorrecta**: en el portal Issuer, abrir el detalle de la emisión, usar **Retirar** para cancelar y emitir una nueva credencial con la dirección correcta.

        ![Credencial en estado DRAFT con botón Retirar](../assets/img/admin/issuer-portal/withdraw.png){ width="560" }

---

## ¿Sigues sin resolverlo?

[Contacta con soporte](../support.md). Incluye:

- Captura del error si la hay.
- Hora aproximada del incidente.
- La organización (tenant).
