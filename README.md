# Taller Microservices - Entorno de Ejecución

Este proyecto consiste en una arquitectura de microservicios desarrollada con diferentes tecnologías y orquestada mediante Docker Compose.

## Arquitectura del Sistema

El sistema está compuesto por los siguientes microservicios:

- **taller-ms-front**: Frontend desarrollado en Next.js (Puerto 3000)
- **taller-ms-persistence**: API REST en Django (Puerto 3001)
- **taller-ms-processor**: Procesador de mensajes en Node.js/TypeScript
- **taller-ms-getter**: Servicio de obtención de datos en Python
- **taller-ms-notifier**: Servicio de notificaciones en Node.js
- **postgres**: Base de datos PostgreSQL (Puerto 5432)
- **redis**: Cache y mensajería (Puerto 6379)

## Requisitos Previos

- Docker
- Docker Compose
- Git

## Configuración del Entorno

### 1. Clonación de Repositorios

Primero, debes clonar los repositorios de los microservicios en el directorio base. La estructura debe ser la siguiente:

# Clonar los repositorios de los microservicios
git clone https://github.com/joseantoniopedraza/taller_ms_persitence.git
git clone https://github.com/joseantoniopedraza/taller_ms_notifier.git
git clone https://github.com/joseantoniopedraza/taller_ms_front.git
git clone https://github.com/joseantoniopedraza/taller_ms_processor.git
git clone https://github.com/joseantoniopedraza/taller_ms_getter.git
```

### 2. Obtención de Archivos de Entorno

Los archivos de configuración de entorno son necesarios para el correcto funcionamiento de los microservicios. **Debes solicitar estos archivos mediante correo electrónico** al equipo de desarrollo.

Debes completar los campos:

.ms_getter.env
API_KEY_MERCADO_PUBLICO

.ms_notifier.env
API_KEY_GMAIL

.ms_persistence.env
DB_PASSWORD

.ms_processor.env
GOOGLE_API_KEY

### 3. Estructura de Directorios

Una vez clonados los repositorios y obtenidos los archivos de entorno, tu estructura de directorios debe verse así:

```
taller/
├── docker-compose.yml
├── .ms_front.env
├── .ms_persistence.env
├── .ms_processor.env
├── .ms_getter.env
├── .ms_notifier.env
├── .postgres.env
├── taller_ms_front/
├── taller_ms_persitence/
├── taller_ms_processor/
├── taller_ms_getter/
└── taller_ms_notifier/
```

## Levantamiento del Entorno

### 1. Verificación de Archivos

Antes de ejecutar el entorno, asegúrate de que todos los archivos de entorno estén presentes:

```bash
ls -la *.env
```

### 2. Construcción y Ejecución

Para levantar todo el entorno de microservicios:

```bash
# Construir e iniciar todos los servicios
docker-compose up --build

# Para ejecutar en segundo plano
docker-compose up -d --build
```

### 3. Verificación del Estado

Para verificar que todos los servicios estén funcionando correctamente:

```bash
# Ver el estado de los contenedores
docker-compose ps

# Ver los logs de todos los servicios
docker-compose logs

# Ver logs de un servicio específico
docker-compose logs taller-ms-front
```

## Acceso a los Servicios

Una vez levantado el entorno, podrás acceder a:

- **Frontend**: http://localhost:3000
- **API de Persistencia**: http://localhost:3001
- **Base de datos PostgreSQL**: localhost:5432
- **Redis**: localhost:6379

## Configuración de CORS

El sistema está configurado para permitir peticiones cross-origin entre los microservicios:

### Frontend → Backend (Django)
- El frontend (puerto 3000) puede hacer peticiones al backend Django (puerto 3001)
- CORS está configurado en Django para aceptar peticiones desde:
  - `http://localhost:3000`
  - `http://127.0.0.1:3000`
- Se permiten todos los métodos HTTP comunes (GET, POST, PUT, DELETE, etc.)
- Se incluyen headers estándar como `Content-Type`, `Authorization`, etc.

### Configuración Técnica
- **Django**: Utiliza `django-cors-headers` para manejar CORS
- **Frontend**: Configurado para hacer peticiones a `http://localhost:3001`
- **Otros servicios**: Los microservicios processor y notifier no exponen endpoints HTTP, por lo que no requieren configuración CORS

## Comandos Útiles

```bash
# Detener todos los servicios
docker-compose down

# Detener y eliminar volúmenes (cuidado: elimina datos de la BD)
docker-compose down -v

# Reconstruir un servicio específico
docker-compose up --build taller-ms-front

# Ver logs en tiempo real
docker-compose logs -f

# Ejecutar comandos dentro de un contenedor
docker-compose exec taller-ms-persistence python manage.py migrate
```

## Solución de Problemas

### Error de Conexión a Base de Datos
Si el servicio de persistencia no puede conectarse a PostgreSQL:
```bash
# Verificar que PostgreSQL esté ejecutándose
docker-compose ps postgres

# Revisar logs de PostgreSQL
docker-compose logs postgres
```

### Error de Construcción
Si hay problemas durante la construcción:
```bash
# Limpiar imágenes y reconstruir
docker-compose down
docker system prune -f
docker-compose up --build
```

### Problemas de Red
Si hay problemas de conectividad entre servicios:
```bash
# Verificar la red de Docker
docker network ls
docker network inspect taller_red_ms
```

## Notas Importantes

- Los archivos de entorno contienen información sensible y no deben ser compartidos o subidos a repositorios públicos
- La primera ejecución puede tardar varios minutos debido a la descarga de imágenes y construcción de contenedores
- Los datos de PostgreSQL se almacenan en un volumen persistente
- Redis se ejecuta sin persistencia de datos (los datos se pierden al reiniciar)

## Soporte

Para obtener los archivos de entorno o reportar problemas, contacta al equipo de desarrollo mediante correo electrónico.
