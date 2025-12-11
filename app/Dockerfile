# Usamos la imagen base de Alpine Linux
FROM node:18-alpine

# Establecemos el directorio de trabajo
WORKDIR /app

# Copiamos los archivos del proyecto al contenedor
COPY . .

# Instalamos las dependencias del proyecto
RUN yarn install

# Exponemos el puerto de la aplicación
EXPOSE 3000

# Comando por defecto para ejecutar la aplicación
CMD ["node", "src/index.js"]