# Etapa 1: Compilación
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app

# Copiar archivos de dependencias
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Asegurar permisos de ejecución del wrapper en builds desde Windows y descargar dependencias en caché
RUN chmod +x mvnw && ./mvnw dependency:go-offline -B

# Copiar código fuente y compilar
COPY src src
RUN ./mvnw clean package -DskipTests

# Etapa 2: Entorno de ejecución ligero
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Fuentes requeridas por Java AWT / ZXing para generar códigos de barras con texto
RUN apk add --no-cache fontconfig ttf-dejavu

# Crear usuario no root por seguridad
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copiar el .jar compilado desde la etapa anterior
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]

