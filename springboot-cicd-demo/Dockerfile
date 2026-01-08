# ---------- BUILD STAGE ----------
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy riêng pom.xml để cache dependency
COPY pom.xml .
RUN mvn -q dependency:go-offline

# Copy source
COPY src ./src

# Build JAR
RUN mvn -q package -DskipTests


# ---------- RUNTIME STAGE ----------
FROM eclipse-temurin:17-jdk
WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8081

ENTRYPOINT ["java","-jar","/app/app.jar"]