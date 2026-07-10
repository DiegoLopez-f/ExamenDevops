# Etapa 1: Construcción (Builder)
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Etapa 2: Producción (Imagen minimalista y segura)
FROM nginxinc/nginx-unprivileged:alpine AS runner
# Usuario seguro (UID 101) viene por defecto en esta imagen de Nginx
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]