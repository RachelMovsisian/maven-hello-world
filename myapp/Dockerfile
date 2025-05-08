# Download pom dependencies
FROM maven:3.9.9-eclipse-temurin-17-alpine AS dependencies
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Run tests
FROM maven:3.9.9-eclipse-temurin-17-alpine AS test
WORKDIR /app
COPY --from=dependencies /root/.m2 /root/.m2
COPY pom.xml .
COPY src ./src
RUN mvn surefire:test -B

# Clean and build artifact 
FROM maven:3.9.9-eclipse-temurin-17-alpine AS build
WORKDIR /app
COPY --from=dependencies /root/.m2 /root/.m2
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests -B

# Run app
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
RUN groupadd -r app && useradd -r -g app app && chown -R app:app /app
USER app
ENTRYPOINT ["java"]
CMD ["-jar", "/app/app.jar"]


