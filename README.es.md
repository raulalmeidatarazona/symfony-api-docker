# Base-docker-api-symfony
Este template proporciona un entorno Dockerizado listo para usar como punto de partida inicial para proyectos de Symfony. Contiene configuraciones predefinidas para los servicios de Nginx, PHP y PostgreSQL. La estructura del repositorio es la siguiente:
```
.
├── Makefile
├── README.md
├── docker
│   ├── nginx
│   │   ├── Dockerfile
│   │   └── config
│   │       ├── default.conf
│   │       └── nginx.conf
│   └── php[README.md](README.md)
│       ├── Dockerfile
│       └── config
│           ├── php_dev.ini
│           ├── php_prod.ini
│           ├── supervisor.conf
│           └── www.conf
├── docker-compose.override.yml
├── docker-compose.yml
```

## Makefile

El Makefile contiene comandos de utilidad para construir y operar el proyecto. Puedes utilizar `make [comando]` para ejecutar los comandos definidos en este archivo. Aquí hay una descripción de cada comando:
- `make init`: Inicializa el proyecto. Borrará cualquier contenedor existente, los construirá de nuevo y los iniciará. También instalará las dependencias de composer.
- `make start`: Inicia los contenedores.
- `make stop`: Detiene los contenedores.
- `make build`: Reconstruye todos los contenedores.
- `make restart`: Reinicia los contenedores.
- `make erase`: Borra todos los contenedores.
- `make composer-install`: Instala las dependencias del proyecto.
- `make bash`: Ejecuta una shell en el contenedor PHP.
- `make code-style`: Ejecuta php-cs para arreglar el estilo del código siguiendo las reglas de Symfony.

## Docker Compose

- `docker-compose.yml`: Este archivo define los servicios de nginx y php que se utilizarán en la producción. Se expone el puerto 80 para acceder al servidor nginx.

- `docker-compose.override.yml`: Este archivo añade un servicio de base de datos postgres para el desarrollo local y cambia la configuración de los servicios nginx y php para ajustarse al entorno de desarrollo.

## Docker

Este directorio contiene los Dockerfiles y la configuración de nginx y php.

### nginx

El Dockerfile de nginx se utiliza para crear una imagen que ejecuta nginx. La imagen se basa en `nginx:1.19-alpine`.

La configuración de nginx se divide en dos archivos:

- `nginx.conf`: Configuración global de nginx.
- `default.conf`: Configuración específica del servidor que se utiliza para la API.

### php

El Dockerfile de php se utiliza para crear tres imágenes:

- `base`: Instala las dependencias necesarias y configura el ambiente para ejecutar la API de Symfony.
- `dev`: Extiende la imagen `base` y agrega XDebug para la depuración.
- `prod`: Extiende la imagen `base`, optimiza para producción.

La configuración de PHP y PHP-FPM se divide en varios archivos:

- `php_dev.ini`: Configuración de PHP para el entorno de desarrollo.
- `php_prod.ini`: Configuración de PHP optimizada para producción.
- `www.conf`: Configuración del pool de PHP-FPM.
- `supervisor.conf`: Configuración para supervisord, que se utiliza para manejar el proceso de PHP-FPM.

## Cómo utilizar

Una vez que hayas clonado el repositorio y navegado al directorio del proyecto, sigue los pasos a continuación para comenzar un nuevo proyecto Symfony:

1. **Construir el entorno Docker**: Utiliza el comando `make init`. Este comando construirá y iniciará los contenedores Docker. También instalará las dependencias de Composer.

2. **Acceder al contenedor PHP**: Utiliza el comando `make bash`. Esto abrirá una terminal en el contenedor PHP, donde podrás ejecutar comandos de Symfony y Composer.

3. **Crear un nuevo proyecto Symfony**: Utiliza el comando `symfony new --dir=api --no-git --version=6.3`. Esto creará un nuevo proyecto Symfony 6.3 en el directorio `api`. La opción `--no-git` se utiliza porque el instalador de Symfony inicializaría un nuevo repositorio Git, y ya tenemos uno.

4. **Mover los archivos de Symfony**: Utiliza el comando `mv api/* . && mv api/.* .`. Esto moverá todos los archivos desde el directorio `api` a la raíz del proyecto. También se moverán los archivos ocultos (`.env`, `.gitignore`, etc).

5. **Remover el directorio `api`**: Utiliza el comando `rm -rf api`. Esto eliminará el directorio `api` que ya no se necesita.

6**Verificar la instalación**: Puedes verificar que Symfony se haya instalado correctamente accediendo a `http://localhost:8081` en tu navegador. Deberías ver la página de bienvenida de Symfony.