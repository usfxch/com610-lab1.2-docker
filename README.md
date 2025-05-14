# Laboratorio 1.2: Uso de Contenedores con Docker y Docker Compose

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

## 2\. Gestión y Uso de Contenedores desde la Terminal

Esta sección detalla los comandos básicos para la gestión y el uso de contenedores Docker directamente desde la línea de comandos.

### 2.1 Búsqueda de Imágenes

```bash
docker search nginx
```

> Busca imágenes relacionadas con "nginx" en Docker Hub. La salida mostrará una lista de imágenes, su descripción, el número de estrellas (popularidad) y si son imágenes oficiales (`[OK]`).

### 2.2 Listado de Contenedores

```bash
docker ps
```

> Muestra una lista de todos los contenedores que están actualmente en ejecución.

```bash
docker ps -a
```

> Muestra una lista de todos los contenedores, incluyendo aquellos que están en ejecución (`Up`) y los que están detenidos (`Exited`).

### 2.3 Creación de Contenedores

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

### 2.4 Visualización de Logs

```bash
docker logs mi_nginx_con_puerto
```

> Muestra los logs (salida estándar y error estándar) del contenedor llamado `mi_nginx_con_puerto`. Esto es útil para verificar el funcionamiento de la aplicación dentro del contenedor o para depurar problemas.

```bash
docker logs --follow mi_nginx_con_puerto
```

> Muestra los logs del contenedor `mi_nginx_con_puerto` y sigue la salida en tiempo real. Esto es similar a usar `tail -f` en un archivo de log.

### 2.5 Ejecución, Detención y Eliminación de Contenedores

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

### 2.6 Comando `docker run` Todo en Uno

El comando `docker run` puede realizar múltiples acciones en un solo paso:

1.  **Verifica si la imagen existe localmente y, si no, la descarga.**
2.  **Crea un nuevo contenedor a partir de la imagen.**
3.  **Ejecuta el contenedor.**

El siguiente ejemplo muestra cómo hacer esto en segundo plano (`-d`), estableciendo un nombre (`--name`), mapeando un puerto (`-p`) y montando un volumen (`-v`):

```bash
docker run -d -p 8080:80 --name mi_nginx_con_puerto -v $(pwd)/index.html:/usr/share/nginx/html/index.html nginx:alpine
```

> Ejecuta un contenedor en segundo plano (`-d`) basado en la imagen `nginx:alpine`. Mapea el puerto 80 del contenedor al puerto 8080 del host (`-p 8080:80`). Le asigna el nombre `mi_nginx_con_puerto` (`--name mi_nginx_con_puerto`). Además, monta un volumen (`-v`). En este caso, el archivo `index.html` ubicado en el directorio actual de la máquina host (`$(pwd)/index.html`) se monta en el directorio `/usr/share/nginx/html/index.html` dentro del contenedor (donde Nginx sirve sus archivos web).