# Matriz de trazabilidad

Relaciona requisitos, reglas, casos de uso, clases, componentes, pruebas y evidencias. La correspondencia regla por regla está en [02_REGLAS_DE_NEGOCIO.md](02_REGLAS_DE_NEGOCIO.md).

Evidencias: **A** = `mvn verify` (Back-End, ejecutado: 191 pruebas correctas), **B** = `npm run coverage` (Front-End, ejecutado: 99 pruebas correctas).

## Requisitos funcionales

| Requisito | Reglas | Caso de uso | Clases | Componente Front-End | Pruebas | Ev. |
|---|---|---|---|---|---|---|
| RF-001 a RF-003 Registro | RN-069 a 072 | Solicitar registro | `AccountOnboardingService`, `UserAccount`, `InstitutionalEmail`, `MobilePhone` | `RegisterPage` | `OnboardingIntegrationTest`, `ContactValueObjectsTest`, `RegistrationPages.test` | A, B |
| RF-004, RF-005 Unicidad | RN-073, 074 | Solicitar registro | `AccountDataValidator` | `RegisterPage` | `OnboardingIntegrationTest` | A |
| RF-006, RF-007 Validar contactos | RN-077 a 080 | Verificar | `ContactVerification`, `UserAccount` | `VerifyPage` | `UserAccountTest`, `RegistrationPages.test` | A, B |
| RF-008, RF-009 Información personal | RN-101 a 105 | Perfil | `ProfileService` | `ProfilePage` | `OnboardingIntegrationTest`, `PersonalPages.test` | A, B |
| RF-010, RF-011 Nueva validación | RN-081, 082, 110, 111 | Perfil | `UserAccount.updatePersonalInformation` | `ProfilePage` | `UserAccountTest`, `PersonalPages.test` | A, B |
| RF-012 a RF-014 Roles | RN-001 a 038 | Registro | `Role` | `RegisterPage` | `RoleTest`, `domain/roles.test` | A, B |
| RF-015 a RF-017 Facultades | RN-039 a 045 | Gestionar estructura | `Faculty`, `OrganizationService` | `StructurePage`, `FacultyPage` | `OrganizationModelTest`, `StructurePages.test` | A, B |
| RF-018, RF-019 Responsable e historial | RN-042 a 044 | Gestionar estructura | `PositionSuccession` | `FacultyPage`, `TenureHistory` | `OrganizationModelTest`, `StructurePages.test` | A, B |
| RF-020 a RF-022 Programas | RN-046 a 049 | Gestionar estructura | `AcademicProgram` | `StructurePage`, `ProgramPage` | `OrganizationIntegrationTest`, `StructurePages.test` | A, B |
| RF-023, RF-024 Responsable e historial | RN-050 a 052 | Gestionar estructura | `PositionSuccession` | `ProgramPage` | `OrganizationModelTest`, `StructurePages.test` | A, B |
| RF-025 a RF-029 Estudiantes | RN-053 a 060 | Registro | `ProgramMembership`, `OrganizationalPlacement` | `RegisterPage`, `ProgramPage` | `OrganizationModelTest`, `StructurePages.test` | A, B |
| RF-030 a RF-034 Docentes | RN-061 a 068 | Registro | `ProgramMembership` | `RegisterPage`, `ProgramPage` | `OrganizationModelTest` | A, B |
| RF-035 a RF-038 Aprobaciones | RN-090 a 093, 108 | Aprobar | `ApprovalService`, `RegistrationRequest`, `OrganizationalAuthority` | `ApprovalsPage` | `OrganizationalAuthorityTest`, `AccountManagementPages.test` | A, B |
| RF-039, RF-040 Autoridades | RN-094, 095, 129 | Registrar Rector o Administrador | `PrivilegedAccountRegistrationService` | `ManagedAccountsPage` | `OnboardingIntegrationTest`, `AccountManagementPages.test` | A, B |
| RF-041 a RF-045 Categorías | RN-132 a 145 | Gestionar categorías | `Category`, `CategoryService` | `CategoriesPage` | `SupportingAggregatesTest`, `CategoriesPage.test` | A, B |
| RF-046, RF-051, RF-052 Crear Kudo | RN-146 a 154 | CU-001 | `Kudo`, `KudoService` | `NewKudoPage` | `KudoAndReplyTest`, `NewKudoPage.test` | A, B |
| RF-047 Consultar | RN-155 | CU-002 | `KudoService`, `KudoViewAssembler` | `FeedPage`, `KudoDetailPage` | `RecognitionIntegrationTest`, `FeedPage.test` | A, B |
| RF-048 a RF-050 Historial | RN-151 | CU-003 | `KudoService` | `PersonPage` | `RecognitionIntegrationTest`, `PersonalPages.test` | A, B |
| RF-053, RF-054 Réplicas | RN-161, 162 | CU-004, CU-005 | `Reply` | `KudoDetailPage`, `ConversationThread` | `KudoAndReplyTest`, `KudoDetailPage.test` | A, B |
| RF-055, RF-056 Conversación | RN-163, 164 | CU-006 | `Conversation` | `ConversationThread`, `domain/conversation` | `ConversationTest`, `conversation.test`, `ConversationThread.test` | A, B |
| RF-057 a RF-060 Calificaciones | RN-175 a 185 | CU-007 a CU-009 | `Rating`, `StarRating`, `RatingService` | `StarRating` | `RatingTest`, `StarRating.test` | A, B |
| RF-061 a RF-064 Reportes | RN-186 a 190 | CU-010 a CU-012 | `ContentReport`, `ReportService` | `ReportForm`, `ModerationPage` | `ModerationTest`, `ModerationPage.test` | A, B |
| RF-065 a RF-068 Moderación | RN-191 a 200, 243 a 250 | CU-013 a CU-015 | `Publication`, `ModerationDecision`, `ModerationService` | `ModerationPage`, `KudoDetailPage` | `ModerationIntegrationTest`, `ModerationPage.test` | A, B |
| RF-069 a RF-071 Auditoría | RN-096 a 100 | Consultar auditoría | `AuditEventListener`, `AuditQueryService` | `AuditPage` | `ModerationIntegrationTest`, `PersonalPages.test` | A, B |
| RF-072 a RF-075 Afiliación | RN-054, 062, 118 | Consultar persona | `PeopleDirectoryService`, `AffiliationResolver` | `PersonPage` | `OrganizationIntegrationTest`, `PersonalPages.test` | A, B |
| RF-076, RF-077 Autoridades activas | RN-005, 011 | Consultar estructura | `UniversityPosition` | `StructurePage` | `OrganizationIntegrationTest`, `StructurePages.test` | A, B |

