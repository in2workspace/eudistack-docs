# Presentar credenciales — EUDIW

<!-- TODO: contenido pendiente -->

Cómo compartir credenciales con un Verifier de forma controlada y selectiva.

## Qué es la divulgación selectiva

Las credenciales SD-JWT permiten **revelar solo los atributos necesarios**. Por ejemplo, para demostrar que eres mayor de edad sin revelar tu fecha de nacimiento exacta.

## Flujo paso a paso

1. **El Verifier presenta una solicitud**: QR o redirección.
2. **El wallet te muestra qué credencial necesitas presentar**: verás la credencial de tu wallet que encaja con lo que piden.

    ![Pantalla Solicitud de Credencial](../../assets/img/users/wallet-eudiw/vc-select.png){ width="240" }

3. **Selecciona la credencial que deseas presentar**: pulsa la tarjeta correspondiente.
4. **El wallet firma y envía la presentación**: al confirmar, el dispositivo solicita tu autenticación (passkey o biometría) como parte del proceso de firma.

    ![Diálogo de confirmación — ¿Enviar Credencial?](../../assets/img/users/wallet-eudiw/vc-select-confirm.png){ width="240" }

<!-- TODO: insertar capturas del flujo OID4VP desde la perspectiva del usuario -->

## Privacidad

- Solo se envía lo que confirmas.
- El wallet registra cada presentación en *Actividad* (puedes consultarla en cualquier momento).
- Puedes denegar la presentación sin penalización.
