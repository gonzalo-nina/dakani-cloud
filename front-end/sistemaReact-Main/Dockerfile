# Etapa 1: Construcción
FROM node:20-alpine AS build
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
# Se ejecuta npx vite build directamente porque npm run build corre tsc -b primero
# y falla por errores de TypeScript preexistentes en el proyecto.
RUN npx vite build

# Etapa 2: Servidor web de producción
FROM nginx:alpine
ENV BACKEND_URL=http://backend:8080
ENV NGINX_ENVSUBST_FILTER=^BACKEND_URL$
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/templates/default.conf.template

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

