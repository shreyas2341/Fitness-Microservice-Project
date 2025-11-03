# Build stage
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app

# Copy module-level maven wrapper and files (works if mvnw is present in the module)
# If your module doesn't have mvnw, Docker will still work but you should use a multi-stage maven image instead.
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
# copy all source files from module
COPY src ./src

# Ensure mvnw executable and build
RUN chmod +x mvnw || true
RUN ./mvnw -B clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app

# copy jar built in build stage
COPY --from=build /app/target/*.jar /app/app.jar

# Use SERVER_PORT env var (set by Render) to set Spring Boot server port
EXPOSE 8080
ENV SERVER_PORT=8080
ENTRYPOINT ["sh","-c","java -Dserver.port=${SERVER_PORT} -jar /app/app.jar"]
