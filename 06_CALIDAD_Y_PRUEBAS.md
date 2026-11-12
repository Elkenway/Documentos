# Calidad, análisis estático, pruebas y conclusiones

## 1. Calidad del software

| Criterio | Cómo se atiende |
|---|---|
| Legibilidad | Nombres del dominio (`ensureUsableForNewKudos`, `handOver`); comentarios solo donde explican un porqué |
| Mantenibilidad | Capas con dependencias verificadas automáticamente; una regla, un lugar |
| Reutilización | `Publication`, `PositionSuccession`, `TextForm`, `useAsync` |
| Acoplamiento | Puertos y eventos; agregados referenciados por identificador |
| Cohesión | Módulos por concepto de negocio (`identity`, `moderation`, ...) |
| Testabilidad | Dominio sin frameworks y con el tiempo recibido como parámetro (`Instant now`) |
| Evolución | Puntos de extensión descritos en la arquitectura |

## 2. Antipatrones evitados

| Antipatrón | Medida |
|---|---|
| Modelo anémico | Reglas dentro de los agregados |
| God Object | División por agregados; PMD limita métodos y complejidad |
| Herencia para reutilizar | Roles por valor; sucesión por composición |
| Números y cadenas mágicas | Enumeraciones y constantes con nombre |
| Lógica en controladores | ArchUnit impide que un controlador toque persistencia |
| Ingeniería anticipada | Lista de patrones descartados y decisiones YAGNI |

## 3. Análisis estático

### Front-End

| Herramienta | Configuración | Resultado |
|---|---|---|
| TypeScript | `strict`, `noUncheckedIndexedAccess` | 0 errores |
| ESLint | `typescript-eslint` estricto, reglas de hooks | 0 errores, 0 advertencias |

Hallazgos y correcciones:

| Hallazgo | Corrección |
|---|---|
| 14 usos de `request<void>` (`no-invalid-void-type`) | Se cambió a `request<undefined>`, que además describe lo que devuelve una respuesta 204 |
| `useCallback` envolviendo funciones ya estables (`exhaustive-deps`) | Se eliminó el envoltorio: las funciones de módulo no cambian entre renderizados |
| Primera versión de `descendantCounts` costaba O(n × profundidad) (revisión manual) | Reescrita con una pila en O(n) |
| Restricción de autocalificación solo en la interfaz (revisión manual) | Eliminada: el enunciado no la pide y el servidor no la aplicaba |

### Back-End

PMD 7 con `config/pmd/ruleset.xml`, categorías completas y exclusiones justificadas. `mvn verify` falla ante cualquier violación.

| Regla excluida | Motivo |
|---|---|
| `LawOfDemeter` | Marca fluidez normal de `Stream` y `record` |
| `DataClass` | Marca `record` y vistas, que son datos por diseño |
| `OnlyOneReturn` | Los retornos tempranos dan código más claro |
| `LongVariable`, `ShortMethodName`, `LinguisticNaming` | Chocan con nombres descriptivos del dominio |
| `MethodArgumentCouldBeFinal`, `LocalVariableCouldBeFinal` | Ruido sin beneficio en métodos cortos |
| `ExcessiveImports`, `CouplingBetweenObjects` | Falsos positivos en clases de configuración que conectan todo |
| `AtLeastOneConstructor`, `CallSuperInConstructor`, `CommentDefaultAccessModifier` | Estilo sin efecto en la calidad |
| `GuardLogStatement`, `AvoidDuplicateLiterals`, `AvoidFieldNameMatchingMethodName`, `AvoidThrowingRawExceptionTypes`, `PackageCase`, `TooManyStaticImports`, `UseConcurrentHashMap` | No aplican a este código o generan falsos positivos con `record` y pruebas |

Ajustadas: `TooManyMethods` a 30, `ExcessiveParameterList` a 10, `ShortVariable` a 2 letras, `ShortClassName` a 4 letras y `TooManyFields` a 16 campos.

Corrección detectada en revisión: `MobilePhone` tenía expresiones regulares mal escapadas; se reescribió con clases explícitas (`[0-9]`, `[ ()+\-]`).

