# Manual de puesta a punto — UCO Kudos

Este manual permite ejecutar la solución desde cero en Windows, Linux o macOS.

## 1. Requisitos previos

| Herramienta | Versión | Verificación |
|---|---|---|
| JDK | 21 | `java -version` |
| Maven | 3.9 o superior | `mvn -version` |
| PostgreSQL | 14 o superior | `psql --version` |
| pgAdmin 4 | Cualquiera reciente | Opcional, para administrar la base |
| Node.js | 20 LTS o superior | `node -v` |
| npm | 10 o superior | `npm -v` |

## 2. Instalación

1. Instalar el JDK 21 (por ejemplo Eclipse Temurin) y definir `JAVA_HOME`.
2. Instalar Maven y agregar su carpeta `bin` al `PATH`.
3. Instalar PostgreSQL. Durante la instalación se define la contraseña del usuario `postgres`; guardarla.
4. Instalar pgAdmin 4 si se desea una interfaz gráfica.
5. Instalar Node.js (incluye npm).

## 3. Configuración de la base de datos

Con pgAdmin 4: clic derecho en *Databases* → *Create* → *Database*, nombre `uco_kudos`.

Con consola:

```bash
psql -U postgres -c "CREATE DATABASE uco_kudos;"
```

No hay que crear tablas: Flyway aplica las migraciones de `uco-kudos-backend/src/main/resources/db/migration` al arrancar.

## 4. Variables de entorno del Back-End

| Variable | Obligatoria | Ejemplo |
|---|---|---|
| `KUDOS_DB_URL` | No | `jdbc:postgresql://localhost:5432/uco_kudos` |
| `KUDOS_DB_USER` | No | `postgres` |
| `KUDOS_DB_PASSWORD` | Sí | Contraseña de PostgreSQL |
| `KUDOS_JWT_SECRET` | Sí | Texto aleatorio de al menos 32 caracteres |
| `KUDOS_ADMIN_EMAIL` | Primer arranque | `admin@uco.edu.co` |
| `KUDOS_ADMIN_PHONE` | Primer arranque | `3001234567` |
| `KUDOS_ADMIN_PASSWORD` | Primer arranque | Clave del Administrador inicial |
| `KUDOS_ADMIN_NAME` | No | `Administración UCO Kudos` |
| `KUDOS_FRONTEND_ORIGIN` | No | `http://localhost:5173` |
| `KUDOS_PORT` | No | `8080` |

En PowerShell:

```powershell
$env:KUDOS_DB_PASSWORD = "clave-postgres"
$env:KUDOS_JWT_SECRET = "una-clave-larga-y-aleatoria-de-32-caracteres-o-mas"
$env:KUDOS_ADMIN_EMAIL = "admin@uco.edu.co"
$env:KUDOS_ADMIN_PHONE = "3001234567"
$env:KUDOS_ADMIN_PASSWORD = "ClaveSegura2026"
```

En Bash se usa `export VARIABLE=valor`.

## 5. Ejecución

Back-End (desde `uco-kudos-backend`):

```bash
mvn spring-boot:run
```

Con datos de demostración (cuentas con contraseña `KudosDemo2026`, listadas en el README del Front-End):

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=demo
```

Front-End (desde `uco-kudos-frontend`, en otra terminal):

```bash
npm install
npm run dev
```

Abrir `http://localhost:5173`. Los códigos de verificación de correo y celular de un registro nuevo aparecen en el log del Back-End.

Si el equipo no tiene Maven en el `PATH`, hay dos alternativas con JDK 21:

- Desde Eclipse: clic derecho sobre el proyecto, *Run As*, *Maven build...*, y en *Goals* escribir `verify` o `spring-boot:run`. Para el perfil de demostración, agregar en *Parameters* `spring-boot.run.profiles` con el valor `demo`.
- Con el JAR ya construido en `target`: `java -jar target/uco-kudos-backend-1.0.0.jar --spring.profiles.active=demo`, usando el `java` del JDK 21 y las variables de entorno de la sección 4.

## 6. Pruebas

Back-End: pruebas unitarias e de integración (H2, sin PostgreSQL), ArchUnit, cobertura JaCoCo (mínimo 85 %) y PMD:

```bash
mvn verify
```

Informe de cobertura: `uco-kudos-backend/target/site/jacoco/index.html`.

Front-End:

```bash
npm run lint
npm run typecheck
npm run coverage
```

Informe de cobertura: `uco-kudos-frontend/coverage/index.html`.

## 7. Despliegue

1. Back-End: `mvn package` genera `target/uco-kudos-backend-*.jar`. Ejecutar en el servidor con `java -jar` y las variables de entorno de la sección 4 apuntando a la base de producción. Cambiar `KUDOS_JWT_SECRET` por un valor propio y definir `KUDOS_FRONTEND_ORIGIN` con el dominio público del Front-End.
2. Front-End: `npm run build` genera `dist/`. Publicarlo en cualquier servidor de archivos estáticos (Nginx, Apache) configurando:
   - Redirección de `/api` al Back-End (proxy inverso).
   - Respuesta con `index.html` para cualquier ruta desconocida, porque la navegación es del lado del cliente.
3. Base de datos: basta con crear la base vacía; las migraciones se aplican solas en el primer arranque.
