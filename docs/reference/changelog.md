# Changelog

Aquí encontrarás un resumen de las novedades y mejoras más relevantes de la plataforma EUDIStack, organizado por versión y componente. Si necesitas el detalle técnico completo de cada componente, puedes consultar los changelogs individuales en GitHub.

---

## En curso

> Próximas mejoras en preparación. Se publicarán aquí cuando estén disponibles.

---

## 2026 - Q2

> _Pendiente de publicar._

---

## Componentes de la plataforma

### Emisor de credenciales (`eudistack-core-issuer`)

#### Versiones 3.x — Plataforma multi-organización y emisión avanzada

Esta familia de versiones transformó el emisor en una plataforma robusta capaz de servir a múltiples organizaciones de forma simultánea, con soporte para entornos empresariales exigentes y despliegues en la nube.

**Novedades principales para integradores:**

- **Soporte multi-organización (multi-tenant):** La plataforma puede ahora gestionar múltiples organizaciones de forma completamente aislada. Cada organización tiene su propio espacio de datos, configuración y usuarios, sin interferencias entre sí.

- **Roles de administración más completos:** Se han definido tres niveles de acceso claros — administrador de plataforma, administrador de organización y usuario operativo (LEAR) — para que cada actor tenga exactamente los permisos que necesita.

- **Integración con servicios de firma cualificada (QTSP):** Las organizaciones pueden ahora delegar la firma de credenciales en proveedores de firma electrónica cualificada externos, eliminando la necesidad de gestionar certificados localmente.

- **Compatibilidad con los últimos estándares:** Se ha migrado al formato de credenciales VCDM 2.0 y mejorada la compatibilidad con SD-JWT y DPoP, los estándares más actuales del ecosistema EUDI.

- **Despliegue en la nube mejorado:** Correcciones y ajustes para un despliegue más fiable en entornos AWS, incluyendo la resolución correcta de dominios en configuraciones multi-organización.

- **Comunicaciones más fiables:** Se han modernizado las plantillas de correo electrónico y los flujos de oferta de credenciales, mejorando la experiencia del usuario final.

- **Observabilidad integrada:** Se ha incorporado trazabilidad distribuida (OpenTelemetry) para facilitar el diagnóstico y monitorización en producción.

---

### Verificador de credenciales (`eudistack-core-verifier`)

#### Versiones 3.x — Verificador multi-organización con seguridad reforzada

La serie 3.x representa una renovación profunda del verificador, con el foco en seguridad, compatibilidad con los estándares más recientes y capacidad de servir a múltiples organizaciones.

**Novedades principales para integradores:**

- **Arquitectura renovada y más mantenible:** Se ha reorganizado el código en módulos bien delimitados, lo que facilita las integraciones y reduce el riesgo de regresiones. Se ha actualizado a las últimas versiones de Java y Spring Boot.

- **Seguridad reforzada en todos los niveles:** Se han implementado múltiples capas de protección, entre ellas: autenticación mejorada con PKCE, protección contra ataques de redirección, limitación de peticiones por IP, rotación de tokens de refresco y cabeceras HTTP de seguridad. La verificación criptográfica cubre SD-JWT, Token Status Lists y algoritmos RSA y EC.

- **Soporte completo para los últimos estándares de credenciales:** Compatibilidad simultánea con VCDM 1.1 y 2.0, soporte para SD-JWT VC (RFC 9901), DCQL y credenciales genéricas sin dependencias de esquemas propietarios.

- **Flujo de presentación mejorado:** Sustitución de WebSockets por Server-Sent Events (SSE) para el flujo de autenticación entre dispositivos, con mayor estabilidad y compatibilidad.

- **Personalización visual configurable externamente:** El frontal puede ahora configurarse y personalizarse (logos, colores, traducciones) sin necesidad de modificar el código del verificador.

- **Despliegue multi-organización en la nube:** Compatibilidad con AWS CloudFront y ALB, incluyendo el enrutamiento correcto para despliegues con múltiples organizaciones.

- **Documentación de API disponible:** La API REST está ahora documentada con OpenAPI/Swagger.

---

#### Versiones 2.x — Estabilización de flujos de autenticación y experiencia visual

La rama 2.x consolidó los flujos de autenticación estándar y mejoró la personalización visual del verificador.

**Novedades principales:**

- **Flujos de autenticación más robustos:** Implementación completa del flujo de Authorization Code con PKCE, soporte para tokens de refresco, validaciones de audiencia y nonce para presentaciones verificables.

- **Compatibilidad ampliada con tipos de credenciales:** Soporte para nuevos tipos de credenciales empresariales y de máquina, junto con validaciones de revocación y expiración.

- **Personalización visual dinámica:** Logos, favicons y colores configurables por organización, con soporte de internacionalización basado en el idioma del navegador del usuario.

- **Mejoras de accesibilidad y diseño responsive.**

---

#### Versiones 1.x — Funcionalidades base del verificador

