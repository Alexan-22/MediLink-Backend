MediLink-Backend

**Sistema de Gestión de Citas Médicas basado en Arquitectura de Microservicios**

MediLink-Backend es una aplicación desarrollada con Java, Spring Boot y Spring Cloud que permite gestionar usuarios, pacientes, médicos, citas médicas, notificaciones y reportes.

El sistema utiliza PostgreSQL como gestor de bases de datos, Eureka Server para el descubrimiento de servicios y API Gateway como punto de entrada para las solicitudes HTTP.

## 1. Tecnologías utilizadas

- Java 21
- Spring Boot
- Spring Cloud
- Spring Cloud Netflix Eureka
- Spring Cloud Gateway
- OpenFeign
- Spring Data JPA
- PostgreSQL
- Maven
- JUnit 5
- IntelliJ IDEA
- pgAdmin 4
- Postman
- Git y GitHub

## 2. Arquitectura del proyecto

El backend está organizado en ocho aplicaciones independientes.

| Microservicio | Puerto | Descripción |
|---|---|---|
| Eureka Server | 8761 | Registro y descubrimiento de microservicios |
| API Gateway | 8080 | Punto de entrada de las solicitudes |
| Auth Service | 8081 | Gestión de usuarios y autenticación |
| Patient Service | 8082 | Gestión de pacientes |
| Doctor Service | 8083 | Gestión de médicos |
| Appointment Service | 8084 | Gestión de citas médicas |
| Notification Service | 8085 | Gestión de notificaciones |
| Report Service | 8086 | Generación de reportes |

Cada microservicio se ejecuta de manera independiente y se registra en Eureka Server.

## 3. Requisitos previos

Antes de ejecutar el proyecto, instalar:

1. **Java JDK 21**.
2. **IntelliJ IDEA**.
3. **PostgreSQL**.
4. **pgAdmin 4**.
5. **Git**.
6. **Postman** (opcional, para probar los endpoints).

Verificar Java desde PowerShell:

```powershell
java -version
```

Debe indicar Java 21.

Verificar Git:

```powershell
git --version
```

**Nota:** El proyecto incluye Maven Wrapper (`mvnw.cmd`), por lo que no es necesario instalar Maven manualmente.

## 4. Descargar el proyecto desde GitHub

Abrir PowerShell, seleccionar una carpeta de trabajo y ejecutar:

```powershell
cd D:\
mkdir ProyectosJava
cd ProyectosJava
```

Clonar el repositorio:

```powershell
git clone https://github.com/Alexan-22/MediLink-Backend.git
```

**Importante:** El repositorio es privado. Cada compañero debe tener autorización de acceso en GitHub y autenticarse con su propia cuenta.

Ingresar al proyecto:

```powershell
cd MediLink-Backend
```

También se puede descargar desde GitHub mediante **Code → Download ZIP**, siempre que se tenga acceso al repositorio.

## 5. Abrir el proyecto en IntelliJ IDEA

1. Abrir IntelliJ IDEA.
2. Seleccionar **File → Open**.
3. Buscar la carpeta `MediLink-Backend`.
4. Abrir el proyecto.
5. Esperar a que IntelliJ cargue los archivos y dependencias.
6. Verificar que el SDK configurado sea **Java 21**.

Si IntelliJ solicita importar proyectos Maven, aceptar la importación.

La estructura del proyecto es:

```text
MediLink-Backend/
├── api-gateway/
├── appointment-service/
├── auth-service/
├── doctor-service/
├── eureka-server/
├── notification-service/
├── patient-service/
├── report-service/
├── .gitignore
└── README.md
```

## 6. Configurar PostgreSQL y pgAdmin 4

Cada compañero debe utilizar su propia instalación de PostgreSQL y su propia contraseña.

### Paso 1. Abrir pgAdmin 4

1. Abrir **pgAdmin 4**.
2. En el panel izquierdo, buscar **Servers**.
3. Expandir el servidor PostgreSQL.
4. Introducir la contraseña configurada durante la instalación.
5. Verificar que el servidor esté conectado.

Generalmente, PostgreSQL se ejecuta en:

