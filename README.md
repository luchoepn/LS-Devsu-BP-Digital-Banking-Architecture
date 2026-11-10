# BP Digital Banking | Propuesta de Arquitectura de Soluciones

Este repositorio reúne la solución que preparé para el ejercicio técnico de **Arquitecto de Soluciones de Devsu**. El entregable principal es un documento PDF autocontenido con la propuesta de diseño, las decisiones arquitectónicas, los diagramas y los contratos de referencia.

📄 **[Ver el documento completo de arquitectura](./BP_Digital_Banking_Arquitectura_LS.pdf)**

## ¿En qué consiste el ejercicio?

El reto plantea el diseño de una plataforma de banca digital para canales **web y móvil**, integrada con un Core bancario y otros sistemas existentes. La solución debe contemplar consultas de información del cliente y movimientos, transferencias entre cuentas propias e interbancarias, pagos, incorporación de clientes con verificación facial, autenticación y notificaciones. También requiere abordar seguridad, auditoría, integración, escalabilidad, disponibilidad y continuidad.

El objetivo del documento es explicar **cómo organizaría la solución y por qué tomaría esas decisiones**, más que presentar una aplicación implementada o desplegada.

## Resumen de la solución propuesta

Propongo una arquitectura omnicanal basada en servicios con responsabilidades bien definidas, contratos versionados y adaptadores para desacoplar los canales digitales de las particularidades del Core y los proveedores externos.

| Área | Propuesta |
| --- | --- |
| **Canal web** | SPA con React y TypeScript, integrada mediante BFF y sesiones protegidas. |
| **Canal móvil** | Aplicación Flutter para Android e iOS, con autenticación OIDC mediante Authorization Code + PKCE y almacenamiento seguro. |
| **Backend** | Servicios orientados a capacidades de negocio, siguiendo DDD y arquitectura hexagonal; APIs con contratos OpenAPI. |
| **Integración bancaria** | Adaptadores anticorrupción hacia Core y sistemas complementarios; Apache Camel de forma opcional cuando se necesiten protocolos o transformaciones legacy. |
| **Integridad financiera** | Idempotencia, estados durables de operación y conciliación de respuestas ambiguas para evitar dobles ejecuciones. |
| **Mensajería y auditoría** | Patrón Transactional Outbox, eventos asíncronos y consumidores idempotentes para notificaciones y trazabilidad. |
| **Identidad y seguridad** | Reutilización del proveedor de identidad existente, autorización por recurso, controles antifraude, protección de datos y verificación facial de onboarding. |
| **Infraestructura** | Despliegue de referencia en AWS con CloudFront/WAF, API Gateway, ECS Fargate, Aurora PostgreSQL, Redis, EventBridge/SQS y observabilidad. |

### Decisiones que considero especialmente importantes

- **El Core sigue siendo la fuente de verdad** para saldos, movimientos y confirmación de operaciones financieras. La solución digital no construye un libro mayor paralelo.
- **Un timeout no significa que una transacción haya fallado.** Cuando el resultado es incierto, se conserva el estado y se concilia antes de intentar cualquier nueva ejecución.
- **La seguridad se aplica en varias capas.** La autenticación no sustituye la verificación de titularidad, permisos, límites y riesgo de cada operación.
- **La auditoría no debe depender de que un consumidor esté disponible en ese instante.** El registro durable y el outbox permiten desacoplar la entrega de eventos de la operación de negocio.
- **La tecnología responde a requisitos y restricciones.** AWS y los componentes seleccionados se presentan como una arquitectura de referencia, con alternativas y supuestos explícitos, no como decisiones impuestas por el ejercicio.

## ¿Qué incluye el documento?

El PDF desarrolla el alcance y los atributos de calidad, la arquitectura frontend web y móvil, las vistas **C4** (contexto, contenedores y componentes), diagramas de secuencia, el modelo de seguridad y amenazas, la topología AWS, decisiones arquitectónicas (**ADR**), aspectos de operación y continuidad, y ejemplos ilustrativos de contratos HTTP/JSON y mensajería ISO 20022/SWIFT MX.

Los contratos, cifras de capacidad y objetivos de disponibilidad son **supuestos de diseño sujetos a validación con la entidad y sus proveedores**. Este entregable no representa un sistema productivo ni una certificación de cumplimiento o de pruebas.

## Entregable y autor

- **Documento:** [BP_Digital_Banking_Arquitectura_LS.pdf](./BP_Digital_Banking_Arquitectura_LS.pdf)
- **Ejercicio:** Arquitecto de Soluciones — Devsu
- **Autor:** [luchoepn](https://github.com/luchoepn)

La propuesta busca equilibrar experiencia digital, mantenibilidad, integridad financiera y evolución gradual de los sistemas bancarios existentes.
