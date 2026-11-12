# Diagramas UML

## 1. Casos de uso

Mermaid no tiene notación propia de casos de uso; se representa con actores a la izquierda y casos agrupados.

```mermaid
flowchart LR
    Aspirante([Aspirante])
    Usuario([Usuario autenticado])
    Coordinadores([Coordinadores, Rector])
    Admin([Administrador])
    Moderador([Rector o Coordinador Académico])
    Directivo([Administrador, Rector o Coordinador Académico])
    CoordinadorAcademico([Coordinador Académico])

    subgraph Identidad
        R1(Solicitar registro)
        R2(Verificar correo y celular)
        R3(Ingresar)
        R4(Consultar y actualizar perfil)
        R5(Aprobar o rechazar solicitud)
        R6(Suspender, reactivar o desactivar cuenta)
        R7(Registrar Rector o Administrador)
    end
    subgraph Reconocimiento
        CU1(CU-001 Crear Kudo)
        CU2(CU-002 Consultar Kudo)
        CU3(CU-003 Consultar historial)
        CU4(CU-004 Crear réplica)
        CU5(CU-005 Responder réplica)
        CU6(CU-006 Consultar conversación)
        CU7(CU-007 y 008 Calificar)
        CU9(CU-009 Consultar valoraciones)
        CU10(CU-010 y 011 Reportar)
    end
    subgraph Gobierno
        CU12(CU-012 Consultar reportes)
        CU13(CU-013 y 014 Vetar)
        CU15(CU-015 Histórico de vetos)
        G1(Gestionar categorías)
        G2(Consultar auditoría)
        G3(Gestionar facultades y programas)
    end

    Aspirante --> R1 & R2
    Usuario --> R3 & R4 & CU1 & CU2 & CU3 & CU4 & CU5 & CU6 & CU7 & CU9 & CU10
    Coordinadores --> R5 & R6
    Admin --> R7
    Moderador --> CU12 & CU13 & CU15
    Directivo --> G1 & G2
    Admin --> G3
    CoordinadorAcademico --> G3
```

## 2. Clases del dominio

