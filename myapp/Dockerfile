# Download pom dependencies
FROM maven:3.9.9-eclipse-temurin-17-alpine AS dependencies
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Increase the patch part of the JAR file
FROM maven:3.9.9-eclipse-temurin-17-alpine AS patch
WORKDIR /app
COPY --from=dependencies /root/.m2 /root/.m2
COPY pom.xml .
COPY src ./src
RUN version=$(mvn help:evaluate -Dexpression=project.version -q -DforceStdout) && \
    echo "Current version: $version" && \
    new_version=$(echo $version | awk -F. '{printf "%d.%d.%d", $1, $2, $3+1}') && \
    echo "New version: $new_version" && \
    mvn versions:set -DnewVersion=$new_version && \
    mvn versions:commit

# Clean and build artifact 
FROM maven:3.9.9-eclipse-temurin-17-alpine AS build
WORKDIR /app
COPY --from=patch /app /app
RUN mvn clean package -DskipTests -B

# Run app
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
COPY --from=build /app/pom.xml pom.xml
RUN groupadd -r app && useradd -r -g app app && chown -R app:app /app
USER app
ENTRYPOINT ["java"]
CMD ["-jar", "/app/app.jar"]


