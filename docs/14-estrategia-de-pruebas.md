# Estrategia de pruebas y criterios de calidad

**Propuesta para la beta de Galapa · 13 de noviembre de 2026**

La prioridad es proteger las decisiones que afectan una compra o servicio. Una publicación debe llegar a comercios elegibles, cada cotización debe respetar sus permisos, la aceptación debe fijar una revisión exacta y cada solicitud puede producir un solo acuerdo. Las pruebas verifican el comportamiento del producto y sirven para decidir si un cambio puede pasar a beta.

## Qué probar en cada nivel

| Nivel | Alcance | Herramienta propuesta | Cuándo corre |
| --- | --- | --- | --- |
| Estático | Formato, lint, tipos, compilación afectada, dependencias y secretos. | TypeScript, ESLint y verificaciones del proveedor de CI. | Cada pull request (PR). |
| Unitario | Estados, elegibilidad, vigencias, montos en COP, políticas de visibilidad, reglas de reseñas y decisiones de reintento. Reloj y dependencias controlados. | `node:test` en el backend, con la configuración de TypeScript elegida. Vitest y Testing Library para el panel. | Cada PR. |
| Componentes móviles | Validación de formularios, estados de carga, errores, permisos, reconexión y confirmación antes de aceptar. | Jest con `jest-expo` y React Native Testing Library, según el SDK fijado. | Cada PR que afecte la app o su cliente compartido. |
| Integración | API, Prisma, PostgreSQL, migraciones y trabajador. Abarca consultas por actor, restricciones, transacciones, bloqueos, rollback y deduplicación. | `fastify.inject()` si se adopta Fastify. PostgreSQL efímero por ejecución mediante Testcontainers o servicio de CI, con la versión mayor del entorno destino. | Cada PR que toque backend, esquema o contrato. |
| Contrato | OpenAPI, tipos del cliente, estados HTTP y cuerpos de éxito y error. Comparar con el contrato de la última versión desplegada en el entorno de destino y ejecutar los flujos con las versiones móviles soportadas. | Validador de OpenAPI 3.1, comparación de cambios incompatibles y pruebas de integración con solicitudes y respuestas reales. | Cada PR de API, contrato o cliente. Compatibilidad con la versión anterior antes de promover. |
| Extremo a extremo | Comprador, comercio y operaciones. Flujos de un producto y un servicio, admisión, aprobación comercial, publicación, aceptación, confirmación y cancelación por soporte. | Maestro para app y Playwright para panel. Android e iOS con builds de distribución antes de promover. | Suite breve antes de publicar en beta o producción. Suite ampliada en ejecución programada y ante cambios de alto riesgo. |
| Rendimiento y recuperación | Carga de referencia, cola atrasada, caída de un proceso, pérdida de red, despliegue y restauración. | k6, métricas de aplicación y entorno aislado de restauración. | Antes de abrir la beta, ante cambios relevantes y antes de ampliar tráfico. |

