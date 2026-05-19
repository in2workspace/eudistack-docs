# Gestionar emisiones

Consultar y operar sobre credenciales ya emitidas.

---

## Lista de credenciales emitidas

??? "Ver listado"
    La pantalla de gestión lista todas las emisiones lanzadas con:

    - Sujeto (destinatario).
    - Tipo de credencial.
    - Última actualización.
    - Estado (BORRADOR / VÁLIDO / EXPIRADO / RETIRADO / REVOCADO).

    ![Lista de credenciales emitidas en issuer](../../assets/img/admin/issuer-portal/list-credential.png){ width="640" }

    Pulsar una fila de la tabla para acceder al [detalle de esa emisión](#detalles-de-la-credencial).

---

## Detalles de la credencial

??? "Ver detalle"
    Accesible pulsando cualquier fila de la [lista de credenciales emitidas](#lista-de-credenciales-emitidas).

    Muestra toda la información de la credencial seleccionada: atributos, estado actual, fechas de emisión y caducidad, e historial de cambios de estado.

    ![Pantalla de detalle de una emisión](../../assets/img/admin/issuer-portal/credential-detail.png){ width="640" }

    !!! note "Secciones Emisor y Estado de la credencial"
        Las secciones **Emisor** y **Estado de la credencial** aparecen únicamente cuando el destinatario ha aceptado la credencial desde su wallet.

---

## Estados durante el flujo de emisión

??? "Ver estados"
    El estado de cada emisión es visible tanto en la tabla del listado como en la pantalla de detalle de la credencial. Cada estado determina qué acciones están disponibles; consultar el apartado [Acciones](#acciones) para más información.

    - ![Estado DRAFT](../../assets/img/admin/issuer-portal/draft.png){ width="120" style="vertical-align: middle;" }: oferta enviada, todavía no aceptada por el destinatario.
    - ![Estado EMITIDO](../../assets/img/admin/issuer-portal/issued.png){ width="120" style="vertical-align: middle;" }: el destinatario ha aceptado la credencial pero todavía no ha empezado su periodo de validez.
    - ![Estado WITHDRAWN](../../assets/img/admin/issuer-portal/withdrawn.png){ width="120" style="vertical-align: middle;" }: el emisor ha cancelado la oferta antes de que el destinatario la aceptara.
    - ![Estado VALID](../../assets/img/admin/issuer-portal/valid.png){ width="120" style="vertical-align: middle;" }: el destinatario ha aceptado la credencial y está en su wallet en periodo de validez.
    - ![Estado REVOKED](../../assets/img/admin/issuer-portal/revoked.png){ width="120" style="vertical-align: middle;" }: el emisor ha revocado la credencial explícitamente; deja de ser válida aunque no haya alcanzado su fecha de caducidad.
    - ![Estado EXPIRADO](../../assets/img/admin/issuer-portal/expired.png){ width="120" style="vertical-align: middle;" }: la credencial ha superado su fecha de validez.

---

## Acciones

??? "Ver acciones"
    Las acciones se realizan desde la pantalla de detalle de la credencial. Solo algunas acciones están disponibles según el estado de la emisión.

    - **Retirar**: aparece el botón de esta acción cuando la credencial está en estado **BORRADOR**. Cancela la oferta antes de que el destinatario la acepte. Si la oferta no llegó o algún dato es incorrecto, retirar la emisión actual y emitir una nueva credencial con los datos correctos.

        ![Credencial en estado DRAFT con botón Retirar](../../assets/img/admin/issuer-portal/withdraw.png){ width="560" }

    - **Revocar**: aparece el botón de esta acción cuando la credencial está en estado **VÁLIDO**. Marca la credencial como revocada en el Status List público (cualquier verifier podrá comprobarlo).

        ![Credencial en estado VALID con botón Revocar](../../assets/img/admin/issuer-portal/revoke.png){ width="560" }