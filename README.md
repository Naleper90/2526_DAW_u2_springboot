# 📋 Aplicación CRUD de Usuarios con Spring Boot y Docker

Aplicación web educativa que demuestra cómo desarrollar y desplegar una aplicación Spring Boot utilizando Docker. Este proyecto está diseñado para el módulo de "Despliegue de Aplicaciones Web".

## 📚 Documentación Técnica
Puedes consultar el detalle completo del flujo CI/CD y el despliegue en la documentación oficial del proyecto:

👉 [Ver Documentación del Despliegue (CI/CD)](Documentacion.md)

Realizado por Natalia Alejo Pérez 2º DAW

## 📑 Tabla de Contenidos

- [Características](#características)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Requisitos Previos](#requisitos-previos)
- [Desarrollo de la Aplicación CRUD](#desarrollo-de-la-aplicación-crud)
- [Documentación de Dockerización y Despliegue](#documentación-de-dockerización-y-despliegue)
- [Uso de la Aplicación](#uso-de-la-aplicación)
- [API REST](#api-rest)
- [Estructura del Proyecto](#estructura-del-proyecto)

## ✨ Características

- ✅ CRUD completo de usuarios (Crear, Leer, Actualizar, Eliminar)
- ✅ Persistencia en archivo JSON (sin base de datos)
- ✅ API REST para integración con otros sistemas
- ✅ Interfaz web con Thymeleaf
- ✅ Diseño responsivo y moderno
- ✅ Dockerización con multi-stage build
- ✅ Docker Compose para orquestación
- ✅ Volúmenes para persistencia de datos

## 🛠️ Tecnologías Utilizadas

- **Java 17**: Lenguaje de programación
- **Spring Boot 3.5.6**: Framework principal
- **Gradle 8.x**: Herramienta de construcción
- **Thymeleaf**: Motor de plantillas para vistas
- **Jackson**: Serialización/deserialización JSON
- **Docker**: Contenedorización
- **Docker Compose**: Orquestación de contenedores
- **Tomcat 10.1**: Servidor de aplicaciones

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- Java 17 o superior
- Gradle 8.x+ (el proyecto incluye el wrapper)
- Docker Desktop o Docker Engine
- Docker Compose
- Git (opcional, para clonar el repositorio)

## 🚀 Desarrollo de la Aplicación CRUD

### 1. Arquitectura de la Aplicación

La aplicación sigue el patrón MVC (Model-View-Controller) y está organizada en capas:

```
├── model/          # Entidades de datos (User)
├── service/        # Lógica de negocio y persistencia
├── controller/     # Controladores REST y Web
├── templates/      # Vistas HTML con Thymeleaf
└── static/         # Recursos estáticos (CSS)
```

### 2. Modelo de Datos (User)

El modelo `User` representa un usuario con los siguientes campos:

```java
public class User {
    private Long id;        // Identificador único
    private String nombre;  // Nombre del usuario
    private String email;   // Correo electrónico
    private Integer edad;   // Edad
}
```

**Ubicación**: `src/main/java/com/example/springboot/model/User.java`

**Características**:
- Anotaciones Jackson para serialización JSON
- Constructor vacío para deserialización
- Getters y setters estándar
- Método toString() para debugging

### 3. Capa de Servicio (UserService)

El servicio `UserService` gestiona la persistencia de usuarios en un archivo JSON.

**Ubicación**: `src/main/java/com/example/springboot/service/UserService.java`

**Funcionalidades principales**:

```java
// Obtener todos los usuarios
List<User> getAllUsers()

// Obtener usuario por ID
Optional<User> getUserById(Long id)

// Crear nuevo usuario
User createUser(User user)

// Actualizar usuario existente
Optional<User> updateUser(Long id, User user)

// Eliminar usuario
boolean deleteUser(Long id)
```

**Características**:
- Anotación `@Service` para inyección de dependencias
- Persistencia en archivo JSON con Jackson
- Inicialización automática con `@PostConstruct`
- Generación automática de IDs con `AtomicLong`
- Manejo de errores con `Optional`

### 4. Controlador REST (UserRestController)

Proporciona endpoints API para operaciones CRUD.

**Ubicación**: `src/main/java/com/example/springboot/controller/UserRestController.java`

**Endpoints disponibles**:

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/users` | Obtener todos los usuarios |
| GET | `/api/users/{id}` | Obtener usuario por ID |
| POST | `/api/users` | Crear nuevo usuario |
| PUT | `/api/users/{id}` | Actualizar usuario |
| DELETE | `/api/users/{id}` | Eliminar usuario |

**Características**:
- Anotación `@RestController` para respuestas JSON
- Uso de `ResponseEntity` para códigos HTTP apropiados
- Validación con códigos 200, 201, 204, 404

### 5. Controlador Web (UserWebController)

Gestiona las vistas HTML para la interfaz de usuario.

**Ubicación**: `src/main/java/com/example/springboot/controller/UserWebController.java`

**Rutas disponibles**:

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/` | Página principal con lista de usuarios |
| GET | `/users/new` | Formulario para crear usuario |
| POST | `/users` | Procesar creación de usuario |
| GET | `/users/edit/{id}` | Formulario para editar usuario |
| POST | `/users/{id}` | Procesar actualización de usuario |
| GET | `/users/delete/{id}` | Eliminar usuario |

**Características**:
- Anotación `@Controller` para vistas HTML
- Uso de `Model` para pasar datos a las vistas
- `RedirectAttributes` para mensajes flash
- Validación de existencia de usuarios

### 6. Vistas con Thymeleaf

#### Vista Principal (index.html)

**Ubicación**: `src/main/resources/templates/index.html`

Muestra la lista de usuarios en una tabla con opciones para:
- Crear nuevo usuario
- Editar usuario existente
- Eliminar usuario
- Ver mensajes de notificación

#### Formulario de Usuario (user-form.html)

**Ubicación**: `src/main/resources/templates/user-form.html`

Formulario reutilizable para crear y editar usuarios con:
- Validación HTML5
- Campos para nombre, email y edad
- Botones de guardar y cancelar

### 7. Estilos CSS

**Ubicación**: `src/main/resources/static/css/style.css`

Diseño moderno y responsivo con:
- Gradientes y sombras
- Diseño adaptable para móviles
- Animaciones suaves
- Colores consistentes

### 8. Configuración de la Aplicación

**Ubicación**: `src/main/resources/application.properties`

```properties
# Puerto del servidor
server.port=8080

# Configuración de Thymeleaf
spring.thymeleaf.cache=false

# Archivo de datos
app.data.file=data/users.json
app.data.dir=data
```

### 9. Prueba Local de la Aplicación

#### Opción 1: Usando Gradle

```bash
# Compilar el proyecto
./gradlew build

# Ejecutar la aplicación
./gradlew bootRun
```

#### Opción 2: Usando el WAR generado

```bash
# Generar el archivo WAR
./gradlew bootWar

# El WAR se genera en: build/libs/2526_DAW_u2_springboot-0.0.1-SNAPSHOT.war
```

Una vez iniciada la aplicación, accede a:
- **Interfaz Web**: http://localhost:8080
- **API REST**: http://localhost:8080/api/users

## 🐳 Documentación de Dockerización y Despliegue

### 1. Introducción a Docker

Docker permite empaquetar una aplicación con todas sus dependencias en un contenedor estandarizado, garantizando que funcione de manera consistente en cualquier entorno.

**Conceptos clave**:
- **Imagen**: Plantilla inmutable que contiene la aplicación y sus dependencias
- **Contenedor**: Instancia en ejecución de una imagen
- **Dockerfile**: Archivo de texto con instrucciones para construir una imagen
- **Volumen**: Mecanismo para persistir datos fuera del contenedor

### 2. Estructura del Dockerfile

**Ubicación**: `Dockerfile` (raíz del proyecto)

Nuestro Dockerfile utiliza un **multi-stage build** para optimizar el tamaño de la imagen final:

#### Etapa 1: Construcción (Builder)

```dockerfile
FROM gradle:8.5-jdk17 AS builder
WORKDIR /app
COPY build.gradle settings.gradle gradlew ./
COPY gradle ./gradle
COPY src ./src
RUN ./gradlew bootWar --no-daemon
```

**Explicación**:
1. **FROM**: Usa una imagen base con Gradle y Java 17
2. **WORKDIR**: Establece el directorio de trabajo en `/app`
3. **COPY**: Copia archivos necesarios para la compilación
4. **RUN**: Ejecuta Gradle para generar el archivo WAR

**Ventajas**:
- Aprovecha el caché de Docker para dependencias
- Genera el WAR en un entorno controlado

#### Etapa 2: Imagen Final

```dockerfile
FROM tomcat:10.1-jdk17
ENV APP_DATA_DIR=/app/data
RUN mkdir -p ${APP_DATA_DIR}
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=builder /app/build/libs/*.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
VOLUME ["/app/data"]
CMD ["catalina.sh", "run"]
```

**Explicación**:
1. **FROM**: Imagen base ligera con Tomcat y Java 17
2. **ENV**: Define variables de entorno
3. **RUN mkdir**: Crea directorio para datos persistentes
4. **RUN rm**: Elimina aplicaciones por defecto de Tomcat
5. **COPY --from=builder**: Copia el WAR de la etapa anterior
6. **EXPOSE**: Documenta que el contenedor escucha en el puerto 8080
7. **VOLUME**: Define punto de montaje para datos persistentes
8. **CMD**: Comando para iniciar Tomcat

**Ventajas del multi-stage build**:
- ✅ Imagen final más pequeña (~200MB vs ~500MB)
- ✅ No incluye herramientas de compilación innecesarias
- ✅ Más segura (menos superficie de ataque)
- ✅ Proceso de build automatizado

### 3. Archivo .dockerignore

**Ubicación**: `.dockerignore` (raíz del proyecto)

Especifica qué archivos no deben copiarse a la imagen Docker:

```
.git/
build/
.gradle/
.idea/
*.md
data/
```

**Beneficios**:
- Reduce el tamaño del contexto de construcción
- Acelera el proceso de build
- Evita incluir archivos sensibles

### 4. Construcción de la Imagen Docker

#### Paso 1: Verificar los archivos

```bash
# Asegurarse de estar en el directorio del proyecto
ls -la

# Verificar que existen:
# - Dockerfile
# - build.gradle
# - src/
```

#### Paso 2: Construir la imagen

```bash
# Construir la imagen con un tag específico
docker build -t springboot-crud-app:1.0 .

# O simplemente con 'latest'
docker build -t springboot-crud-app .
```

**Explicación del comando**:
- `docker build`: Comando para construir imágenes
- `-t springboot-crud-app:1.0`: Tag (nombre:versión) de la imagen
- `.`: Contexto de construcción (directorio actual)

#### Paso 3: Verificar la imagen creada

```bash
# Listar imágenes
docker images

# Deberías ver algo como:
# REPOSITORY              TAG       IMAGE ID       SIZE
# springboot-crud-app     1.0       abc123def456   220MB
```

#### Paso 4: Inspeccionar la imagen

```bash
# Ver detalles de la imagen
docker inspect springboot-crud-app:1.0

# Ver capas de la imagen
docker history springboot-crud-app:1.0
```

### 5. Ejecución del Contenedor

#### Opción A: Ejecución simple

```bash
# Ejecutar el contenedor en primer plano
docker run -p 8080:8080 springboot-crud-app

# Ejecutar en segundo plano (modo detached)
docker run -d -p 8080:8080 --name my-crud-app springboot-crud-app
```

**Explicación de parámetros**:
- `-d`: Ejecuta en segundo plano (detached)
- `-p 8080:8080`: Mapea puerto host:contenedor
- `--name my-crud-app`: Nombre del contenedor

#### Opción B: Con volumen para persistencia

```bash
# Crear directorio local para datos
mkdir -p ./data

# Ejecutar con volumen montado
docker run -d \
  -p 8080:8080 \
  -v $(pwd)/data:/app/data \
  --name my-crud-app \
  springboot-crud-app
```

**Explicación**:
- `-v $(pwd)/data:/app/data`: Monta directorio local en el contenedor
- Los datos se guardarán en `./data/users.json` del host

#### Opción C: Con variables de entorno personalizadas

```bash
docker run -d \
  -p 8080:8080 \
  -e APP_DATA_FILE=/app/data/usuarios.json \
  -v $(pwd)/data:/app/data \
  --name my-crud-app \
  springboot-crud-app
```

### 6. Gestión del Contenedor

#### Ver contenedores en ejecución

```bash
# Listar contenedores activos
docker ps

# Listar todos los contenedores
docker ps -a
```

#### Ver logs del contenedor

```bash
# Ver logs en tiempo real
docker logs -f my-crud-app

# Ver últimas 100 líneas
docker logs --tail 100 my-crud-app
```

#### Detener y eliminar contenedor

```bash
# Detener el contenedor
docker stop my-crud-app

# Eliminar el contenedor
docker rm my-crud-app

# Detener y eliminar en un solo comando
docker rm -f my-crud-app
```

#### Acceder al contenedor

```bash
# Abrir shell en el contenedor
docker exec -it my-crud-app bash

# Ejecutar un comando
docker exec my-crud-app ls -la /app/data
```

### 7. Docker Compose

**Ubicación**: `docker-compose.yml` (raíz del proyecto)

Docker Compose permite definir y ejecutar aplicaciones multi-contenedor. Aunque nuestra aplicación es simple, esto facilita la configuración y es escalable.

#### Estructura del archivo

```yaml
version: '3.8'

services:
  springboot-crud-app:
    container_name: springboot-users-crud
    build:
      context: .
      dockerfile: Dockerfile
    image: springboot-crud-app:latest
    ports:
      - "8080:8080"
    environment:
      - APP_DATA_DIR=/app/data
      - APP_DATA_FILE=/app/data/users.json
    volumes:
      - ./data:/app/data
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/"]
      interval: 30s
      timeout: 10s
      retries: 3
```

**Componentes**:
- **services**: Define los contenedores
- **build**: Instrucciones para construir la imagen
- **ports**: Mapeo de puertos
- **environment**: Variables de entorno
- **volumes**: Persistencia de datos
- **restart**: Política de reinicio
- **healthcheck**: Verificación de salud de la aplicación

#### Comandos de Docker Compose

```bash
# Construir y levantar los servicios
docker-compose up -d

# Ver logs
docker-compose logs -f

# Detener servicios
docker-compose stop

# Detener y eliminar contenedores
docker-compose down

# Reconstruir imágenes
docker-compose build

# Reconstruir y levantar
docker-compose up -d --build
```

### 8. Verificación del Despliegue

Una vez que el contenedor esté en ejecución:

#### 1. Verificar que el contenedor está corriendo

```bash
docker ps | grep springboot
```

#### 2. Verificar los logs

```bash
docker logs springboot-users-crud
```

Deberías ver algo como:
```
Started Application in X.XXX seconds
```

#### 3. Probar la aplicación

```bash
# Probar la interfaz web
curl http://localhost:8080

# Probar la API REST
curl http://localhost:8080/api/users

# Crear un usuario de prueba
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{"nombre":"Juan Pérez","email":"juan@example.com","edad":30}'
```

#### 4. Acceder desde el navegador

- **Interfaz Web**: http://localhost:8080
- **API REST**: http://localhost:8080/api/users

### 9. Persistencia de Datos

Los datos se guardan en `data/users.json`:

```bash
# Ver el contenido del archivo de datos
cat data/users.json

# Ver datos dentro del contenedor
docker exec springboot-users-crud cat /app/data/users.json
```

**Ventaja**: Los datos persisten incluso si detienes o eliminas el contenedor, siempre que uses volúmenes.

### 10. Buenas Prácticas

#### ✅ Optimización de Imágenes

1. **Multi-stage build**: Reduce tamaño de imagen final
2. **Ordenar comandos**: Aprovecha caché de Docker
3. **Minimizar capas**: Combina comandos RUN cuando sea posible

#### ✅ Seguridad

1. **No incluir secretos**: Usa variables de entorno
2. **Usar imágenes oficiales**: Como base
3. **Mantener actualizadas**: Las imágenes base

#### ✅ Gestión de Datos

1. **Usar volúmenes**: Para datos persistentes
2. **Backups regulares**: Del directorio de datos
3. **No guardar datos en el contenedor**: Siempre usar volúmenes

### 11. Comandos de Referencia Rápida

```bash
# CONSTRUCCIÓN
docker build -t springboot-crud-app .

# EJECUCIÓN BÁSICA
docker run -d -p 8080:8080 --name crud-app springboot-crud-app

# EJECUCIÓN CON VOLUMEN
docker run -d -p 8080:8080 -v $(pwd)/data:/app/data --name crud-app springboot-crud-app

# DOCKER COMPOSE
docker-compose up -d           # Iniciar
docker-compose logs -f         # Ver logs
docker-compose down            # Detener

# GESTIÓN
docker ps                      # Ver contenedores
docker logs crud-app           # Ver logs
docker stop crud-app           # Detener
docker rm crud-app             # Eliminar

# LIMPIEZA
docker system prune -a         # Limpiar todo
```

## 📱 Uso de la Aplicación

### Interfaz Web

1. Accede a http://localhost:8080
2. Haz clic en "Crear Nuevo Usuario"
3. Completa el formulario con:
   - Nombre
   - Email
   - Edad
4. Haz clic en "Crear"
5. Verás el usuario en la lista
6. Puedes editarlo o eliminarlo usando los botones correspondientes

### API REST

#### Crear un usuario

```bash
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "María García",
    "email": "maria@example.com",
    "edad": 25
  }'
```

#### Obtener todos los usuarios

```bash
curl http://localhost:8080/api/users
```

#### Obtener un usuario específico

```bash
curl http://localhost:8080/api/users/1
```

#### Actualizar un usuario

```bash
curl -X PUT http://localhost:8080/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "María García Actualizada",
    "email": "maria.nueva@example.com",
    "edad": 26
  }'
```

#### Eliminar un usuario

```bash
curl -X DELETE http://localhost:8080/api/users/1
```

## 🔌 API REST

### Endpoints Disponibles

| Método | Endpoint | Descripción | Body |
|--------|----------|-------------|------|
| GET | `/api/users` | Listar todos los usuarios | - |
| GET | `/api/users/{id}` | Obtener un usuario | - |
| POST | `/api/users` | Crear usuario | JSON |
| PUT | `/api/users/{id}` | Actualizar usuario | JSON |
| DELETE | `/api/users/{id}` | Eliminar usuario | - |

### Formato JSON del Usuario

```json
{
  "id": 1,
  "nombre": "Juan Pérez",
  "email": "juan@example.com",
  "edad": 30
}
```

### Códigos de Respuesta HTTP

- `200 OK`: Operación exitosa
- `201 Created`: Usuario creado
- `204 No Content`: Usuario eliminado
- `404 Not Found`: Usuario no encontrado
- `500 Internal Server Error`: Error del servidor

## 📁 Estructura del Proyecto

```
.
├── src/
│   ├── main/
│   │   ├── java/com/example/springboot/
│   │   │   ├── Application.java              # Clase principal
│   │   │   ├── ServletInitializer.java       # Configuración WAR
│   │   │   ├── model/
│   │   │   │   └── User.java                 # Modelo de datos
│   │   │   ├── service/
│   │   │   │   └── UserService.java          # Lógica de negocio
│   │   │   └── controller/
│   │   │       ├── UserRestController.java   # API REST
│   │   │       └── UserWebController.java    # Controlador web
│   │   └── resources/
│   │       ├── application.properties        # Configuración
│   │       ├── static/css/
│   │       │   └── style.css                 # Estilos
│   │       └── templates/
│   │           ├── index.html                # Página principal
│   │           └── user-form.html            # Formulario
│   └── test/
│       └── java/com/example/springboot/
│           └── ApplicationTests.java         # Tests
├── gradle/                                   # Gradle wrapper
├── build.gradle                              # Configuración Gradle
├── settings.gradle                           # Settings Gradle
├── Dockerfile                                # Imagen Docker
├── docker-compose.yml                        # Orquestación
├── .dockerignore                             # Exclusiones Docker
├── .gitignore                                # Exclusiones Git
├── AGENTS.md                                 # Documentación del proyecto
└── README.md                                 # Este archivo
```

## 🎓 Objetivos Educativos Cumplidos

### Objetivo Principal: Dockerización ✅

- ✅ Creación de Dockerfile multi-stage
- ✅ Construcción de imágenes Docker
- ✅ Ejecución de contenedores
- ✅ Gestión de volúmenes para persistencia
- ✅ Uso de Docker Compose
- ✅ Buenas prácticas de Docker

### Objetivo Secundario: Aplicación CRUD Funcional ✅

- ✅ Modelo de datos completo
- ✅ Capa de servicio con persistencia en JSON
- ✅ API REST completa
- ✅ Interfaz web con Thymeleaf
- ✅ Diseño responsivo
- ✅ Validaciones y manejo de errores

## 📚 Recursos Adicionales

### Documentación Oficial

- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Docker Documentation](https://docs.docker.com/)
- [Thymeleaf Documentation](https://www.thymeleaf.org/documentation.html)
- [Gradle Documentation](https://docs.gradle.org/)

### Tutoriales Recomendados

- [Spring Boot Getting Started](https://spring.io/guides/gs/spring-boot/)
- [Docker for Java Developers](https://www.docker.com/blog/java-docker/)
- [Docker Compose Tutorial](https://docs.docker.com/compose/gettingstarted/)

## 🤝 Contribuciones

Este proyecto es educativo. Para sugerencias o mejoras, por favor abre un issue o pull request.

## 📄 Licencia

Este proyecto es de código abierto y está disponible para fines educativos.

## ✍️ Autor

Desarrollado para el módulo de **Despliegue de Aplicaciones Web**.

---

**¡Feliz aprendizaje! 🚀**