`node:test` y `fastify.inject()` cubren casos rápidos en el backend. Las transacciones, restricciones y migraciones se prueban contra PostgreSQL real. Para el flujo diario, el proveedor de correo, push y archivos puede sustituirse por dobles controlados. Antes de promover, probar OTP y avisos en beta con los proveedores y dispositivos reales usando cuentas de prueba. [Node.js Test Runner](https://nodejs.org/docs/latest-v26.x/api/test.html) · [Fastify Testing](https://fastify.dev/docs/latest/Guides/Testing/) · [Testcontainers PostgreSQL](https://node.testcontainers.org/modules/postgresql/)

Referencias de las herramientas de interfaz y de carga. [Vitest](https://vitest.dev/guide/) · [Pruebas unitarias en Expo](https://docs.expo.dev/develop/unit-testing/) · [Maestro](https://docs.maestro.dev/) · [Playwright](https://playwright.dev/docs/best-practices) · [k6](https://grafana.com/docs/k6/latest/using-k6/thresholds/)

## Regresiones críticas de la beta

| Caso | Comprobación mínima |
| --- | --- |
| Admisión y perfil comercial | Un correo verificado sin aprobación puede consultar su estado. Una cuenta admitida compra con perfil comercial pendiente. Solo el comercio aprobado recibe oportunidades y puede cotizar. |
| Acceso entre cuentas | Un comprador ajeno y un comercio ajeno carecen de acceso a ofertas, adjuntos y acuerdos privados. La oportunidad omite presupuesto y contacto privado. Un aviso conserva la misma regla al abrirse. |
| Condiciones y vigencia | Recalcular el total desde precio y cargos. Con el reloj en el límite de vigencia, verificar la respuesta esperada. Una revisión vieja da conflicto y la app solicita confirmar de nuevo. |
| Aceptación concurrente | Dos conexiones a PostgreSQL compiten por aceptar ofertas distintas sobre la misma solicitud. Quedan un acuerdo, una cotización aceptada y un evento de adjudicación. Si falla la transacción, ninguno queda a medio guardar. |
| Red y reintentos | Simular pérdida de la respuesta después de confirmar la escritura. Reenviar la misma acción con la misma clave y verificar el resultado existente. Cambiar el cuerpo con esa clave devuelve conflicto. |
| Cancelación y cumplimiento | Confirmar la resolución de soporte y el historial. Si se adopta la pausa de cumplimiento, probar que una confirmación y un caso abierto concurrentes respetan la decisión que se registró primero. |
| Vencimientos y avisos | Aunque el trabajador esté detenido, la API rechaza una oferta vencida. Al recuperar el trabajador, el aviso aparece una vez en la bandeja aunque se reprocese el evento. |
| Versiones coexistentes | Ejecutar la app anterior soportada contra la API candidata y probar la API y el trabajador anteriores sobre el esquema ampliado. Comprobar el recorrido con un producto y un servicio. |
| Restauración | Restaurar en un entorno aislado, comprobar recuentos y muestras de solicitudes, acuerdos y adjuntos, y registrar cuánto se perdió y cuánto se tardó. |

## Evidencia que acompaña un cambio

- Una funcionalidad trae pruebas de sus criterios de aceptación. Un parche incluye una prueba que reproduce el fallo cuando existe un caso determinista. Para flujos críticos, dejar evidencia del caso exitoso, un rechazo y cualquier condición concurrente o de red que cambie el comportamiento.
- Reportar cobertura de líneas y ramas por paquete y módulo. Durante el arranque, usarla como diagnóstico y comprobar la matriz de casos críticos como requisito de integración. Fijar un piso de cobertura con la línea base de la primera implementación. Si una rama crítica queda sin prueba, documentar el caso faltante en el PR y resolverlo antes de promover la funcionalidad.
- Usar datos sintéticos y aislar la base y los archivos de cada ejecución. Usar fechas fijas, esperas basadas en estados y registros de errores. Si una prueba crítica falla de manera intermitente, conservar el primer fallo, investigar y mantener bloqueado el cambio afectado hasta tener una verificación estable.
- Publicar por ejecución los resultados de pruebas, cobertura y compilación. Conservar trazas o capturas de los fallos de interfaz con datos sintéticos. Vincular el resultado al commit y al entorno.

El objetivo de tiempo para un PR normal se fija tras medir las primeras ejecuciones. Si la suite crece, mantener las comprobaciones rápidas en cada PR y trasladar escenarios costosos a ejecuciones programadas y a la verificación previa al despliegue. Conservar los casos de integridad, autorización y compatibilidad relevantes en la verificación obligatoria del cambio.

Los objetivos medibles están en [rendimiento y operación](11-rendimiento-y-operacion.md). El orden de ejecución y las reglas de integración están en [forma de trabajo](15-forma-de-trabajo.md).