## Requisitos no funcionales

| Requisito | Solución | Evidencia |
|---|---|---|
| RNF-001 a RNF-004 Seguridad | JWT, BCrypt, `Capability`, `OrganizationalAuthority` | Pruebas de integración |
| RNF-005, RNF-006 Disponibilidad y errores | `GlobalExceptionHandler`, ProblemDetail | Pruebas de integración |
| RNF-007 a RNF-009 Mantenibilidad | Hexagonal, pocas dependencias | `ArchitectureTest` |
| RNF-010, RNF-011 Escalabilidad | Paginación, índices, sesión sin estado | `V1__institutional_schema.sql` |
| RNF-012 a RNF-017 Observabilidad y auditabilidad | Eventos + `AuditEventListener`, logs | `ModerationIntegrationTest` |
| RNF-018 a RNF-022 Calidad | PMD, ESLint, revisión | [06_CALIDAD_Y_PRUEBAS.md](06_CALIDAD_Y_PRUEBAS.md) |
| RNF-023, RNF-024 Pruebas | Dominio sin frameworks | JaCoCo, Vitest |
| RNF-025 a RNF-027 Análisis estático | PMD y ESLint con hallazgos documentados | [06_CALIDAD_Y_PRUEBAS.md](06_CALIDAD_Y_PRUEBAS.md) |
| RNF-028 a RNF-030 Documentación | Documentos en Word, modelos en Excel y draw.io, y manual | Esta carpeta |
| RNF-031 a RNF-033 Arquitectura | Hexagonal justificada | [05_DATOS_Y_ARQUITECTURA.md](05_DATOS_Y_ARQUITECTURA.md) |
| RNF-034, RNF-035 Trazabilidad | Esta matriz | Este documento |

## Interfaz

| Requisito | Solución |
|---|---|
| ARQ-065 Entender el estado | `StatusBadge`, avisos de estado de cuenta |
| ARQ-066 Navegar conversaciones | Sangría limitada a 8 niveles, colapso de ramas, indicador de nivel |
| ARQ-067 Kudos extensos | Recorte visual con CSS en listados y texto completo en el detalle, sin truncar datos |