```mermaid
classDiagram
    class AggregateRoot {
        <<abstract>>
        #recordEvent(event)
        +pullDomainEvents() List
    }
    class UserAccount {
        +selfRegister()$ UserAccount
        +registerByAdministrator()$ UserAccount
        +verifyEmail(code, now)
        +verifyPhone(code, now)
        +approve(approverId, now)
        +updatePersonalInformation(change, now)
        +suspend(manager, reason, now)
        +reactivate(manager, now)
        +deactivate(manager, reason, now)
        +ensureManages(role)
        +ensureHasCapability(capability)
        +ensureActive()
    }
    class Role {
        <<enumeration>>
        ADMINISTRATOR
        RECTOR
        ACADEMIC_COORDINATOR
        FACULTY_COORDINATOR
        PROGRAM_COORDINATOR
        TEACHER
        STUDENT
        +grants(capability) boolean
        +manages(other) boolean
        +allowsSelfRegistration() boolean
    }
    class Publication {
        <<abstract>>
        +receiveReport(reporterId, now)
        +startReview(moderator, now)
        +release(moderator, now)
        +veto(moderator, rationale, now)
        +ensureAcceptsInteraction()
        #reportedEvent()* DomainEvent
        #vetoedEvent()* DomainEvent
    }
    class Kudo {
        +publish()$ Kudo
        +registerView(viewerId, now)
    }
    class Reply {
        +onKudo()$ Reply
        +answering()$ Reply
    }
    class ModerationStatus {
        <<enumeration>>
        CREATED
        VISIBLE
        REPORTED
        UNDER_REVIEW
        VETOED
        +publish() ModerationStatus
        +report() ModerationStatus
        +startReview() ModerationStatus
        +release() ModerationStatus
        +veto() ModerationStatus
    }
    class Conversation {
        +of(kudoId, replies)$ Conversation
        +entriesInReadingOrder() List
    }
    class Category {
        +create()$ Category
        +modify(editor, name, description, now)
        +activate(editor, now)
        +deactivate(editor, now)
        +ensureUsableForNewKudos()
    }
    class Rating {
        +register()$ Rating
        +change(rater, content, stars, now)
    }
    class StarRating {
        <<value>>
        int stars
    }
    class ContentReport {
        +file()$ ContentReport
        +markInReview()
        +accept(moderatorId, now)
        +reject(moderatorId, now)
    }
    class ModerationDecision {
        +startReview()$
        +closeReview()$ ModerationDecision
        +vetoDirectly()$ ModerationDecision
    }
    class ContentReference {
        <<value>>
        ContentType type
        UUID id
    }
    class Faculty {
        +create()$ Faculty
        +rename(editor, name, now)
        +assignCoordinator(id, by, now) Optional
    }
    class AcademicProgram {
        +createIn(faculty, creator, name, now)$ AcademicProgram
        +rename(editor, name, now)
        +assignCoordinator(id, by, now) Optional
    }
    class UniversityPosition {
        +establish(role)$ UniversityPosition
        +assignHolder(id, by, now) Optional
    }
    class PositionSuccession {
        +handOver(holderId, now) Optional
        +currentHolderId() Optional
        +history() List
    }
    class PositionTenure {
        <<value>>
        UUID holderId
        Instant startedAt
        Instant endedAt
    }
    class ProgramMembership {
        +link()$ ProgramMembership
        +unlink(responsibleId, now)
    }
    class RegistrationRequest {
        +submit()$ RegistrationRequest
        +approve(approverId, now)
        +reject(approverId, reason, now)
    }
    class OrganizationalAuthority {
        <<service>>
        +ensureCanManage(manager, role, affiliation)
        +ensureCoordinates(coordinator, program)
    }

    AggregateRoot <|-- UserAccount
    AggregateRoot <|-- Publication
    AggregateRoot <|-- Category
    AggregateRoot <|-- Rating
    AggregateRoot <|-- ContentReport
    AggregateRoot <|-- ModerationDecision
    AggregateRoot <|-- Faculty
    AggregateRoot <|-- AcademicProgram
    AggregateRoot <|-- UniversityPosition
    AggregateRoot <|-- ProgramMembership
    AggregateRoot <|-- RegistrationRequest
    Publication <|-- Kudo
    Publication <|-- Reply
    Publication *-- ModerationStatus
    UserAccount *-- Role
    Rating *-- StarRating
    Rating *-- ContentReference
    ContentReport *-- ContentReference
    ModerationDecision *-- ContentReference
    Faculty *-- PositionSuccession
    AcademicProgram *-- PositionSuccession
    UniversityPosition *-- PositionSuccession
    PositionSuccession *-- "0..*" PositionTenure
    Kudo ..> Category : valida al publicar
    Reply ..> Kudo : nace en
    Conversation o-- "0..*" Reply
    AcademicProgram ..> Faculty : nace en
    OrganizationalAuthority ..> UserAccount
    OrganizationalAuthority ..> Faculty
    OrganizationalAuthority ..> AcademicProgram
    ModerationDecision ..> Publication
    ModerationDecision ..> ContentReport
```

## 3. Secuencia

### Publicar un Kudo

```mermaid
sequenceDiagram
    actor U as Usuario
    participant C as KudoController
    participant S as KudoService
    participant A as AccountDirectory
    participant K as Kudo
    participant R as KudoRepository
    participant P as DomainEventPublisher
    participant L as Listeners

    U->>C: POST /api/kudos
    C->>S: publish(command)
    S->>A: requireActive(autor) y destinatario
    S->>K: publish(autor, destinatario, categoría, texto, ahora)
    K->>K: valida y pasa a VISIBLE
    K-->>S: kudo con KudoPublished
    S->>R: save(kudo)
    S->>P: publishEventsOf(kudo)
    P->>L: KudoPublished
    L->>L: guarda auditoría y notificación
    S-->>C: id
    C-->>U: 201 Created
```

### Aprobar una solicitud

```mermaid
sequenceDiagram
    actor C as Coordinador
    participant S as ApprovalService
    participant Sol as RegistrationRequest
    participant Auth as OrganizationalAuthority
    participant Cu as UserAccount
    participant P as DomainEventPublisher

    C->>S: POST /registration-requests/{id}/approval
    S->>Sol: buscar solicitud
    S->>Auth: ensureCanManage(aprobador, rol, afiliación)
    alt fuera de su ámbito
        Auth-->>C: 403 ActionNotAllowed
    else autorizado
        S->>Sol: approve(aprobador, ahora)
        S->>Cu: approve(aprobador, ahora)
        Cu->>Cu: si correo y celular verificados, ACTIVE
        S->>P: publica eventos de solicitud y cuenta
        P-->>S: auditoría y notificación
        S->>S: placement.place: crea vínculos o asigna el cargo
        S-->>C: 204
    end
```

