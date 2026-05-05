# Gestionar emisiones

<!-- TODO: contenido pendiente -->

Consultar y operar sobre credenciales ya emitidas.

## Listado de emisiones

La pestaña *Credenciales* del portal lista todas las emisiones lanzadas con:

- Destinatario.
- Tipo de credencial.
- Estado (BORRADOR / VÁLIDO / EXPIRADO / REVOCADO / RETIRADO).
- Fecha.

## Acciones

- **Reenviar oferta**: si el destinatario no la aceptó a tiempo, puedes generar una nueva.
- **Retirar**: aparece en estado *BORRADOR*; cancela la oferta antes de que el destinatario la acepte.

    ![Credencial en estado BORRADOR con botón Retirar](../../assets/img/users/issuer-portal/withdraw.png){ width="560" }

- **Revocar**: aparece en estado *VÁLIDO*; marca la credencial como revocada en el Status List público (cualquier verifier podrá comprobarlo).

    ![Credencial en estado VÁLIDO con botón Revocar](../../assets/img/users/issuer-portal/revoke.png){ width="560" }

- **Renovar**: emite una nueva versión con periodo de validez extendido.

<!-- TODO: capturas del listado y acciones -->
