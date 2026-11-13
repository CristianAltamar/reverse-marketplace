# Despliegue, parches y respuesta a incidentes

**Procedimiento propuesto para la beta de Galapa · 13 de noviembre de 2026**

El objetivo es que las personas puedan seguir cotizando y cerrando acuerdos durante las publicaciones habituales. Para ello se prueban las versiones que coexistirán, se conserva una versión recuperable y se detiene la promoción si aparecen errores. El equipo confirmó **promoción manual a producción en horario cubierto** y **TestFlight y Google Play para las primeras actualizaciones móviles**. En beta, programar los cambios que afecten a usuarios dentro del horario acordado con el titular y su suplente.

## De integración a publicación

| Etapa | Qué debe ocurrir | Evidencia |
| --- | --- | --- |
| PR | Ejecutar controles estáticos, unitarios y los trabajos afectados de componentes, contrato e integración. Obtener una revisión de otra persona. | PR aprobado y comprobaciones requeridas satisfactorias. |
| `main` | Crear un artefacto identificable por commit y digest para API y trabajador. Registrar build y configuración por entorno para móvil. | Versiones y resultados en GitHub Actions. |
| Beta | Ejecutar migraciones compatibles una sola vez. Desplegar y verificar API, trabajador y panel. Instalar los builds móviles en los canales de prueba y ejecutar los casos afectados. | Migración registrada, comprobaciones breves verdes y evidencia del recorrido afectado. |
| Producción | Promover manualmente tras revisar la evidencia de beta en horario cubierto. Reutilizar los artefactos de servidor probados cuando la configuración se inyecte por entorno. Si móvil requiere un binario distinto por firma o configuración, compilar desde el mismo commit y verificarlo. | Ficha de publicación con autorización, versiones, responsables, observación y acción de recuperación. |

