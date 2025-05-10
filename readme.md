# CI/CD Pipeline for Hello World Java App

An implementation of CI/CD pipeline using GitHub Actions, Dockerfile, and Docker Hub for the "maven-hello-world" project, forked from: [https://github.com/ido83/maven-hello-world](https://github.com/ido83/maven-hello-world)

## Overview
This project demonstrates a complete automated workflow for building, versioning, and delivering a Java application using:

- **GitHub Actions** for CI/CD automation
- **Docker multi-stage builds** for efficient image creation, with an emphasis on performing as many steps as possible within the multi-stage Dockerfile.

The CI/CD pipeline automatically performs the following steps, divided between the Dockerfile and the GitHub Actions workflow:

### Steps handled within the Dockerfile:
* Increase the patch part of the jar version automatically using Maven's versioning plugin.
* Compiles and packages the application into a `.jar` artifact using Maven.
* Creates a non-root user "app" for running the application and sets ownership of the `/app` directory for improved security. 

### Steps handled within the GitHub Actions workflow:
* Builds the Docker image based on the Dockerfile and tags it with the JAR version and "latest".
* Pushes the Docker image to Docker Hub.
* Verifies the image by pulling it from Docker Hub and running it.
* Handles errors by restoring the previous `pom.xml` file and removing the faulty Docker image from Docker Hub.

## Project Structure
```
├── myapp
│   ├── Dockerfile # Multi-stage Docker build file
│   ├── pom.xml
│   └── src
│       ├── main
│       │   └── java
│       │       └── com
│       │           └── myapp
│       │               └── App.java
├── .github
│   └── workflows
│       ├── build-deploy-java-app.yaml  # GitHub Actions workflow
```

## Getting Started

### Prerequisites
* GitHub account
* Docker Hub account

### Setting up secrets
1. Create a new repository in Docker Hub named "hello-world".
   The repository and the image name can be customized by modifying environment variables in the `build-deploy-java-app.yaml` file.
2. Create a Docker Hub access token with permission to read, write, and delete.

### Running the Pipeline
1. Fork the repository.
2. Add Docker Hub username and access token to the GitHub repository secrets:
   - `DOCKER_HUB_USERNAME`: Docker Hub username
   - `DOCKER_HUB_ACCESS_TOKEN`: Docker Hub access token
3. If the default branch of your repository is not `main`, update the `TARGET_BRANCH` environment variable in `build-deploy-java-app.yaml` file.
4. The pipeline will be triggered automatically after a `push` to the target branch.

## Changes in Forked Repository
* Added my name to "Hello World" message output of the app.
* In `pom.xml` file:
  - Set JAR version to `1.0.0`
  - Added repository link.
  - Updated Java version to `17` for security reasons.
