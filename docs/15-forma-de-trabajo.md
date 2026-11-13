# Forma de trabajo del equipo

**Acuerdos y procedimiento propuesto · 13 de noviembre de 2026**

El equipo usará **GitHub y GitHub Actions**. Todo cambio a `main` pasa por un pull request, una aprobación de otra persona y pruebas automáticas satisfactorias. En beta habrá un horario acordado, una persona responsable y un suplente. La promoción a producción será manual dentro del horario cubierto. Las decisiones de publicación se detallan en el [procedimiento de despliegue e incidentes](16-despliegue-e-incidentes.md).

## De la tarea a `main`

1. **Definir el cambio.** Crear un issue con objetivo, criterios de aceptación, responsable y riesgo. Si toca la app instalada, API, base de datos, permisos o avisos, indicar las versiones o datos afectados y el procedimiento de recuperación. Vincular el issue al backlog técnico cuando aplique.
2. **Trabajar en una rama corta.** Crear la rama desde `main` y dividir cambios grandes en entregas que se puedan revisar e integrar por separado. Usar activación por configuración para una funcionalidad que deba quedar deshabilitada mientras se completa. La API conserva sus verificaciones de permisos en cualquier estado de esa configuración.
3. **Abrir el PR.** Describir el cambio, qué se probó, cómo se revierte o mitiga y qué alertas mirar si aplica. Incluir una prueba de regresión para parches reproducibles y notas de migración o compatibilidad cuando afecte un contrato. Poner los cambios de interfaz delante de la persona que los revisa mediante una vista o captura de prueba cuando sea pertinente.
4. **Revisar e integrar.** Otra persona revisa el comportamiento y las pruebas. Para cambios en datos, identidad o despliegue, asignar a alguien que conozca ese componente. Volver a solicitar aprobación si se modifica el cambio revisado. Integrar con `squash merge` como convención propuesta para dejar un cambio rastreable por PR.
5. **Verificar en el entorno.** El responsable revisa los controles posteriores al despliegue y registra resultado, versión y enlace a la ejecución. Una tarea que requiere publicación se cierra con evidencia del entorno indicado en sus criterios.

Las ramas cortas y la integración frecuente permiten revisar y revertir cambios pequeños. [DORA sobre desarrollo en rama principal](https://dora.dev/capabilities/trunk-based-development/) · [DORA sobre integración continua](https://dora.dev/capabilities/continuous-integration/)

## Configuración prevista en GitHub

| Control | Política acordada o propuesta |
| --- | --- |
| Rama principal | Usar un ruleset o protección equivalente. Exigir PR, una aprobación ajena al autor y comprobaciones de CI. Bloquear pushes directos, borrados y force pushes. Invalidar aprobaciones obsoletas y pedir resolución de conversaciones. Usar base actualizada antes de integrar o una cola si el volumen lo requiere y el plan lo permite. |
| Comprobaciones requeridas | Mantener un trabajo final estable, por ejemplo `ci-gate`, que se ejecute incluso si un trabajo anterior falla y compruebe el resultado explícito de cada trabajo aplicable. Una omisión solo es válida cuando el trabajo queda fuera del alcance por regla declarada. Cambios en contrato, paquetes compartidos, CI, lockfile o versiones del runtime verifican todos los consumidores que afecten. |
| Permisos de CI | PRs con credenciales de solo lectura y sin secretos de despliegue. Restringir publicación a flujos de confianza. Fijar acciones externas por SHA revisado y conceder permisos mínimos. Serializar despliegues por entorno sin cancelar un despliegue activo para iniciar otro. |
| Ambientes | Usar secretos y permisos separados. Confirmar qué controles permite el plan de GitHub para un repositorio privado. Si las aprobaciones nativas de entorno faltan, registrar la autorización en la publicación y limitar la ejecución o el acceso al proveedor a los responsables habilitados. |
| Registro de cambios | Vincular issue, PR, commit, artefacto y ejecución. Registrar migraciones, configuración relevante y builds móviles para poder identificar la última versión sana. |

Referencias para configurar la protección según el plan de GitHub. [Rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets) · [Entornos](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments) · [Uso seguro de Actions](https://docs.github.com/en/actions/reference/security/secure-use) · [Concurrencia de flujos](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency)

## Cambios nuevos, mantenimiento y parches

| Tipo de trabajo | Cómo se incorpora |
| --- | --- |
| Funcionalidad | Issue con criterios, PR pequeño, pruebas de los comportamientos afectados y verificación en beta. Activación gradual si cambia un flujo sensible. |
| Parche normal | Prueba que reproduce el problema cuando sea viable, corrección, comprobaciones automáticas y verificación puntual del caso en beta. |
| Parche urgente | Mitigar primero mediante pausa, desactivación o reversión de una versión compatible si ayuda a recuperar el servicio. Priorizar un PR pequeño con otra revisión y comprobaciones requeridas. Si la persona titular no está disponible, escalar al suplente. |
| Dependencias y runtime | Proponer revisión semanal de actualizaciones, además de revisión inmediata de avisos relevantes. Usar Dependabot si el repositorio y el plan lo permiten. Actualizar Node 26 con sus parches probados, fijar pnpm y mantener el lockfile. Revisar por separado cambios mayores de Prisma, Expo o PostgreSQL y probar el conjunto compatible. [Seguridad de la cadena de suministro en GitHub](https://docs.github.com/en/code-security/how-tos/secure-your-supply-chain) |
| Migración o contrato | Añadir una nota de compatibilidad, pruebas con versiones coexistentes y plan de recuperación. Coordinar el orden entre base, API, trabajador, panel y app instalada. |

## Responsables y acuerdos por completar

- **Autor y revisor.** El autor presenta el cambio y los resultados. El revisor es otra persona y confirma los criterios de aceptación. El equipo acuerda quién puede revisar datos, seguridad y despliegue.
- **Responsable de beta y suplente.** El titular atiende y coordina la primera respuesta en el horario publicado. El suplente cubre ausencias y tiene los accesos necesarios. Antes de iniciar beta, el equipo debe nombrarlos, definir horario en `America/Bogota`, canal de alertas y acceso a GitHub, infraestructura y tiendas.
- **Responsable de publicación.** Registra la versión, realiza la verificación posterior y coordina una reversión si hace falta. Puede coincidir con quien cubre la beta. Dejar quién autoriza la promoción y quién ejecuta en la ficha de publicación.

Una ficha de PR debe poder responder qué cambia, cómo se verificó, qué versiones afecta y cómo recuperar el flujo. Para una publicación, registrar entorno, artefacto, migración, responsable, comprobaciones, ventana de observación y resultado.