```text
Host: localhost
Puerto: 5432
Usuario: postgres
```

La contraseña será la que cada compañero haya configurado en su computadora.

### Paso 2. Crear las bases de datos

Debemos crear cinco bases de datos independientes:

| Microservicio | Base de datos |
|---|---|
| Auth Service | auth_db |
| Patient Service | patient_db |
| Doctor Service | doctor_db |
| Appointment Service | appointment_db |
| Notification Service | notification_db |

Los otros tres componentes (Eureka Server, API Gateway y Report Service) no requieren una base de datos propia en esta configuración.

### Paso 3. Crear `auth_db`

1. En pgAdmin 4, expandir **Servers → PostgreSQL**.
2. Hacer clic derecho sobre **Databases**.
3. Seleccionar **Create → Database**.
4. En **Database**, escribir `auth_db`.
5. En **Owner**, seleccionar `postgres`.
6. Presionar **Save**.

### Paso 4. Crear `patient_db`

Repetir el procedimiento:

1. Clic derecho en **Databases**.
2. Seleccionar **Create → Database**.
3. Escribir `patient_db`.
4. Seleccionar `postgres` como propietario.
5. Presionar **Save**.

### Paso 5. Crear `doctor_db`

1. Clic derecho en **Databases**.
2. Seleccionar **Create → Database**.
3. Escribir `doctor_db`.
4. Seleccionar `postgres`.
5. Presionar **Save**.

### Paso 6. Crear `appointment_db`

1. Clic derecho en **Databases**.
2. Seleccionar **Create → Database**.
3. Escribir `appointment_db`.
4. Seleccionar `postgres`.
5. Presionar **Save**.

### Paso 7. Crear `notification_db`

1. Clic derecho en **Databases**.
2. Seleccionar **Create → Database**.
3. Escribir `notification_db`.
4. Seleccionar `postgres`.
5. Presionar **Save**.

Al terminar, pgAdmin 4 debe mostrar:

```text
Databases
├── appointment_db
├── auth_db
├── doctor_db
├── notification_db
├── patient_db
└── postgres
```

**Importante:** No es necesario crear las tablas manualmente si la configuración JPA del proyecto permite generarlas automáticamente.

## 7. Configurar las contraseñas de PostgreSQL

Por seguridad, las contraseñas reales no están incluidas en GitHub.

Los cinco microservicios que utilizan PostgreSQL tienen una configuración pública similar a esta:

```yaml
spring:
  profiles:
    active: ${SPRING_PROFILES_ACTIVE:local}

  datasource:
    url: jdbc:postgresql://localhost:5432/patient_db
    username: postgres
    password: ${DB_PASSWORD:}
```

Cada compañero debe configurar su propia contraseña.

### Paso 1. Localizar los archivos de configuración

Buscar estos cinco archivos:

```text
auth-service/src/main/resources/application.yml
patient-service/src/main/resources/application.yml
doctor-service/src/main/resources/application.yml
appointment-service/src/main/resources/application.yml
notification-service/src/main/resources/application.yml
```

### Paso 2. Crear los archivos privados

En la carpeta `src/main/resources` de cada uno de esos cinco microservicios, crear un archivo llamado:

```text
application-local.yml
```

### Paso 3. Configurar cada archivo privado

**Auth Service**

Archivo:

`auth-service/src/main/resources/application-local.yml`

```yaml
spring:
  datasource:
    password: TU_PASSWORD_POSTGRESQL
```

**Patient Service**

Archivo:

`patient-service/src/main/resources/application-local.yml`

```yaml
spring:
  datasource:
    password: TU_PASSWORD_POSTGRESQL
```

**Doctor Service**

Archivo:

`doctor-service/src/main/resources/application-local.yml`

```yaml
spring:
  datasource:
    password: TU_PASSWORD_POSTGRESQL
```

**Appointment Service**

Archivo:

`appointment-service/src/main/resources/application-local.yml`

```yaml
spring:
  datasource:
    password: TU_PASSWORD_POSTGRESQL
```

**Notification Service**

Archivo:

`notification-service/src/main/resources/application-local.yml`