**Resultado de la ejecución.** Con JDK 21 y PMD 7.3.0, `mvn verify` termina en `BUILD SUCCESS` con 0 violaciones. La primera ejecución encontró 87 hallazgos, atendidos así:

| Hallazgo | Cantidad | Tratamiento |
|---|---|---|
| `ShortVariable` | 76 | `id` es el nombre universal del identificador; la regla se ajustó para aceptar dos letras |
| `ShortClassName` | 1 | `Kudo` es el término central del dominio; la regla se ajustó a cuatro letras |
| `TooManyFields` | 1 | `UserAccountEntity` refleja las 16 columnas de `user_account`; límite ajustado a 16 |
| `CyclomaticComplexity` | 1 | Corregido en el código: `Affiliation.requiredFor` se dividió con `isConsistentWith` |
| `AvoidLiteralsInIfCondition` | 2 | Corregido en el código: constantes con nombre en `Affiliation` y `PositionSuccession` |
| `MissingStaticMethodInNonInstantiatableClass` | 2 | Suprimida en `OrganizationViews` y `RecognitionViews`: solo agrupan records |
| `AbstractClassWithoutAbstractMethod` | 1 | Suprimida en `AggregateRoot`: es abstracta para impedir instanciar un agregado genérico |
| `TooFewBranchesForASwitchStatement`, `SwitchStmtsShouldHaveDefault` | 2 | Suprimidas en `PublicationCatalog.save`: `Publication` es sellada y el compilador exige cubrir sus dos casos |
| `UnusedAssignment` | 1 | Suprimida en `Affiliation`: en un constructor compacto, reasignar el parámetro es lo que fija el campo |

## 4. Estrategia de pruebas

La mayor parte del valor está en las reglas, así que la mayor parte de las pruebas apunta al dominio, donde son rápidas y no necesitan infraestructura. Las de integración comprueban que todas las piezas encajan.

| Nivel | Herramienta | Qué verifica |
|---|---|---|
| Dominio | JUnit 5 | Invariantes, transiciones y permisos |
| Arquitectura | ArchUnit | Dirección de dependencias |
| API | `@SpringBootTest`, MockMvc, H2 en modo PostgreSQL | Flujos completos por HTTP con seguridad |
| Lógica del Front-End | Vitest | Roles, conversación, cliente HTTP |
| Pantallas | Testing Library + servidor simulado | Lo que ve la persona y lo que se envía |

Pruebas ejecutadas en el Back-End (191, contando cada repetición de las pruebas parametrizadas y las 4 reglas de ArchUnit):

| Clase | Pruebas | Clase | Pruebas |
|---|---|---|---|
| `UserAccountTest` | 26 | `SupportingAggregatesTest` | 10 |
| `KudoAndReplyTest` | 19 | `ModerationStatusTest` | 22 |
| `OrganizationModelTest` | 12 | `ConversationTest` | 3 |
| `ModerationTest` | 10 | `OnboardingIntegrationTest` | 12 |
| `ContactValueObjectsTest` | 21 | `RecognitionIntegrationTest` | 10 |
| `OrganizationalAuthorityTest` | 7 | `OrganizationIntegrationTest` | 9 |
| `RatingTest` | 9 | `ModerationIntegrationTest` | 6 |
| `RoleTest` | 11 | `ArchitectureTest` | 4 |

H2 no ejecuta la migración de índices parciales; por eso esas garantías se comprobaron aparte contra PostgreSQL 18 (ver "Pruebas de punta a punta").

### Resultados

**Front-End (ejecutado):** 17 archivos, 99 pruebas, todas correctas.

| Métrica | Cobertura | Mínimo exigido |
|---|---|---|
| Líneas | 98,35 % | 85 % |
| Sentencias | 98,35 % | 85 % |
| Ramas | 93,46 % | 80 % |
| Funciones | 87,71 % | 80 % |

Excluidos de la medición: `main.tsx`, los tipos y las utilidades de prueba.

**Back-End (ejecutado):** 191 pruebas, todas correctas. `mvn verify` termina en `BUILD SUCCESS`: pruebas, ArchUnit, JaCoCo y PMD.

