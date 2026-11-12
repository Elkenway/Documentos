# Análisis del problema

## 1. Introducción

UCO Kudos es la plataforma con la que la Universidad Católica de Oriente registra reconocimientos entre integrantes de su comunidad. Un Kudo es un reconocimiento formal de una persona hacia otra, clasificado en una categoría institucional, sobre el que la comunidad conversa, califica y reporta, y que dos autoridades pueden moderar.

El sistema es, en el fondo, dos problemas unidos: una **red de reconocimientos** abierta a la participación y una **estructura institucional** rígida que decide quién puede hacer qué sobre quién.

## 2. Contexto del problema

Hoy los reconocimientos circulan por correo, redes sociales, conversaciones y eventos. Por eso no quedan rastro ni historial, no se pueden medir y no responden a una estrategia común. La Universidad quiere conservar esa memoria institucional sin perder el control sobre lo que se publica en su nombre.

De ahí salen las tres tensiones que guían el diseño:

| Tensión | Consecuencia en el diseño |
|---|---|
| Participación libre frente a gobernanza | Publicar no requiere aprobación previa; la moderación actúa después y solo la ejercen dos roles |
| Datos que cambian frente a memoria histórica | Nada se borra: los vínculos se cierran, los cargos acumulan periodos y los vetos ocultan sin eliminar |
| Identidad institucional frente a registro abierto | Cualquiera puede solicitar cuenta, pero solo entra con correo y celular verificados y con aprobación de su autoridad |

## 3. Objetivos

**General:** diseñar e implementar una solución completa para gestionar el reconocimiento institucional, aplicando diseño orientado a objetos con un modelo rico.

**Específicos:**

1. Reconocer públicamente a las personas y conservar el contenido completo.
2. Permitir conversaciones de profundidad ilimitada y calificación de 1 a 5 estrellas.
3. Respetar la estructura organizacional en cada aprobación y gestión.
4. Dar a Rector y Coordinación Académica un proceso de moderación con evidencia.
5. Hacer auditable toda acción relevante.

## 4. Actores

| Actor | Aprueba o gestiona a | Modera | Categorías | Estructura | Auditoría institucional |
|---|---|---|---|---|---|
| Administrador | Administradores y Rectores (los registra directamente) | No | Sí | Sí | Sí |
| Rector | Coordinadores Académicos | Sí | Sí | No | Sí |
| Coordinador Académico | Coordinadores de Facultad | Sí | Sí | Sí | Sí |
| Coordinador de Facultad | Coordinadores de Programa de su facultad | No | No | No | No |
| Coordinador de Programa | Docentes y estudiantes de su programa | No | No | No | No |
| Docente | Nadie | No | No | No | No |
| Estudiante | Nadie | No | No | No | No |

Todo actor con cuenta activa puede publicar Kudos, replicar, calificar, reportar y consultar su perfil y actividad.

## 5. Requisitos

Los 77 requisitos funcionales y los 35 no funcionales del enunciado se relacionan con el diseño, el código y las pruebas en [07_TRAZABILIDAD.md](07_TRAZABILIDAD.md). Agrupados:

| Área | Requisitos | Resumen |
|---|---|---|
| Usuarios y roles | RF-001 a RF-014 | Registro, unicidad, verificación, actualización y exclusividad de rol |
| Facultades y programas | RF-015 a RF-024 | Crear, consultar, modificar, responsable e historial |
| Estudiantes y docentes | RF-025 a RF-034 | Registro, vínculos múltiples y su historia |
| Aprobaciones | RF-035 a RF-040 | Cadena de aprobación y registro directo de autoridades |
| Categorías | RF-041 a RF-045 | Catálogo y activación |
| Kudos y réplicas | RF-046 a RF-056 | Publicación, historial, longitud libre, árbol ilimitado |
| Calificaciones | RF-057 a RF-060 | Escala y unicidad por persona y contenido |
| Reportes y moderación | RF-061 a RF-068 | Motivos, veto autorizado, historial |
| Auditoría y consultas | RF-069 a RF-077 | Registro, consulta y preguntas organizacionales |
| No funcionales | RNF-001 a RNF-035 | Seguridad, errores, mantenibilidad, auditabilidad, calidad, pruebas, documentación |

## Contradicciones y vacíos del enunciado

El catálogo es deliberadamente abierto y tiene puntos que exigen una decisión. Cada una queda aquí para poder defenderla.

| Id | Situación | Decisión | Razón |
|---|---|---|---|
| D-01 | RN-134 dice "solo los Administradores podrán crear categorías", pero RN-135 y RN-136 lo permiten a Rector y Coordinador Académico | Pueden crear los tres | RN-141 a RN-143 agrupan a esos mismos tres roles para el resto de operaciones; RN-137 a RN-140 niegan solo a los demás |
| D-02 | RN-193 y RN-194 dicen "solo el Rector" y "solo el Coordinador Académico" para el mismo veto | Vetan ambos | RN-243 e INV-033 lo dicen explícitamente |
| D-03 | Ninguna regla dice quién aprueba a los docentes (RN-090 solo nombra estudiantes) | El Coordinador de Programa | RN-113 le da la gestión de los docentes de su programa |
| D-04 | Una Facultad "puede existir sin Programas únicamente durante procesos de creación" (RN-045), sin definir cuándo termina ese proceso | No se impone un límite temporal | Toda facultad nace vacía; imponer un plazo inventaría una regla |
| D-05 | RN-130 y RN-131 piden gestionar parámetros globales y configuración técnica, sin indicar cuáles | Se hace por variables de entorno | La plataforma solo tiene parámetros técnicos; una pantalla de parámetros sería funcionalidad sin uso (YAGNI) |
| D-06 | No se dice quién crea facultades y programas (RF-015, RF-020) | Administrador y Coordinador Académico | Es estructura global; el Rector queda limitado por RN-125 a gestionar Coordinadores Académicos |
| D-07 | CU-001 habla de reconocer "a otra persona", sin regla numerada | Prohibido el autorreconocimiento | La definición de Kudo exige dos personas; lo refuerza `ck_kudo_recognizes_someone_else` |
| D-08 | RN-178 permite calificar "una vez", pero RN-237 exige un evento al modificar | Una calificación por persona y contenido, modificable | RN-180 habla de calificaciones "activas": cambiar el valor no crea una segunda |
| D-09 | Nada prohíbe calificar el contenido propio | Se permite | Añadir la restricción sería inventar una regla |
| D-10 | Los motivos de reporte incluyen "Otro" | Si el motivo es "Otro" el detalle es obligatorio | Sin detalle, el moderador no tendría información para decidir |
| D-11 | RN-229 exige un evento al consultar un Kudo, lo que puede inflar la auditoría | Se cumple: `KudoViewed` se registra | Es requisito explícito. En producción sería el primer candidato a retención limitada |
| D-12 | El enunciado no pide proveedor real de correo o SMS | Los códigos se escriben en el log detrás del puerto `VerificationCodeSender` | Integrar un proveedor es un cambio de un adaptador, sin tocar dominio |
| D-13 | "Veto" sin revisión previa: RN-204 dice "un Kudo puede ser vetado" sin condición | Se admite veto directo desde visible, reportado o en revisión | RN-204 y RN-209 no exigen reporte; `ModerationDecision.vetoDirectly` deja la misma evidencia |
