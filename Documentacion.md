# 🚀 Documentación del Flujo CI/CD y Despliegue

Este documento detalla la implementación y características del flujo de Integración Continua/Despliegue Continuo (CI/CD) automatizado para la aplicación Spring Boot, utilizando GitHub Actions y Docker Hub.

## 1. Implementación del Flujo de CI/CD (GitHub Actions)

El flujo de trabajo automatizado se activa con cada git push a la rama principal (master) y ejecuta el job `build-and-publish-docker`.

### 1.1. Tareas Automatizadas y Justificación

| Tarea Requerida | Estado | Acción Realizada y Justificación |
|----------------|--------|----------------------------------|
| Generar una imagen | ✅ Hecho | La acción `docker/build-push-action` ejecuta el Dockerfile de múltiples etapas. La compilación de Gradle se realiza dentro del runner de Docker, asegurando que el entorno de build sea siempre limpio. |
| Cambio de Título | ✅ Hecho | El comando `sed` modifica el archivo `src/main/resources/templates/index.html` antes de la construcción de Docker. Esto permite insertar el hash del commit (`${{ github.sha }}`) para trazabilidad y verificación de la versión en producción. |
| Publicar en Docker Hub | ✅ Hecho | El `docker/login-action` autentica de forma segura el runner con el PAT. La imagen se publica en el repositorio `esanatyoe/proyecto1`. |
| Seguridad de Token | ✅ Hecho | El error de `insufficient scopes` fue resuelto generando un nuevo Token de Acceso Personal (PAT) con permisos de Write (Escritura) para la publicación de la imagen. |
| Documentación | ✅ Hecho | El último paso del workflow imprime el resumen de la publicación, el hash del commit y el comando de ejecución final. |

### 1.2. Gestión de Credenciales

La autenticación para la publicación en Docker Hub se realiza a través de Secrets de Repositorio en GitHub:

| Secret | Valor Contenido | Función |
|--------|----------------|---------|
| SesioDocker | Nombre de Usuario (esanatyoe) | Identificación del repositorio de destino. |
| SesionToken | Token de Acceso Personal (PAT) | Actúa como contraseña con permisos de escritura para autorizar el `docker push`. |

## 2. Descripción de la Imagen Docker

La imagen empaqueta la aplicación Spring Boot para la Gestión de Usuarios, utilizando un enfoque optimizado para el despliegue en contenedores.

### 2.1. Arquitectura de la Imagen (Multi-stage Build)

Se utiliza un patrón de construcción de Múltiples Etapas (Multi-stage build) para optimizar el tamaño de la imagen y la seguridad:

- **Etapa builder** (`gradle:8.5-jdk17`): Contiene el SDK completo y Gradle. Aquí se compila el código y se genera el archivo `.war`. Esta etapa se descarta al final.
- **Etapa Final** (`tomcat:10.1-jdk17`): Contiene solo el JRE y Tomcat. Únicamente se copia el archivo `.war` generado desde la primera etapa. Esto resulta en una imagen final mucho más pequeña, ya que no incluye herramientas de desarrollo ni compiladores.

### 2.2. Características y Trazabilidad

| Característica | Detalle | Significado para el Despliegue |
|---------------|---------|-------------------------------|
| Imagen Publicada | `esanatyoe/proyecto1` | Repositorio final en Docker Hub. |
| Etiquetado | `latest` y `sha_corto` | Permite un despliegue rápido (`latest`) y garantiza la inmutabilidad y trazabilidad de cada versión mediante el hash de commit. |
| Puerto Expuesto | 8080 | Puerto estándar de Tomcat/Spring Boot, listo para el mapeo de puertos (`-p 8080:8080`). |
| Trazabilidad | Hash de Commit en Título | La página web muestra el hash del commit, permitiendo a los testers o el equipo de Operaciones verificar exactamente qué versión del código se está ejecutando. |

## 3. Despliegue y Enlace Final

La imagen está lista para ser desplegada en cualquier entorno compatible con Docker.

🔗 **Enlace al Repositorio de Imágenes:**  
[https://hub.docker.com/repositories/esanatyoe](https://hub.docker.com/repositories/esanatyoe)