Para API, proponer una comprobación de proceso y otra de disponibilidad para servir tráfico. Esta última verifica la conexión a PostgreSQL y que el esquema admite la versión que está arrancando, con un tiempo límite. Al sustituir instancias, dejar de aceptar trabajo, drenar las peticiones en curso y cerrar conexiones. El trabajador deja de tomar tareas y termina las que pueda o las devuelve a una cola recuperable. Si el proveedor admite una promoción gradual, empezar con una parte del tráfico. Para una beta con poco tráfico, verificar con el equipo y una cohorte reducida y observar resultados absolutos. [Despliegues de Render](https://render.com/docs/deploys) · [Publicación gradual según Google SRE](https://sre.google/workbook/canarying-releases/)

## Base de datos, API y app instalada

1. **Ampliar.** Agregar columnas o tablas compatibles en un único trabajo de migración. Probarlo sobre un esquema vacío y sobre el esquema anterior con datos sintéticos. Medir bloqueo y duración en un conjunto representativo. Verificar que API y trabajador anteriores siguen funcionando con el esquema ampliado.
2. **Transicionar.** Desplegar el código compatible y completar los datos históricos con un proceso reanudable si hace falta. Comprobar conteos y casos de negocio antes de cambiar las lecturas. Si se requieren escrituras dobles durante una transición, conservar su consistencia y comprobarlas en beta.
3. **Retirar.** Eliminar campos o rutas en una publicación separada tras comprobar que las versiones soportadas y el proceso de recuperación dejaron de usarlos. Definir cómo se identifica la versión móvil anterior soportada y cómo se comunica una actualización necesaria. Ante incompatibilidades de API, versionar y coordinar la transición con las apps instaladas.

Probar tanto la app actual como la anterior soportada contra la API candidata. Al restaurar el servidor anterior, conservar las migraciones compatibles ya aplicadas. Para un problema de datos, evaluar primero una corrección hacia delante. Una restauración que pueda perder escrituras posteriores requiere estimar su impacto, acordar cómo reconciliar esas escrituras y verificar el respaldo en un entorno aislado. [Migraciones expandir y contraer de Prisma](https://www.prisma.io/docs/guides/database/data-migration)

## Actualizaciones móviles

Usar TestFlight y los canales de prueba de Google Play para verificar el binario de distribución en ambos sistemas. En producción, usar publicación por etapas para actualizaciones cuando la tienda y el tipo de versión lo permitan. Si aparece una regresión, pausar la distribución y preparar un binario corregido para quienes ya actualizaron. La API debe seguir operando con las versiones soportadas durante ese período. [Fases en App Store](https://developer.apple.com/help/app-store-connect/update-your-app/release-a-version-update-in-phases/) · [Despliegue por etapas en Google Play](https://support.google.com/googleplay/android-developer/answer/6346149)

Evaluar Expo Update después de operar las primeras actualizaciones por tienda. Si se adopta para cambios compatibles con el runtime nativo y las políticas de tienda, separar canales, fijar una política de `runtimeVersion`, ensayar el rollback en una build de prueba y medir el resultado antes de ampliar. Un cambio nativo se publica con un nuevo binario. [Runtime de Expo Update](https://docs.expo.dev/eas-update/runtime-versions/) · [Rollbacks de Expo](https://docs.expo.dev/eas-update/rollbacks/)

## Cómo decidir si continuar, pausar o revertir

Antes de ampliar, comprobar que la API sirve solicitudes, que el trabajador procesa eventos y que una cuenta de prueba puede completar el recorrido afectado. Usar datos aislados en producción para comprobaciones sintéticas y mantenerlos fuera de oportunidades, avisos y métricas comerciales de usuarios. Comparar errores, latencia, retraso de avisos y fallos móviles con la [línea base de beta](11-rendimiento-y-operacion.md).

| Señal | Acción |
| --- | --- |
| Falló migración, arranque o comprobación breve. | Detener la promoción. Revisar el estado de la migración antes de reintentar. Mantener la versión sana si sigue compatible. |
| Aparece un acuerdo duplicado, exposición de datos o cambio de condiciones aceptadas. | Tratar como incidente crítico. Pausar la función o distribución afectada y preservar registros para investigar y reconciliar. |
| La versión candidata eleva fallos de acceso, 5xx, latencia, retraso de cola o cierres inesperados. | Pausar expansión. Revisar trazas y números absolutos. Desactivar la función, revertir el servidor si el esquema lo permite o publicar una corrección. |
| Las comprobaciones pasan y las métricas se mantienen dentro de los límites acordados. | Registrar la evidencia y promover según la política del entorno. |

Propuesta inicial de observación. Vigilar 30 minutos después del cambio y esperar al menos un recorrido sintético exitoso de cada rol afectado. Si el tráfico real es bajo, ampliar la observación hasta la siguiente ventana con uso suficiente o repetir las comprobaciones. Definir umbrales por ventana y canal antes de abrir la beta. Con poco volumen, una sola falla reproducible de una operación crítica puede justificar la pausa. [Alertas por objetivos de servicio](https://sre.google/workbook/alerting-on-slos/)

Medir disponibilidad del flujo crítico como comprobaciones válidas exitosas sobre el total de comprobaciones válidas en un período. Reportar también cuántas hubo y cuántas fallaron. La prueba de carga usa sus propios umbrales. Acordar la ventana de medición, el objetivo de disponibilidad y el canal de alertas con la primera línea base de beta. Para recuperar datos, medir RPO y RTO por separado según [rendimiento y operación](11-rendimiento-y-operacion.md).

### Ficha de publicación

- [ ] Registrar entorno, commit, artefactos, versión móvil, migraciones y última versión recuperable.
- [ ] Confirmar revisión, CI, compatibilidad con la app instalada y comprobaciones en beta.
- [ ] Identificar responsable, suplente, horario, alertas y medida concreta de recuperación para este cambio.
- [ ] Ejecutar la promoción, verificar el flujo afectado y registrar métricas durante la ventana de observación.
- [ ] Registrar la decisión de ampliar, pausar o revertir y dejar evidencia del resultado.

## Respuesta y recuperación

- **Crítico.** Hay pérdida o exposición de datos, acuerdos incorrectos o el flujo principal está indisponible. El responsable coordina, el suplente cubre si hace falta y otra persona puede ejecutar la mitigación. Pausar publicaciones, determinar impacto, aplicar la medida reversible disponible y verificar la recuperación desde la app.
- **Degradación.** Hay una caída parcial o una dependencia con retraso. Registrar alcance y versión. Si la API y la bandeja funcionan aunque fallen los avisos push, comunicar el estado al equipo y recuperar el procesamiento pendiente. Escalar si afecta el flujo comercial.
- **Fallo acotado.** Registrar un issue con pasos, versión y evidencia. Incorporar la corrección y su prueba en el ciclo normal de PR.

Durante un incidente registrar cuándo empezó, qué se afectó, decisiones, responsables y cómo se comprobó la recuperación. Los avisos fuera del horario acordado se registran para la siguiente cobertura según el mecanismo que defina el equipo. Ante un incidente grave, hacer una revisión centrada en causas y medidas, con responsable y fecha para cada tarea. Antes de invitar usuarios, ensayar la reversión del servidor, la recuperación del trabajador y la restauración de la base y adjuntos. [Gestión de incidentes de Google SRE](https://sre.google/sre-book/managing-incidents/) · [Revisión posterior de incidentes](https://sre.google/sre-book/postmortem-culture/)
