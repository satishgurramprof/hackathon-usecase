# ---- Build stage ----
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy only pom first to leverage Docker layer caching
COPY pom.xml .
RUN mvn -q -e -DskipTests dependency:go-offline

# Copy sources and build
COPY src ./src
RUN mvn -q -DskipTests package

# ---- Runtime stage ----
FROM eclipse-temurin:17-jre
WORKDIR /app

# Copy the built jar (adjust if your jar name differs)
COPY --from=build /app/target/*.jar app.jar

# Optional: run as non-root
RUN useradd -r -u 10001 appuser && chown -R appuser:appuser /app
USER 10001

EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