| Métrica | Cobertura | Mínimo exigido |
|---|---|---|
| Líneas | 97,20 % | 85 % |
| Instrucciones | 96,18 % | No exigido |
| Métodos | 95,56 % | No exigido |
| Ramas | 83,59 % | No exigido |

Se excluye `DemoDataSeeder`, que solo carga datos de ejemplo.

La primera ejecución del Back-End dejó 4 pruebas fallidas por errores de las propias pruebas, no de la aplicación: dos leían la respuesta como ISO-8859-1 en lugar de UTF-8 y dos comparaban también los identificadores de objetos anidados. Se corrigieron en `ApiIntegrationTest` (lectura en UTF-8 y utilidad `idsOf`).

### Pruebas de punta a punta

Con el perfil `demo`, sobre PostgreSQL 18 real y la API en ejecución, se comprobaron 84 verificaciones, todas correctas:

| Área | Qué se comprobó |
|---|---|
| Autenticación y seguridad | Ingreso por rol, 401 sin token, 403 por permisos, mensajes sin revelar qué dato falló |
| Kudos y réplicas | Publicación, autorreconocimiento rechazado, texto de 20.000 caracteres sin truncar, tildes en UTF-8, conversación de 13 niveles |
| Calificaciones | Escala de 1 a 5, una por persona y contenido, promedio recalculado |
| Reportes y moderación | Solo Rector y Coordinador Académico moderan; veto, liberación y veto directo; contenido oculto sin borrarlo; evidencia en auditoría |
| Categorías y estructura | Permisos por rol, nombres únicos, categoría inactiva rechazada, Kudo histórico con su nombre de categoría original |
| Registro y cuentas | Registro, códigos, aprobación por ámbito, activación con las tres condiciones, suspensión, reactivación, nueva verificación al cambiar correo |
| Base de datos | Migraciones V1 y V2, 18 tablas, 30 claves foráneas, 20 CHECK y los 4 índices únicos parciales rechazando datos inválidos |

También se recorrió la interfaz en el navegador (ingreso, muro, publicar, replicar, calificar, reportar, moderar y auditoría) sin errores en la consola.

### Concurrencia en la moderación

Las pruebas automáticas corren en secuencia, así que no pueden detectar una condición de carrera. Por eso se enviaron 25 pares de peticiones simultáneas contra PostgreSQL 18. La primera medición encontró un defecto real, corregido con un bloqueo de fila en `PublicationCatalog.requireForUpdate`.

| Escenario | Sin bloqueo | Con bloqueo |
|---|---|---|
| Dos decisiones opuestas sobre la misma revisión | 24 de 25: ambas responden éxito y quedan dos decisiones registradas | 25 de 25: queda una decisión y la otra petición recibe 422 |
| Un reporte que llega durante un veto | 3 de 25: el reporte sobrescribe el veto y el contenido vuelve a quedar reportado | 0 de 25: el veto siempre se conserva |

## 5. Duplicación evitada y refinamiento del modelo

| Riesgo | Solución | Motivo |
|---|---|---|
| Periodos de cargo repetidos en tres agregados | `PositionSuccession` | DRY, composición |
| Preparación repetida en cada prueba de pantalla | `fakeServer` y `renderApp` | Pruebas que leen como el comportamiento |
| Modelo anémico derivado del enunciado | Modelo enriquecido | Ver [03_MODELOS.md](03_MODELOS.md) |

## 6. Conclusiones

- Separar primero la estructura (modelo anémico) y después asignar comportamiento obligó a preguntar quién es dueño de cada regla, y varias respuestas cambiaron la forma del modelo, en particular representar los roles como valor y no como herencia.
- Las reglas de exclusividad de roles del enunciado desaparecen como código al elegir la representación correcta: un campo único hace imposible el estado inválido.
- El historial se resolvió con una regla simple: cerrar registros, nunca borrarlos.
- Los eventos permitieron cumplir decenas de exigencias de auditoría desde un único listener.
- El análisis encontró contradicciones reales en el enunciado; documentarlas es parte del diseño.

Trabajo futuro: proveedor real de correo y SMS, política de retención para `KudoViewed` y actualización en vivo de notificaciones.
