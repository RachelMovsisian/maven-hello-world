# A simple, minimal Maven example: hello world

## Overview
This project demonstrates a complete automated workflow for building, versioning, and delivering the Java application using:

- **GitHub Actions** for CI/CD automation
- **Docker multi-stage builds** for efficient image creation.

The CI/CD pipeline automatically performs the following steps, divided between the Dockerfile and the GitHub Actions workflow:

### Steps handled within the [Dockerfile](./myapp/Dockerfile):
* Increase the patch part of the JAR version automatically.
* Package the application into artifact. 

### Steps handled within the [GitHub Actions workflow](.github/workflows/build-deploy-java-app.yaml):
* Build the Docker image based on the Dockerfile and update the JAR version in pom.xml file.
* Push the Docker image to Docker Hub with JAR version and "latest" tags.

## Project Structure
```
├── myapp
│   ├── Dockerfile
│   ├── pom.xml
│   └── src
│       ├── main
│       │   └── java
│       │       └── com
│       │           └── myapp
│       │               └── App.java
├── .github
│   └── workflows
│       └── build-deploy-java-app.yaml
```
## Running the Application

You can run this application in several ways:

### Method 1: Using Maven Directly

**Prerequisites:**
JDK 17 or newer, Maven 3.8.1 or newer

**Compile and run directly from classes:**
```
cd myapp
mvn compile
java -cp target/classes com.myapp.App
```

OR

**Package as JAR and run:**
```
cd myapp
mvn package
java -cp target/myapp-<version>.jar com.myapp.App
```

**Clean up build artifacts:**
```
mvn clean
```
This will remove the `target` directory, leaving only the source Java files and pom.xml.

### Method 2: Using Docker Locally

**Prerequisites:**
Docker

**Build and run the Docker image:**
```
cd myapp
docker build -t myapp . && docker run myapp
```

### Method 3: Using GitHub Actions and Docker Hub

1. Push your changes to the master branch:
   ```
   git checkout master
   git add .
   git commit -m "Your message"
   git push origin master
   ```
   or by opening a pull request to merge your branch into the master branch.

2. Download and run the published Docker image from anywhere:
   ```
   docker run justme2024/hello-world:latest
   ```