```yaml
spring:
  datasource:
    password: TU_PASSWORD_POSTGRESQL
```

Reemplazar `TU_PASSWORD_POSTGRESQL` por la contraseña real del usuario `postgres` en su computadora. Si la contraseña contiene caracteres especiales, utilizar comillas simples de YAML cuando corresponda.

No agregar `spring.profiles.active` dentro de `application-local.yml`.

**Seguridad:** Los archivos `application-local.yml` están excluidos de Git mediante `.gitignore`. No deben subirse a GitHub ni compartirse con otros compañeros.

### Paso 4. Verificar las conexiones

Comprobar que PostgreSQL esté ejecutándose y que las cinco bases de datos existan.

Si PostgreSQL utiliza otro puerto o usuario, modificar la configuración pública de conexión según corresponda, sin publicar credenciales privadas.

## 8. Ejecutar los microservicios paso a paso

Cada microservicio debe ejecutarse en su propia terminal de IntelliJ IDEA.

Para abrir una nueva terminal, utilizar la pestaña **Terminal** y el botón **+**.

Se recomienda seguir el orden indicado.

### 8.1. Iniciar Eureka Server

**Terminal 1**

```powershell
cd eureka-server
.\mvnw.cmd spring-boot:run
```

Esperar a que el servidor termine de iniciar.

Abrir en el navegador:

http://localhost:8761

Debe aparecer el panel de Eureka Server.

**No cerrar esta terminal.**

### 8.2. Iniciar Auth Service

**Terminal 2**, desde la raíz del proyecto:

```powershell
cd auth-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8081**.

Este servicio se conecta a:

```text
auth_db
```

Verificar que no existan errores de conexión a PostgreSQL.

### 8.3. Iniciar Patient Service

**Terminal 3**, desde la raíz del proyecto:

```powershell
cd patient-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8082**.

Base de datos:

```text
patient_db
```

### 8.4. Iniciar Doctor Service

**Terminal 4**, desde la raíz del proyecto:

```powershell
cd doctor-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8083**.

Base de datos:

```text
doctor_db
```

### 8.5. Iniciar Appointment Service

**Terminal 5**, desde la raíz del proyecto:

```powershell
cd appointment-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8084**.

Base de datos:

```text
appointment_db
```

### 8.6. Iniciar Notification Service

**Terminal 6**, desde la raíz del proyecto:

```powershell
cd notification-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8085**.

Base de datos:

```text
notification_db
```

### 8.7. Iniciar Report Service

**Terminal 7**, desde la raíz del proyecto:

```powershell
cd report-service
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8086**.

Este microservicio consulta información de otros servicios.

### 8.8. Iniciar API Gateway

**Terminal 8**, desde la raíz del proyecto:

```powershell
cd api-gateway
.\mvnw.cmd spring-boot:run
```

Puerto esperado: **8080**.

API Gateway permite acceder a los microservicios mediante un único punto de entrada.

**Nota:** Cada comando anterior supone que se abre una terminal nueva situada en la raíz `MediLink-Backend`. Si una terminal ya está dentro de otro microservicio, regresar primero a la carpeta raíz.

## 9. Verificar que todos los microservicios estén activos

Abrir:

http://localhost:8761

En Eureka Server, revisar la sección **Instances currently registered with Eureka**.

Deben aparecer registrados los siguientes servicios:

```text
AUTH-SERVICE
PATIENT-SERVICE
DOCTOR-SERVICE
APPOINTMENT-SERVICE
NOTIFICATION-SERVICE
REPORT-SERVICE
API-GATEWAY
```

El estado esperado es:

```text
UP
```

Eureka Server no aparece como cliente registrado en esa lista porque actúa como servidor de descubrimiento.

Si algún microservicio no aparece, revisar su terminal y comprobar que haya iniciado correctamente.

## 10. Probar el backend con Postman

Una vez iniciados todos los microservicios, abrir Postman.

La dirección principal es:

```text
http://localhost:8080
```

API Gateway redirige las solicitudes a los servicios correspondientes.

### Rutas principales

