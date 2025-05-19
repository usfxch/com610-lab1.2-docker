# Especifica la imagen base de Python desde la que vamos a construir
FROM python:3.13.3-slim-bookworm

# Establece el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copia el archivo de requerimientos al directorio de trabajo
COPY requirements.txt .

# Instala las dependencias de Python
RUN pip install -r requirements.txt

# Copia el resto del código de la aplicación al directorio de trabajo
COPY . .

# Expone el puerto en el que la aplicación Flask escuchará (por defecto es 5000)
EXPOSE 5000

# Comando para ejecutar la aplicación Python cuando se inicie el contenedor
CMD ["python", "app.py"]