# Catálogo de reglas de negocio

Las 250 reglas del enunciado, con el elemento del diseño que las garantiza y la clase de prueba que las cubre. Las contradicciones del catálogo y cómo se resolvieron están en [01_ANALISIS.md](01_ANALISIS.md#contradicciones-y-vacíos-del-enunciado).

## Exclusividad de roles

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-001 | Una persona solamente podrá tener un rol institucional activo. | `UserAccount` tiene un único campo `Role`; el enum hace imposible acumular roles | RoleTest, UserAccountTest |
| RN-002 | Los roles institucionales son mutuamente excluyentes. | `UserAccount` tiene un único campo `Role`; el enum hace imposible acumular roles | RoleTest, UserAccountTest |
| RN-003 | No se permite la acumulación de cargos institucionales. | `UserAccount` tiene un único campo `Role`; el enum hace imposible acumular roles | RoleTest, UserAccountTest |
| RN-004 | Toda asignación de rol deberá respetar las políticas institucionales vigentes. | `UserAccount` tiene un único campo `Role`; el enum hace imposible acumular roles | RoleTest, UserAccountTest |
| RN-005 | La Universidad solamente podrá tener un Rector activo. | `UniversityPosition` + `PositionSuccession`; índice parcial de titular vigente | OrganizationModelTest |
| RN-006 | Un Rector no podrá ser Coordinador Académico. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-007 | Un Rector no podrá ser Coordinador de Facultad. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-008 | Un Rector no podrá ser Coordinador de Programa. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-009 | Un Rector no podrá ser Docente. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-010 | Un Rector no podrá ser Estudiante. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-011 | La Universidad solamente podrá tener un Coordinador Académico activo. | `UniversityPosition` + `PositionSuccession`; índice parcial de titular vigente | OrganizationModelTest |
| RN-012 | Un Coordinador Académico no podrá ser Rector. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-013 | Un Coordinador Académico no podrá ser Coordinador de Facultad. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-014 | Un Coordinador Académico no podrá ser Coordinador de Programa. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-015 | Un Coordinador Académico no podrá ser Docente. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-016 | Un Coordinador Académico no podrá ser Estudiante. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-017 | Cada Facultad solamente podrá tener un Coordinador de Facultad activo. | `Faculty.assignCoordinator` + `PositionSuccession`; `uk_faculty_single_current_tenure` | OrganizationModelTest |
| RN-018 | Un Coordinador de Facultad no podrá ser Rector. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-019 | Un Coordinador de Facultad no podrá ser Coordinador Académico. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-020 | Un Coordinador de Facultad no podrá ser Coordinador de Programa. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-021 | Un Coordinador de Facultad no podrá ser Docente. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-022 | Un Coordinador de Facultad no podrá ser Estudiante. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-023 | Cada Programa Académico solamente podrá tener un Coordinador de Programa activo. | `AcademicProgram.assignCoordinator` + `PositionSuccession`; `uk_program_single_current_tenure` | OrganizationModelTest |
| RN-024 | Un Coordinador de Programa no podrá ser Rector. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-025 | Un Coordinador de Programa no podrá ser Coordinador Académico. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-026 | Un Coordinador de Programa no podrá ser Coordinador de Facultad. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-027 | Un Coordinador de Programa no podrá ser Docente. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-028 | Un Coordinador de Programa no podrá ser Estudiante. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-029 | Un Docente no podrá ser Rector. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-030 | Un Docente no podrá ser Coordinador Académico. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-031 | Un Docente no podrá ser Coordinador de Facultad. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-032 | Un Docente no podrá ser Coordinador de Programa. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-033 | Un Docente no podrá ser Estudiante. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-034 | Un Estudiante no podrá ser Rector. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-035 | Un Estudiante no podrá ser Coordinador Académico. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-036 | Un Estudiante no podrá ser Coordinador de Facultad. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-037 | Un Estudiante no podrá ser Coordinador de Programa. | Campo único `Role` en `UserAccount` | RoleTest |
| RN-038 | Un Estudiante no podrá ser Docente. | Campo único `Role` en `UserAccount` | RoleTest |

## Facultades

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-039 | Una Facultad deberá poseer un identificador único. | `Faculty` (id UUID, nombre obligatorio, coordinador vigente) | OrganizationModelTest, OrganizationIntegrationTest |
| RN-040 | Una Facultad deberá tener un nombre. | `Faculty` (id UUID, nombre obligatorio, coordinador vigente) | OrganizationModelTest, OrganizationIntegrationTest |
| RN-041 | Una Facultad podrá contener múltiples Programas Académicos. | `Faculty` (id UUID, nombre obligatorio, coordinador vigente) | OrganizationModelTest, OrganizationIntegrationTest |
| RN-042 | Debe ser posible identificar el Coordinador responsable de una Facultad. | `Faculty` (id UUID, nombre obligatorio, coordinador vigente) | OrganizationModelTest, OrganizationIntegrationTest |
| RN-043 | Debe existir trazabilidad histórica de los Coordinadores de Facultad. | `PositionSuccession.history`; eventos `FacultyCoordinator*` auditados | OrganizationModelTest |
| RN-044 | La sustitución de un Coordinador de Facultad deberá quedar auditada. | `PositionSuccession.history`; eventos `FacultyCoordinator*` auditados | OrganizationModelTest |
| RN-045 | Una Facultad puede existir sin Programas únicamente durante procesos de creación institucional. | `Faculty.create` no exige programas | OrganizationModelTest |

## Programas académicos

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-046 | Todo Programa Académico deberá pertenecer a una Facultad. | `AcademicProgram.createIn(Faculty, ...)`; FK obligatoria `faculty_id` | OrganizationModelTest |
| RN-047 | Todo Programa Académico deberá poseer un identificador único. | `AcademicProgram.createIn(Faculty, ...)`; FK obligatoria `faculty_id` | OrganizationModelTest |
| RN-048 | Todo Programa Académico deberá tener nombre. | `AcademicProgram.createIn(Faculty, ...)`; FK obligatoria `faculty_id` | OrganizationModelTest |
| RN-049 | Todo Programa Académico deberá estar asociado a exactamente una Facultad. | `AcademicProgram.createIn(Faculty, ...)`; FK obligatoria `faculty_id` | OrganizationModelTest |
| RN-050 | Debe ser posible identificar el Coordinador responsable de un Programa. | `AcademicProgram.createIn(Faculty, ...)`; FK obligatoria `faculty_id` | OrganizationModelTest |
| RN-051 | Debe existir trazabilidad histórica de Coordinadores de Programa. | `PositionSuccession.history`; eventos `ProgramCoordinator*` auditados | OrganizationModelTest |
| RN-052 | Los cambios de Coordinador deberán quedar auditados. | `PositionSuccession.history`; eventos `ProgramCoordinator*` auditados | OrganizationModelTest |

## Estudiantes y docentes

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-053 | Un Estudiante podrá pertenecer simultáneamente a múltiples Programas Académicos. | `ProgramMembership` por vínculo; desvincular cierra el registro, no lo borra | OrganizationModelTest, OrganizationIntegrationTest |
| RN-054 | Debe ser posible identificar todos los Programas Académicos asociados a un Estudiante. | `ProgramMembership` por vínculo; desvincular cierra el registro, no lo borra | OrganizationModelTest, OrganizationIntegrationTest |
| RN-055 | Debe ser posible identificar todos los Estudiantes pertenecientes a un Programa. | `ProgramMembership` por vínculo; desvincular cierra el registro, no lo borra | OrganizationModelTest, OrganizationIntegrationTest |
| RN-056 | La vinculación entre Estudiante y Programa deberá ser trazable. | `ProgramMembership` por vínculo; desvincular cierra el registro, no lo borra | OrganizationModelTest, OrganizationIntegrationTest |
| RN-057 | Toda desvinculación deberá conservar historial. | `ProgramMembership` por vínculo; desvincular cierra el registro, no lo borra | OrganizationModelTest, OrganizationIntegrationTest |
| RN-058 | Un Estudiante podrá otorgar Kudos. | Cualquier cuenta activa publica y reporta (`KudoService`, `ReportService`) | RecognitionIntegrationTest |
| RN-059 | Un Estudiante podrá recibir Kudos. | Cualquier cuenta activa publica y reporta (`KudoService`, `ReportService`) | RecognitionIntegrationTest |
| RN-060 | Un Estudiante podrá reportar contenido. | Cualquier cuenta activa publica y reporta (`KudoService`, `ReportService`) | RecognitionIntegrationTest |
| RN-061 | Un Docente podrá pertenecer simultáneamente a múltiples Programas Académicos. | `ProgramMembership` con tipo `TEACHER` | OrganizationModelTest |
| RN-062 | Debe ser posible identificar todos los Programas asociados a un Docente. | `ProgramMembership` con tipo `TEACHER` | OrganizationModelTest |
| RN-063 | Debe ser posible identificar todos los Docentes de un Programa Académico. | `ProgramMembership` con tipo `TEACHER` | OrganizationModelTest |
| RN-064 | La vinculación entre Docente y Programa deberá ser trazable. | `ProgramMembership` con tipo `TEACHER` | OrganizationModelTest |
| RN-065 | Toda desvinculación deberá conservar historial. | `ProgramMembership` con tipo `TEACHER` | OrganizationModelTest |
| RN-066 | Un Docente podrá otorgar Kudos. | Cualquier cuenta activa publica y reporta | RecognitionIntegrationTest |
| RN-067 | Un Docente podrá recibir Kudos. | Cualquier cuenta activa publica y reporta | RecognitionIntegrationTest |
| RN-068 | Un Docente podrá reportar contenido. | Cualquier cuenta activa publica y reporta | RecognitionIntegrationTest |

## Registro de usuarios

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-069 | Todo usuario deberá registrarse utilizando correo institucional. | `InstitutionalEmail` y `MobilePhone` obligatorios en `NewAccountData` | ContactValueObjectsTest |
| RN-070 | Todo usuario deberá registrar un número celular. | `InstitutionalEmail` y `MobilePhone` obligatorios en `NewAccountData` | ContactValueObjectsTest |
| RN-071 | El correo institucional será obligatorio. | `InstitutionalEmail` y `MobilePhone` obligatorios en `NewAccountData` | ContactValueObjectsTest |
| RN-072 | El número celular será obligatorio. | `InstitutionalEmail` y `MobilePhone` obligatorios en `NewAccountData` | ContactValueObjectsTest |
| RN-073 | El correo institucional deberá ser único. | `AccountDataValidator`; `uk_user_account_email` y `uk_user_account_phone` | OnboardingIntegrationTest |
| RN-074 | El número celular deberá ser único. | `AccountDataValidator`; `uk_user_account_email` y `uk_user_account_phone` | OnboardingIntegrationTest |
| RN-075 | No podrá existir una cuenta activa sin correo institucional. | Columnas `NOT NULL`; `ck_user_account_active_requirements` | UserAccountTest |
| RN-076 | No podrá existir una cuenta activa sin número celular. | Columnas `NOT NULL`; `ck_user_account_active_requirements` | UserAccountTest |

## Verificación de identidad

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-077 | Todo correo institucional deberá ser validado. | `ContactVerification.confirm`; `UserAccount.verifyEmail/verifyPhone` | UserAccountTest, OnboardingIntegrationTest |
| RN-078 | Todo número celular deberá ser validado. | `ContactVerification.confirm`; `UserAccount.verifyEmail/verifyPhone` | UserAccountTest, OnboardingIntegrationTest |
| RN-079 | La verificación de correo es obligatoria. | `ContactVerification.confirm`; `UserAccount.verifyEmail/verifyPhone` | UserAccountTest, OnboardingIntegrationTest |
| RN-080 | La verificación de celular es obligatoria. | `ContactVerification.confirm`; `UserAccount.verifyEmail/verifyPhone` | UserAccountTest, OnboardingIntegrationTest |
| RN-081 | Si el correo cambia deberá volver a verificarse. | `UserAccount.updatePersonalInformation` reinicia la verificación del contacto cambiado | UserAccountTest |
| RN-082 | Si el celular cambia deberá volver a verificarse. | `UserAccount.updatePersonalInformation` reinicia la verificación del contacto cambiado | UserAccountTest |
| RN-083 | La modificación de datos de contacto deberá quedar auditada. | Evento `PersonalInformationUpdated` auditado | UserAccountTest |

## Activación de cuenta

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-084 | Una cuenta aprobada no implica una cuenta activa. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |
| RN-085 | Una cuenta activa requiere aprobación institucional. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |
| RN-086 | Una cuenta activa requiere correo validado. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |
| RN-087 | Una cuenta activa requiere celular validado. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |
| RN-088 | Las tres condiciones son obligatorias. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |
| RN-089 | La ausencia de cualquiera de ellas impedirá el ingreso. | `UserAccount` solo pasa a `ACTIVE` con las tres condiciones; `AuthenticationService` exige cuenta activa | UserAccountTest, OnboardingIntegrationTest |

## Flujos de aprobación

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-090 | Los Estudiantes serán aprobados por Coordinadores de Programa. | `Role.manages` + `OrganizationalAuthority.ensureCanManage` | RoleTest, OrganizationalAuthorityTest |
| RN-091 | Los Coordinadores de Programa serán aprobados por Coordinadores de Facultad. | `Role.manages` + `OrganizationalAuthority.ensureCanManage` | RoleTest, OrganizationalAuthorityTest |
| RN-092 | Los Coordinadores de Facultad serán aprobados por Coordinadores Académicos. | `Role.manages` + `OrganizationalAuthority.ensureCanManage` | RoleTest, OrganizationalAuthorityTest |
| RN-093 | Los Coordinadores Académicos serán aprobados por el Rector. | `Role.manages` + `OrganizationalAuthority.ensureCanManage` | RoleTest, OrganizationalAuthorityTest |
| RN-094 | Los Rectores serán registrados por Administradores. | `PrivilegedAccountRegistrationService`; `Role.allowsSelfRegistration` | RoleTest, OnboardingIntegrationTest |
| RN-095 | Los Administradores serán registrados por otros Administradores. | `PrivilegedAccountRegistrationService`; `Role.allowsSelfRegistration` | RoleTest, OnboardingIntegrationTest |

## Auditoría

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-096 | Toda aprobación deberá quedar auditada. | `AuditEventListener` registra cada evento en la misma transacción | ModerationIntegrationTest |
| RN-097 | Toda modificación de datos deberá quedar auditada. | `AuditEventListener` registra cada evento en la misma transacción | ModerationIntegrationTest |
| RN-098 | Toda activación deberá quedar auditada. | `AuditEventListener` registra cada evento en la misma transacción | ModerationIntegrationTest |
| RN-099 | Toda desactivación deberá quedar auditada. | `AuditEventListener` registra cada evento en la misma transacción | ModerationIntegrationTest |
| RN-100 | Toda acción administrativa relevante deberá conservar trazabilidad histórica. | `AuditEventListener` registra cada evento en la misma transacción | ModerationIntegrationTest |

## Información personal

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-101 | Todo usuario podrá consultar su propia información personal. | `ProfileService`, `GET /api/profile` | OnboardingIntegrationTest |
| RN-102 | Todo usuario podrá actualizar su propia información personal. | `ProfileService`, `GET /api/profile` | OnboardingIntegrationTest |
| RN-103 | Todo usuario podrá consultar el estado de sus validaciones. | `ProfileService`, `GET /api/profile` | OnboardingIntegrationTest |
| RN-104 | Todo usuario podrá actualizar su correo institucional. | `UserAccount.updatePersonalInformation` | UserAccountTest |
| RN-105 | Todo usuario podrá actualizar su número celular. | `UserAccount.updatePersonalInformation` | UserAccountTest |
| RN-106 | Todo usuario podrá consultar su historial de actividad. | `ProfileService` + `AuditTrail.byActor` | OnboardingIntegrationTest |
| RN-107 | Ningún usuario podrá modificar directamente su rol institucional. | `PersonalInformationChange` no incluye el rol | UserAccountTest |
| RN-108 | Ningún usuario podrá aprobarse a sí mismo. | `RegistrationRequest.approve`; `ck_registration_not_self_decided` | SupportingAggregatesTest |
| RN-109 | Toda modificación realizada por un usuario sobre su información personal deberá registrarse en auditoría. | Evento `PersonalInformationUpdated` auditado | UserAccountTest |
| RN-110 | La actualización del correo institucional generará una nueva validación pendiente. | `ContactVerification.awaiting` tras el cambio | UserAccountTest |
| RN-111 | La actualización del número celular generará una nueva validación pendiente. | `ContactVerification.awaiting` tras el cambio | UserAccountTest |

## Ámbitos de gestión

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-112 | Un Coordinador de Programa podrá gestionar información únicamente de estudiantes pertenecientes a su Programa Académico. | `OrganizationalAuthority.coordinatesAnyProgramOf` | OrganizationalAuthorityTest |
| RN-113 | Un Coordinador de Programa podrá gestionar información únicamente de docentes asociados a su Programa Académico. | `OrganizationalAuthority.coordinatesAnyProgramOf` | OrganizationalAuthorityTest |
| RN-114 | Un Coordinador de Programa no podrá gestionar estudiantes pertenecientes exclusivamente a otros Programas Académicos. | `OrganizationalAuthority.coordinatesAnyProgramOf` | OrganizationalAuthorityTest |
| RN-115 | Un Coordinador de Programa no podrá gestionar docentes asociados exclusivamente a otros Programas Académicos. | `OrganizationalAuthority.coordinatesAnyProgramOf` | OrganizationalAuthorityTest |
| RN-116 | Debe existir trazabilidad de todas las acciones realizadas por un Coordinador de Programa. | Eventos con `actorId`, auditados | ModerationIntegrationTest |
| RN-117 | Un Coordinador de Facultad podrá gestionar únicamente Coordinadores de Programa pertenecientes a los Programas Académicos de su Facultad. | `OrganizationalAuthority.coordinatesFacultyOfAnyProgram`; `ManagedPopulation` | OrganizationalAuthorityTest |
| RN-118 | Un Coordinador de Facultad podrá consultar todos los Programas Académicos de su Facultad. | `OrganizationalAuthority.coordinatesFacultyOfAnyProgram`; `ManagedPopulation` | OrganizationalAuthorityTest |
| RN-119 | Un Coordinador de Facultad podrá consultar los Coordinadores de Programa de su Facultad. | `OrganizationalAuthority.coordinatesFacultyOfAnyProgram`; `ManagedPopulation` | OrganizationalAuthorityTest |
| RN-120 | Un Coordinador de Facultad no podrá gestionar Coordinadores pertenecientes a otras Facultades. | `OrganizationalAuthority.coordinatesFacultyOfAnyProgram`; `ManagedPopulation` | OrganizationalAuthorityTest |
| RN-121 | Un Coordinador Académico podrá gestionar Coordinadores de Facultad de toda la Universidad. | `Role.manages`; consultas públicas de `OrganizationService`; auditoría por eventos | RoleTest, OrganizationIntegrationTest |
| RN-122 | Un Coordinador Académico podrá consultar todas las Facultades. | `Role.manages`; consultas públicas de `OrganizationService`; auditoría por eventos | RoleTest, OrganizationIntegrationTest |
| RN-123 | Un Coordinador Académico podrá consultar todos los Coordinadores de Facultad. | `Role.manages`; consultas públicas de `OrganizationService`; auditoría por eventos | RoleTest, OrganizationIntegrationTest |
| RN-124 | Toda modificación realizada sobre Coordinadores de Facultad deberá quedar auditada. | `Role.manages`; consultas públicas de `OrganizationService`; auditoría por eventos | RoleTest, OrganizationIntegrationTest |
| RN-125 | El Rector podrá gestionar exclusivamente Coordinadores Académicos. | `Role.manages(ACADEMIC_COORDINATOR)` para Rector | RoleTest |
| RN-126 | El Rector podrá aprobar registros de Coordinadores Académicos. | `Role.manages(ACADEMIC_COORDINATOR)` para Rector | RoleTest |
| RN-127 | El Rector podrá consultar información de los Coordinadores Académicos. | `Role.manages(ACADEMIC_COORDINATOR)` para Rector | RoleTest |
| RN-128 | Toda gestión realizada por el Rector deberá quedar registrada en auditoría. | `Role.manages(ACADEMIC_COORDINATOR)` para Rector | RoleTest |
| RN-129 | Los Administradores gestionarán Administradores y Rectores. | `Role.manages` para Administrador | RoleTest |
| RN-130 | Los Administradores gestionarán parámetros globales de la plataforma. | Variables de entorno y `KudosProperties` (ver decisión D-05 del análisis) | No aplica prueba automática |
| RN-131 | Los Administradores gestionarán configuraciones técnicas del sistema. | Variables de entorno y `KudosProperties` (ver decisión D-05 del análisis) | No aplica prueba automática |

## Categorías

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-132 | Las categorías de Kudos representan clasificaciones institucionales de reconocimiento. | `Category` con estado activo/inactivo | SupportingAggregatesTest |
| RN-133 | Una categoría podrá encontrarse activa o inactiva. | `Category` con estado activo/inactivo | SupportingAggregatesTest |
| RN-134 | Solo los Administradores podrán crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-135 | El Rector podrá crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-136 | El Coordinador Académico podrá crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-137 | Los Coordinadores de Facultad no podrán crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-138 | Los Coordinadores de Programa no podrán crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-139 | Los Docentes no podrán crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-140 | Los Estudiantes no podrán crear categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-141 | Solo Administradores, Rector y Coordinador Académico podrán modificar categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-142 | Solo Administradores, Rector y Coordinador Académico podrán activar categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-143 | Solo Administradores, Rector y Coordinador Académico podrán desactivar categorías. | `Capability.MANAGE_CATEGORIES` en `Category.create/modify/activate/deactivate` | SupportingAggregatesTest, RoleTest |
| RN-144 | Las categorías desactivadas no podrán utilizarse para nuevos Kudos. | `Category.ensureUsableForNewKudos` | KudoAndReplyTest |
| RN-145 | Los Kudos históricos conservarán la categoría utilizada en el momento de su creación. | `Kudo.categoryNameAtPublication`, copiado al publicar | KudoAndReplyTest |

## Kudos

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-146 | Un Kudo representa un reconocimiento institucional. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-147 | Todo Kudo deberá tener un autor. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-148 | Todo Kudo deberá tener un destinatario. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-149 | Todo Kudo deberá estar asociado a una categoría. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-150 | Todo Kudo deberá registrar fecha de creación. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-151 | Todo Kudo deberá conservar trazabilidad histórica. | `Kudo.publish` exige autor, destinatario, categoría y fecha | KudoAndReplyTest |
| RN-152 | El contenido textual de un Kudo no tendrá límite de longitud. | Columna `TEXT`; `TextRequirements` solo exige no vacío; la UI recorta visualmente con CSS | KudoAndReplyTest |
| RN-153 | El sistema no podrá truncar automáticamente el contenido de un Kudo. | Columna `TEXT`; `TextRequirements` solo exige no vacío; la UI recorta visualmente con CSS | KudoAndReplyTest |
| RN-154 | El contenido completo deberá permanecer disponible para consulta. | Columna `TEXT`; `TextRequirements` solo exige no vacío; la UI recorta visualmente con CSS | KudoAndReplyTest |
| RN-155 | Todo Kudo podrá ser visualizado por la comunidad autenticada. | `GET /api/kudos` con autenticación obligatoria | RecognitionIntegrationTest |
| RN-156 | Todo Kudo podrá recibir réplicas. | `Publication`: réplicas, calificación, reporte y veto | KudoAndReplyTest, ModerationTest |
| RN-157 | Todo Kudo podrá recibir calificaciones. | `Publication`: réplicas, calificación, reporte y veto | KudoAndReplyTest, ModerationTest |
| RN-158 | Todo Kudo podrá ser reportado. | `Publication`: réplicas, calificación, reporte y veto | KudoAndReplyTest, ModerationTest |
| RN-159 | Todo Kudo podrá ser vetado conforme a las políticas institucionales. | `Publication`: réplicas, calificación, reporte y veto | KudoAndReplyTest, ModerationTest |
| RN-160 | Las operaciones asociadas al ciclo de vida de un Kudo deberán generar eventos auditables. | `Kudo` registra eventos en `AggregateRoot` | KudoAndReplyTest |

## Réplicas

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-161 | Los usuarios podrán crear réplicas sobre un Kudo. | `Reply.onKudo` y `Reply.answering` | KudoAndReplyTest |
| RN-162 | Los usuarios podrán crear réplicas sobre otras réplicas. | `Reply.onKudo` y `Reply.answering` | KudoAndReplyTest |
| RN-163 | Las conversaciones podrán crecer indefinidamente. | Sin topes; `Conversation` recorre con pila, sin recursión | ConversationTest |
| RN-164 | No existirá límite en la profundidad de una conversación. | Sin topes; `Conversation` recorre con pila, sin recursión | ConversationTest |
| RN-165 | No existirá límite en la cantidad de réplicas que un usuario pueda realizar. | Sin topes; `Conversation` recorre con pila, sin recursión | ConversationTest |
| RN-166 | Una misma persona podrá realizar múltiples réplicas sobre un mismo Kudo. | Sin topes; `Conversation` recorre con pila, sin recursión | ConversationTest |
| RN-167 | Una misma persona podrá realizar múltiples réplicas sobre la misma réplica. | Sin topes; `Conversation` recorre con pila, sin recursión | ConversationTest |
| RN-168 | Toda réplica deberá tener un autor. | `Reply` exige autor y fecha; columna `TEXT` | KudoAndReplyTest |
| RN-169 | Toda réplica deberá tener una fecha de creación. | `Reply` exige autor y fecha; columna `TEXT` | KudoAndReplyTest |
| RN-170 | El contenido textual de una réplica no tendrá límite de longitud. | `Reply` exige autor y fecha; columna `TEXT` | KudoAndReplyTest |
| RN-171 | Toda réplica podrá ser calificada. | `Publication` (herencia común con `Kudo`) | RatingTest, ModerationTest |
| RN-172 | Toda réplica podrá ser reportada. | `Publication` (herencia común con `Kudo`) | RatingTest, ModerationTest |
| RN-173 | Toda réplica podrá ser vetada conforme a las políticas institucionales. | `Publication` (herencia común con `Kudo`) | RatingTest, ModerationTest |
| RN-174 | Toda réplica deberá conservar historial y trazabilidad. | Las réplicas no se borran; eventos auditados | ModerationIntegrationTest |

## Calificaciones

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-175 | Los Kudos podrán recibir calificaciones. | `Rating.register` sobre `Publication` | RatingTest |
| RN-176 | Las réplicas podrán recibir calificaciones. | `Rating.register` sobre `Publication` | RatingTest |
| RN-177 | La escala de calificación será de 1 a 5 estrellas. | `StarRating`; `ck_rating_stars` | RatingTest |
| RN-178 | Una persona podrá calificar un mismo Kudo solamente una vez. | `RatingService` actualiza la existente; `uk_rating_one_per_rater_and_content` | RatingTest, RecognitionIntegrationTest |
| RN-179 | Una persona podrá calificar una misma réplica solamente una vez. | `RatingService` actualiza la existente; `uk_rating_one_per_rater_and_content` | RatingTest, RecognitionIntegrationTest |
| RN-180 | No podrán existir dos calificaciones activas de una misma persona para un mismo Kudo. | `RatingService` actualiza la existente; `uk_rating_one_per_rater_and_content` | RatingTest, RecognitionIntegrationTest |
| RN-181 | No podrán existir dos calificaciones activas de una misma persona para una misma réplica. | `RatingService` actualiza la existente; `uk_rating_one_per_rater_and_content` | RatingTest, RecognitionIntegrationTest |
| RN-182 | Toda calificación deberá registrar la fecha en que fue realizada. | `Rating` (fechas, `raterId`, `ContentReference`) | RatingTest |
| RN-183 | Toda calificación deberá registrar quién la realizó. | `Rating` (fechas, `raterId`, `ContentReference`) | RatingTest |
| RN-184 | Toda calificación deberá asociarse al contenido calificado. | `Rating` (fechas, `raterId`, `ContentReference`) | RatingTest |
| RN-185 | Las calificaciones deberán participar en el cálculo de indicadores definidos por el equipo. | `RatingSummary` (promedio y cantidad) | RatingTest |

## Reportes

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-186 | Cualquier usuario autenticado podrá reportar un Kudo. | `ContentReport.file` con autor, fecha y `ReportReason` | ModerationTest, ModerationIntegrationTest |
| RN-187 | Cualquier usuario autenticado podrá reportar una réplica. | `ContentReport.file` con autor, fecha y `ReportReason` | ModerationTest, ModerationIntegrationTest |
| RN-188 | Todo reporte deberá registrar la persona que reporta. | `ContentReport.file` con autor, fecha y `ReportReason` | ModerationTest, ModerationIntegrationTest |
| RN-189 | Todo reporte deberá registrar la fecha. | `ContentReport.file` con autor, fecha y `ReportReason` | ModerationTest, ModerationIntegrationTest |
| RN-190 | Todo reporte deberá registrar un motivo. | `ContentReport.file` con autor, fecha y `ReportReason` | ModerationTest, ModerationIntegrationTest |

## Moderación

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-191 | Los Kudos podrán ser vetados por razones institucionales. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-192 | Las réplicas podrán ser vetadas por razones institucionales. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-193 | Solo el Rector podrá vetar cualquier Kudo de la plataforma. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-194 | Solo el Coordinador Académico podrá vetar cualquier Kudo de la plataforma. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-195 | Solo el Rector podrá vetar cualquier réplica de la plataforma. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-196 | Solo el Coordinador Académico podrá vetar cualquier réplica de la plataforma. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-197 | Los Administradores no podrán vetar contenido. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-198 | Los Coordinadores de Facultad no podrán vetar contenido. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-199 | Los Coordinadores de Programa no podrán vetar contenido. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |
| RN-200 | Docentes y Estudiantes no podrán vetar contenido. | `Capability.MODERATE_CONTENT` en `Publication.veto` | ModerationTest, RoleTest |

## Estados del contenido y de los reportes

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-201 | Todo Kudo inicia su ciclo de vida en estado creado. | Estado inicial `CREATED` | ModerationStatusTest |
| RN-202 | Un Kudo visible puede ser reportado. | Patrón State en `ModerationStatus` | ModerationStatusTest |
| RN-203 | Un Kudo reportado puede ingresar a revisión. | Patrón State en `ModerationStatus` | ModerationStatusTest |
| RN-204 | Un Kudo puede ser vetado. | Patrón State en `ModerationStatus` | ModerationStatusTest |
| RN-205 | Un Kudo vetado deja de estar disponible para interacción normal. | `Publication.ensureAcceptsInteraction` | KudoAndReplyTest |
| RN-206 | Toda réplica inicia como creada. | `ModerationStatus`, compartido con `Kudo` | ModerationStatusTest |
| RN-207 | Una réplica visible puede ser reportada. | `ModerationStatus`, compartido con `Kudo` | ModerationStatusTest |
| RN-208 | Una réplica reportada puede pasar a revisión. | `ModerationStatus`, compartido con `Kudo` | ModerationStatusTest |
| RN-209 | Una réplica puede ser vetada. | `ModerationStatus`, compartido con `Kudo` | ModerationStatusTest |
| RN-210 | Todo reporte inicia en estado pendiente. | `ReportStatus` y `ContentReport.markInReview/accept/reject` | ModerationTest |
| RN-211 | Un reporte pendiente podrá ser analizado. | `ReportStatus` y `ContentReport.markInReview/accept/reject` | ModerationTest |
| RN-212 | Una decisión deberá producir aceptación o rechazo. | `ReportStatus` y `ContentReport.markInReview/accept/reject` | ModerationTest |

## Eventos del dominio

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-213 | Debe generarse un evento cuando un usuario se registra. | `UserRegistered` | UserAccountTest |
| RN-214 | Debe generarse un evento cuando un usuario es aprobado. | `UserApproved` | UserAccountTest |
| RN-215 | Debe generarse un evento cuando una cuenta es activada. | `AccountActivated` | UserAccountTest |
| RN-216 | Debe generarse un evento cuando una cuenta es suspendida. | `AccountSuspended` | UserAccountTest |
| RN-217 | Debe generarse un evento cuando una cuenta es desactivada. | `AccountDeactivated` | UserAccountTest |
| RN-218 | Debe generarse un evento cuando una Facultad es creada. | `FacultyCreated` | OrganizationModelTest |
| RN-219 | Debe generarse un evento cuando se asigna un Coordinador de Facultad. | `FacultyCoordinatorAssigned` | OrganizationModelTest |
| RN-220 | Debe generarse un evento cuando se reemplaza un Coordinador de Facultad. | `FacultyCoordinatorReplaced` | OrganizationModelTest |
| RN-221 | Debe generarse un evento cuando se crea un Programa Académico. | `ProgramCreated` | OrganizationModelTest |
| RN-222 | Debe generarse un evento cuando se asigna un Coordinador de Programa. | `ProgramCoordinatorAssigned` | OrganizationModelTest |
| RN-223 | Debe generarse un evento cuando se reemplaza un Coordinador de Programa. | `ProgramCoordinatorReplaced` | OrganizationModelTest |
| RN-224 | Debe generarse un evento cuando una categoría es creada. | `CategoryCreated` | SupportingAggregatesTest |
| RN-225 | Debe generarse un evento cuando una categoría es modificada. | `CategoryModified` | SupportingAggregatesTest |
| RN-226 | Debe generarse un evento cuando una categoría es activada. | `CategoryActivated` | SupportingAggregatesTest |
| RN-227 | Debe generarse un evento cuando una categoría es desactivada. | `CategoryDeactivated` | SupportingAggregatesTest |
| RN-228 | Debe generarse un evento cuando se publica un Kudo. | `KudoPublished` | KudoAndReplyTest |
| RN-229 | Debe generarse un evento cuando un Kudo es consultado. | `KudoViewed` | KudoAndReplyTest |
| RN-230 | Debe generarse un evento cuando un Kudo es reportado. | `KudoReported` | ModerationTest |
| RN-231 | Debe generarse un evento cuando un Kudo es vetado. | `KudoVetoed` | ModerationTest |
| RN-232 | Debe generarse un evento cuando una réplica es publicada. | `ReplyPublished` | KudoAndReplyTest |
| RN-233 | Debe generarse un evento cuando una réplica recibe una respuesta. | `ReplyAnswered` | KudoAndReplyTest |
| RN-234 | Debe generarse un evento cuando una réplica es reportada. | `ReplyReported` | ModerationTest |
| RN-235 | Debe generarse un evento cuando una réplica es vetada. | `ReplyVetoed` | ModerationTest |
| RN-236 | Debe generarse un evento cuando una calificación es registrada. | `RatingRegistered` | RatingTest |
| RN-237 | Debe generarse un evento cuando se modifica una calificación. | `RatingUpdated` | RatingTest |
| RN-238 | Debe generarse un evento cuando se recalculan indicadores derivados de calificaciones. | `RatingIndicatorRecalculated` | RatingTest |
| RN-239 | Debe generarse un evento cuando se inicia una revisión. | `ReviewStarted` | ModerationTest |
| RN-240 | Debe generarse un evento cuando una revisión es cerrada. | `ReviewClosed` | ModerationTest |
| RN-241 | Debe generarse un evento cuando un contenido es vetado. | `KudoVetoed` o `ReplyVetoed` | ModerationTest |
| RN-242 | Debe generarse un evento cuando un contenido es liberado tras revisión. | `ContentReleased` | ModerationTest |

## Restricciones de moderación y trazabilidad

| Regla | Enunciado | Dónde se garantiza | Prueba |
|---|---|---|---|
| RN-243 | Solamente Rector y Coordinador Académico pueden vetar contenido. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-244 | Los Administradores no participan en decisiones de moderación. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-245 | Los Coordinadores de Facultad no participan en decisiones de moderación. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-246 | Los Coordinadores de Programa no participan en decisiones de moderación. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-247 | Docentes no participan en decisiones de moderación. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-248 | Estudiantes no participan en decisiones de moderación. | `Capability.MODERATE_CONTENT` solo en Rector y Coordinador Académico | RoleTest, ModerationIntegrationTest |
| RN-249 | Toda decisión de moderación deberá quedar auditada. | `ModerationDecision` con justificación + `AuditEventListener` con evidencia JSON | ModerationIntegrationTest |
| RN-250 | Toda auditoría deberá conservar la evidencia necesaria para reconstruir el proceso de decisión aplicado al contenido. | `ModerationDecision` con justificación + `AuditEventListener` con evidencia JSON | ModerationIntegrationTest |