| Módulo | Ruta |
|---|---|
| Autenticación | `/auth/**` |
| Pacientes | `/patients/**` |
| Médicos | `/doctors/**` |
| Citas | `/appointments/**` |
| Notificaciones | `/notifications/**` |
| Reportes | `/reports/**` |

### Ejemplo: registrar un paciente

Método:

```http
POST http://localhost:8080/patients
```

En Postman:

1. Seleccionar **POST**.
2. Introducir la URL.
3. Abrir **Body → raw → JSON**.
4. Ingresar los datos.

Ejemplo:

```json
{
  "dni": "12345678",
  "nombres": "Juan Carlos",
  "apellidos": "Perez Lopez",
  "fechaNacimiento": "1998-05-15",
  "sexo": "M",
  "telefono": "987654321",
  "email": "juan@example.com",
  "direccion": "Lima",
  "tipoSangre": "O+",
  "alergias": "Ninguna"
}
```

Presionar **Send**.

Si el registro es correcto, el backend devolverá los datos del paciente registrado.

### Ejemplo: listar pacientes

Método:

```http
GET http://localhost:8080/patients
```

Presionar **Send**.

La respuesta mostrará los pacientes almacenados en `patient_db`.

## 11. Reportes disponibles

El módulo de reportes dispone de las siguientes rutas:

```http
GET http://localhost:8080/reports/general
```

```http
GET http://localhost:8080/reports/citas-estado
```

```http
GET http://localhost:8080/reports/activos
```

Estas rutas permiten consultar información consolidada del sistema.

## 12. Problemas frecuentes y soluciones

### Error: contraseña incorrecta

Mensaje habitual:

```text
FATAL: password authentication failed for user "postgres"
```

**Solución:**

1. Abrir pgAdmin 4.
2. Comprobar la contraseña del usuario `postgres`.
3. Revisar `application-local.yml` del microservicio.
4. Guardar los cambios.
5. Reiniciar el microservicio.

### Error: la base de datos no existe

Ejemplo:

```text
FATAL: database "patient_db" does not exist
```

**Solución:**

Crear la base de datos correspondiente desde pgAdmin 4, respetando exactamente su nombre.

### Error: puerto ocupado

Ejemplo:

```text
Port 8082 was already in use
```

**Solución:**

Verificar que el microservicio no esté ejecutándose en otra terminal.

En PowerShell:

```powershell
netstat -ano | findstr :8082
```

Identificar el proceso antes de detenerlo.

### Error: Java no reconocido

Ejemplo:

```text
java is not recognized
```

**Solución:**

Instalar Java 21 y configurar correctamente `JAVA_HOME` y `PATH`.

### Error: servicio no registrado en Eureka

**Solución:**

1. Verificar que Eureka Server esté iniciado.
2. Revisar http://localhost:8761.
3. Comprobar la configuración de Eureka del microservicio.
4. Reiniciar el microservicio.

### Error: no se puede conectar a PostgreSQL

**Solución:**

1. Verificar que PostgreSQL esté iniciado.
2. Comprobar que utiliza el puerto `5432`.
3. Revisar el nombre de la base de datos.
4. Verificar el usuario y la contraseña.
5. Confirmar que PostgreSQL acepte conexiones locales.

## 13. Detener los microservicios

Para detener un microservicio, ingresar a su terminal y presionar:

```text
Ctrl + C
```

Repetir el procedimiento en las ocho terminales.

## 14. Seguridad y recomendaciones

- No subir contraseñas reales a GitHub.
- No compartir archivos `application-local.yml`.
- Mantener los archivos privados excluidos mediante `.gitignore`.
- Utilizar una contraseña propia de PostgreSQL.
- No modificar los puertos de los microservicios sin actualizar las configuraciones correspondientes.
- Iniciar Eureka Server antes de los demás servicios.
- Verificar que cada microservicio se registre correctamente.
- No subir datos médicos reales o información personal de pacientes para realizar pruebas.

## 15. Repositorio

**Proyecto:** MediLink-Backend

**Repositorio:** https://github.com/Alexan-22/MediLink-Backend

**Arquitectura:** Microservicios con Spring Boot, Spring Cloud y PostgreSQL.
