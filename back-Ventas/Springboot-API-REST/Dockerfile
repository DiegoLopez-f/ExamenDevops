# Etapa 1: Build
FROM maven:3.9-eclipse-temurin-17-alpine AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Runner
FROM eclipse-temurin:17-jre-alpine AS runner
WORKDIR /app

# Creación de usuario no root por seguridad
RUN addgroup --system --gid 1001 springgroup \
    && adduser --system --uid 1001 springuser

COPY --from=builder --chown=springuser:springgroup /app/target/*.jar app.jar

# Cambiamos al usuario seguro
USER springuser

EXPOSE 8080
CMD ["java", "-jar", "app.jar"]