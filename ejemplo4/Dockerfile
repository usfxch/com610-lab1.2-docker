# Usa una imagen base de PHP con el servidor web embebido
FROM php:8.1-cli

# Establece el directorio de trabajo
WORKDIR /app

# Copia los archivos de la aplicación
COPY . .

# Expone el puerto 8080 (un puerto común para servidores de desarrollo)
EXPOSE 8080

# Comando para iniciar el servidor web embebido de PHP
CMD ["php", "-S", "0.0.0.0:8080", "-t", "."]