### Cerrar una revisión con veto

```mermaid
sequenceDiagram
    actor M as Moderador
    participant S as ModerationService
    participant D as ModerationDecision
    participant Pub as Publication
    participant St as ModerationStatus
    participant Rep as ContentReport

    M->>S: POST /moderation/contents/{tipo}/{id}/decision
    S->>D: closeReview(contenido, reportes, moderador, VETOED, justificación)
    D->>Pub: veto(moderador, justificación, ahora)
    Pub->>Pub: exige MODERATE_CONTENT
    Pub->>St: veto()
    St-->>Pub: VETOED
    D->>Rep: accept(moderador, ahora) por cada reporte abierto
    D-->>S: decisión con ReviewClosed
    S->>S: guarda y publica eventos
    S-->>M: 204
```

## 4. Actividad

### Del registro a la cuenta activa

```mermaid
flowchart TD
    A([Inicio]) --> B[Aspirante envía datos, rol y afiliación]
    B --> C{Correo institucional y datos únicos?}
    C -- No --> X[Se informa el error] --> B
    C -- Sí --> D[Cuenta PENDING_ACTIVATION y solicitud PENDING]
    D --> E[Se emiten códigos de correo y celular]
    E --> F[Aspirante confirma los códigos]
    D --> G{Autoridad de su ámbito decide}
    G -- Rechaza --> H([Solicitud rechazada con motivo])
    G -- Aprueba --> I[Aprobación registrada]
    F --> J{Correo, celular y aprobación completos?}
    I --> J
    J -- No --> K[Sigue pendiente]
    J -- Sí --> L([Cuenta ACTIVE])
```

### Moderación de contenido

```mermaid
flowchart TD
    A([Contenido visible]) --> B[Usuario reporta con motivo]
    B --> C[Contenido REPORTED, reporte PENDING]
    C --> D[Moderador inicia revisión]
    D --> E[Contenido UNDER_REVIEW, reportes IN_REVIEW]
    E --> F{Decisión con justificación}
    F -- Vetar --> G[Contenido VETOED, reportes ACCEPTED]
    F -- Liberar --> H[Contenido VISIBLE, reportes REJECTED]
    G --> I[Decisión y auditoría, aviso al autor]
    H --> J[Decisión y auditoría]
    A -. veto directo .-> G
```

## 5. Estados

### Kudo y réplica

```mermaid
stateDiagram-v2
    [*] --> CREATED
    CREATED --> VISIBLE: publicar
    VISIBLE --> REPORTED: reportar
    REPORTED --> REPORTED: nuevo reporte
    REPORTED --> UNDER_REVIEW: iniciar revisión
    UNDER_REVIEW --> UNDER_REVIEW: nuevo reporte
    UNDER_REVIEW --> VISIBLE: liberar
    VISIBLE --> VETOED: vetar
    REPORTED --> VETOED: vetar
    UNDER_REVIEW --> VETOED: vetar
    VETOED --> [*]
```

### Reporte

```mermaid
stateDiagram-v2
    [*] --> PENDING
    PENDING --> IN_REVIEW: iniciar revisión
    IN_REVIEW --> ACCEPTED: contenido vetado
    IN_REVIEW --> REJECTED: contenido liberado
    PENDING --> ACCEPTED: veto directo
    ACCEPTED --> [*]
    REJECTED --> [*]
```

### Cuenta

```mermaid
stateDiagram-v2
    [*] --> PENDING_ACTIVATION: registro
    PENDING_ACTIVATION --> ACTIVE: correo, celular y aprobación
    ACTIVE --> PENDING_ACTIVATION: cambia correo o celular
    ACTIVE --> SUSPENDED: suspender
    SUSPENDED --> ACTIVE: reactivar
    ACTIVE --> DEACTIVATED: desactivar
    SUSPENDED --> DEACTIVATED: desactivar
    PENDING_ACTIVATION --> DEACTIVATED: desactivar
    DEACTIVATED --> [*]
```

### Solicitud de registro

```mermaid
stateDiagram-v2
    [*] --> PENDING
    PENDING --> APPROVED: aprobar
    PENDING --> REJECTED: rechazar
    APPROVED --> [*]
    REJECTED --> [*]
```