La rama 1.x introdujo las capacidades fundamentales para la verificación de credenciales descentralizadas.

**Funcionalidades iniciales:**

- Implementación de OpenID Connect y OpenID for Verifiable Presentations (OID4VP).
- Soporte para autenticación persona-a-máquina y máquina-a-máquina.
- Login mediante código QR.
- Verificación de presentaciones y credenciales verificables, incluyendo revocación y firma.
- Primera versión del frontal de login con branding corporativo.

---

### Wallet (aplicación web progresiva — `eudistack-core-wallet-pwa`)

#### Versiones 3.x — Wallet multi-organización con experiencia móvil optimizada

La serie 3.x convirtió el wallet en una aplicación adaptable a múltiples organizaciones de forma dinámica, con mejoras significativas en seguridad, accesibilidad y compatibilidad móvil.

**Novedades principales para integradores:**

- **Multi-organización en tiempo de ejecución:** El wallet adapta automáticamente su configuración, apariencia y conexiones según la organización, sin necesidad de despliegues separados.

- **Experiencia iOS mejorada:** Proceso de instalación como PWA optimizado específicamente para Safari en iOS, con compatibilidad mejorada para cámara y deep-links.

- **Accesibilidad WCAG 2.1 AA:** Se han incorporado mejoras de accesibilidad visual y de uso con lectores de pantalla.

- **Flujos de presentación y emisión más fiables:** Mejoras en los flujos OID4VP y OID4VCI tanto en mismo dispositivo como en proximidad. El parser de SD-JWT ha sido reescrito conforme al RFC 9901 más reciente.

- **Seguridad criptográfica reforzada:** Derivación de claves maestras mediante PRF y uso de Web Crypto API para todas las operaciones sensibles.

- **Estabilidad multi-pestaña:** Se ha implementado un mecanismo para evitar conflictos cuando el wallet está abierto en varias pestañas simultáneamente.

- **Actualizaciones automáticas:** El Service Worker se actualiza automáticamente para garantizar que los usuarios siempre dispongan de la versión más reciente.

---

#### Versiones 2.x — Consolidación de capacidades criptográficas y personalización

**Novedades principales:**

- Operaciones criptográficas integradas mediante Web Crypto API e IndexedDB.
- Soporte para generación local de firmas (opcional).
- Branding dinámico configurable: logos, colores y favicons por organización.
- Mejoras en los flujos de introducción de PIN, navegación y selección de cámara.
- Gestión mejorada del ciclo de vida de credenciales y mandatos.

---

#### Versiones 1.x — Funcionalidades base del wallet

**Funcionalidades iniciales:**

- Login, registro y cierre de sesión de usuarios.
- Gestión de identidad descentralizada (DIDs) y credenciales verificables.
- Escaneo de QR y comunicación en tiempo real.
- Integración inicial con EBSI.
- Primeros flujos OIDC y autenticación basada en wallet.

---

### Backend del Enterprise Business Wallet (`eudistack-core-wallet-ebw`)

#### Versiones 1.x — Backend empresarial multi-organización

La rama 1.x construyó el backend del Enterprise Business Wallet desde cero, con foco en seguridad empresarial y soporte multi-organización.

**Novedades principales para integradores:**

- **Arquitectura reactiva y preparada para la nube:** Backend construido sobre Java 25, Spring Boot y WebFlux, con soporte nativo para despliegues en AWS.

- **Multi-organización con aislamiento completo:** Cada organización dispone de su propio espacio de datos, con migraciones automáticas al incorporar nuevas organizaciones. Alineado con el mismo modelo que el Emisor.

- **Autenticación empresarial segura:** Acceso mediante OTP por correo electrónico, tokens JWT con rotación automática, detección de reutilización de tokens y gestión de passkeys.

- **Protección contra abusos:** Limitación de peticiones por IP y por cuenta de correo, además de cabeceras de seguridad HTTP.

- **Almacenamiento cifrado de credenciales:** Las credenciales del wallet se almacenan cifradas con AES-256-GCM. Se gestiona su ciclo de vida completo (válida, suspendida, revocada, expirada) con auditoría trazable.

- **Observabilidad integrada:** Endpoints de salud normalizados, trazabilidad distribuida (OTLP) y métricas Prometheus, alineados con el resto de componentes de la plataforma.

---

## ¿Necesitas más detalle?

Consulta los changelogs técnicos completos de cada componente en GitHub:

- [eudistack-core-issuer](https://github.com/in2workspace/eudistack-core-issuer/blob/main/CHANGELOG.md)
- [eudistack-core-verifier](https://github.com/in2workspace/eudistack-core-verifier/blob/main/CHANGELOG.md)
- [eudistack-core-wallet-pwa](https://github.com/in2workspace/eudistack-core-wallet-pwa/blob/main/CHANGELOG.md)
- [eudistack-core-wallet-ebw](https://github.com/in2workspace/eudistack-core-wallet-ebw/blob/main/CHANGELOG.md)