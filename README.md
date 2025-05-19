# Laboratorio 1.2: Uso de Contenedores con Docker

Este laboratorio introduce el uso de contenedores con Docker y la orquestación básica con Docker Compose.

## 1. Descarga de Imágenes desde Docker Hub

Esta sección muestra cómo descargar imágenes de contenedores desde Docker Hub, el registro público de imágenes de Docker.

```bash
docker pull nginx
```

> Descarga la última versión de la imagen oficial de Nginx.

```bash
docker pull nginx:alpine
```

> Descarga una versión específica de la imagen de Nginx, en este caso, la variante "alpine", que es una imagen más ligera basada en Alpine Linux. El `:alpine` se conoce como una etiqueta (tag).

## 2. Gestión y Uso de Contenedores desde la Terminal

Esta sección detalla los comandos básicos para la gestión y el uso de contenedores Docker directamente desde la línea de comandos.

### 2.1 Búsqueda de Imágenes

```bash
docker search nginx
```

> Busca imágenes relacionadas con "nginx" en Docker Hub. La salida mostrará una lista de imágenes, su descripción, el número de estrellas (popularidad) y si son imágenes oficiales (`[OK]`).

### 2.2 Creación y ejecución de Contenedores

El comando `docker create` crea un contenedor pero no lo inicia.

```bash
docker create nginx
```

> Crea un nuevo contenedor basado en la imagen de Nginx sin ninguna configuración adicional. El ID del contenedor será generado automáticamente.

```bash
docker create -p 8080:80 nginx
```

> Crea un contenedor basado en la imagen de Nginx y realiza un mapeo de puertos. La opción `-p` (publish) mapea el puerto 80 dentro del contenedor al puerto 8080 en la máquina host. Esto permitirá acceder al servicio dentro del contenedor a través de `localhost:8080` o la IP del host en el puerto 8080.

```bash
docker create --name mi_nginx nginx
```

> Crea un contenedor basado en la imagen de Nginx y le asigna el nombre `mi_nginx`. Utilizar nombres facilita la gestión de los contenedores.

```bash
docker create -p 8080:80 --name mi_nginx_con_puerto nginx
```

> Crea un contenedor basado en la imagen de Nginx, mapea el puerto 80 del contenedor al puerto 8080 del host y le asigna el nombre `mi_nginx_con_puerto`.

### 2.3 Ejecución, Detención y Eliminación de Contenedores

```bash
docker run mi_nginx_con_puerto
```

> **¡Importante\!** Si un contenedor ya fue creado con `docker create`, este comando intentará ejecutarlo. Sin embargo, la forma más común de crear y ejecutar un contenedor en un solo paso es usando `docker run` directamente con la imagen.

```bash
docker stop mi_nginx_con_puerto
```

> Detiene el contenedor en ejecución llamado `mi_nginx_con_puerto`. El contenedor dejará de consumir recursos.

```bash
docker rm mi_nginx_con_puerto
```

> Elimina el contenedor llamado `mi_nginx_con_puerto`. Un contenedor debe estar detenido para poder ser eliminado. Esta acción elimina el contenedor y su configuración.

### 2.4 Listado de Contenedores

```bash
docker ps
```

> Muestra una lista de todos los contenedores que están actualmente en ejecución.

```bash
docker ps -a
```

> Muestra una lista de todos los contenedores, incluyendo aquellos que están en ejecución (`Up`) y los que están detenidos (`Exited`).

### 2.5 Visualización de Logs

```bash
docker logs mi_nginx_con_puerto
```

> Muestra los logs (salida estándar y error estándar) del contenedor llamado `mi_nginx_con_puerto`. Esto es útil para verificar el funcionamiento de la aplicación dentro del contenedor o para depurar problemas.

```bash
docker logs --follow mi_nginx_con_puerto
```

> Muestra los logs del contenedor `mi_nginx_con_puerto` y sigue la salida en tiempo real. Esto es similar a usar `tail -f` en un archivo de log.

### 2.6 Comando `docker run` Todo en Uno

El comando `docker run` puede realizar múltiples acciones en un solo paso:

1.  **Verifica si la imagen existe localmente y, si no, la descarga.**
2.  **Crea un nuevo contenedor a partir de la imagen.**
3.  **Ejecuta el contenedor.**

El siguiente ejemplo muestra cómo hacer esto en segundo plano (`-d`), estableciendo un nombre (`--name`), mapeando un puerto (`-p`) y montando un volumen (`-v`):

```bash
docker run -d -p 8080:80 --name mi_nginx_con_puerto nginx:alpine
```

> Ejecuta un contenedor en segundo plano (`-d`) basado en la imagen `nginx:alpine`. Mapea el puerto 80 del contenedor al puerto 8080 del host (`-p 8080:80`). Le asigna el nombre `mi_nginx_con_puerto` (`--name mi_nginx_con_puerto`). Además, monta un volumen (`-v`). En este caso, el archivo `index.html` ubicado en el directorio actual de la máquina host (`$(pwd)/index.html`) se monta en el directorio `/usr/share/nginx/html/index.html` dentro del contenedor (donde Nginx sirve sus archivos web).

```bash
docker run -d -p 8080:80 --name mi_nginx_con_puerto_temporal --rm nginx:alpine
```

> Similar al anterior pero al momento de que se detenga el contenedor se eliminará automáticamente.

## 3. Ejemplos de Dockerfile

### 3.1 Crear un balanceador con contenedores

```bash
docker run -d -p 8081:80 --rm --name ejemplo1_1 -v $(pwd)/index.1.html:/usr/share/nginx/html/index.html nginx:alpine
```

```bash
docker run -d -p 8082:80 --rm --name ejemplo1_2 -v $(pwd)/index.2.html:/usr/share/nginx/html/index.html nginx:alpine
```

```bash
docker run -d -p 8083:80 --rm --name ejemplo1_3 -v $(pwd)/index.3.html:/usr/share/nginx/html/index.html nginx:alpine
```

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo1_1
```

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo1_2
```

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo1_3
```

```bash
docker run -d -p 8080:80 --rm --name ejemplo1_0 -v $(pwd)/nginx.conf:/etc/nginx/conf.d/default.conf nginx:alpine
```

### 3.2 Construir una imagen con Dockerfile y crear un contenedor a partir de ella

Ingresar a la carpeta del ejemplo 2 y construir la imagen

```bash
cd ejemplo2
```

```bash
docker build -t ejemplo2_nginx:v1 .
```

Ejecutar el contenedor a partir de la imagen creada

```bash
docker run -d -p 8080:80 --name ejemplo2_nginx ejemplo2_nginx:v1
```

### 3.3 Aplicación PHP Simple con Servidor Web Embebido

Ingresar a la carpeta del ejemplo 3 y construir la imagen

```bash
cd ejemplo3
```

```bash
docker build -t ejemplo3_node:v1 .
```

Ejecutar el contenedor a partir de la imagen creada

```bash
docker run -d -p 8080:3000 --name ejemplo3_node ejemplo3_node:v1
```

### 3.4 Aplicación PHP Simple con Servidor Web Embebido

Construcción de la imagen

```bash
docker build -t ejemplo4_php:v1 .
```

Iniciar el contenedor

```bash
docker run -d -p 8000:8080 --name ejemplo4_php:v1 ejemplo4_php
```