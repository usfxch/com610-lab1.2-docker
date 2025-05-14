# Usa la imagen base de NGINX con la versión alpine
FROM nginx:alpine

# Copia el archivo index.html con el texto "Hola Mundo" al directorio de NGINX
COPY index.html /usr/share/nginx/html/index.html

# Expone el puerto 80 para que NGINX sea accesible desde fuera del contenedor
EXPOSE 8080