# Especifica la imagen base de Node.js desde la que vamos a construir
FROM node:20-alpine

# Establece el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copia los archivos package.json y package-lock.json (si existe) al directorio de trabajo
COPY package*.json .

# Instala las dependencias de la aplicación Node.js
RUN npm install

# Copia el resto del código de la aplicación al directorio de trabajo
COPY . .

# Expone el puerto en el que la aplicación Express.js escuchará (por defecto es 3000)
EXPOSE 3000

# Comando para ejecutar la aplicación Node.js cuando se inicie el contenedor
CMD ["npm", "run", "dev"]