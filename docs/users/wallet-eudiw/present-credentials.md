# Presentar credenciales — EUDIW

Cómo compartir credenciales con un Verifier de forma controlada y selectiva.

## Qué es la divulgación selectiva

Las credenciales en formato SD-JWT permiten **revelar solo los atributos necesarios** para cada solicitud, sin exponer el resto de la información de la credencial.

## Pasos

1. **El Verifier presenta una solicitud**: QR o redirección.

2. **El wallet muestra la credencial que coincide con la solicitud**.

    ![Pantalla Solicitud de Credencial](../../assets/img/users/wallet-eudiw/vc-select.png){ width="240" }

3. **Seleccionar la credencial a presentar**: pulsar la tarjeta correspondiente.

4. **El wallet firma y envía la presentación**: el dispositivo solicita autenticación (passkey o biometría) como parte del proceso de firma.

    ![Diálogo de confirmación — ¿Enviar Credencial?](../../assets/img/users/wallet-eudiw/vc-select-confirm.png){ width="240" }

## Privacidad

- Solo se envía lo que se confirma
- El wallet registra cada presentación en *Actividad*, accesible desde **Ajustes → Actividad**.
- Es posible denegar la presentación sin penalización.
