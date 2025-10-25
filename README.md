# Dockerized Patient Management System

This repository contains a modular patient management system designed to practice building microservices using Java, Spring Boot, and Docker. Each service is independently containerized and communicates via RESTful APIs or gRPC.

## Purpose

The goal of this project is to provide a hands-on environment for developing and deploying microservices without relying on Docker Compose. It emphasizes service isolation, protocol diversity, and manual container orchestration to simulate real-world backend infrastructure.

## Technologies

- Java
- Spring Boot
- Docker
- RESTful APIs
- gRPC
- Microservices Architecture

## Modules

Each module below represents a standalone microservice or supporting component. Docker setup instructions are intended to be configured manually within IntelliJ using Docker SDK integration.

## Docker setup
I personally used the IntelliJ Idea Docker UI setup for this project, setting up a basic dockerfile for each service or pulling from the official postgresql image for databases.
The environment variable string can be directly pasted into the IntelliJ Idea UI. Anything with a {} is a per-usecase option that needs to be changed from the user.

- [Patient Service]
  - Name: patient-service
  - Dockerfile: Dockerfile
  - Image ID or name: patient-service:latest
  - Container name: patient-service
  - Environment variables: "BILLING_SERVICE_ADDRESS=billing-service;BILLING_SERVICE_GRPC_PORT={port};SPRING_DATASOURCE_PASSWORD={database password};SPRING_DATASOURCE_URL=jdbc:postgresql://patient-service-db:5432/db;SPRING_DATASOURCE_USERNAME={database username};SPRING_JPA_HIBERNATE_DLL_AUTO={DLL_AUTO};SPRING_SQL_INIT_MODE={SQL_INIT_MODE}"
  - Run options: --network internal

- [Auth Service]
  - Name: auth-service
  - Dockerfile: Dockerfile
  - Image ID or name: auth-service:latest
  - Container name: auth-service
  - Environment variables: "JWT_SECRET={Generated JWT secret};SPRING_DATASOURCE_PASSWORD={password};SPRING_DATASOURCE_URL=jdbc:postgresql://auth-service-db:5432/db;SPRING_DATASOURCE_USERNAME={username};SPRING_JPA_HIBERNATE_DDL_AUTO={DDL_AUTO};SPRING_SQL_INIT_MODE={SQL_INIT_MODE}"

- [Billing Service]
  - Name: billing-service
  - Image ID or name: billing-service:latest
  - Container name: billing-service
  - Bind ports: {host port}:{container port} {host port}:{container port}
  - Run options: --network internal

- [Patient Service DB]
  - Name: patient-service-db
  - Image ID or name: postgres:latest
  - Container name: patient-service-db
  - Bind ports: {host port}:5432
  - Bind mounts:
    - Host path: {host path address}
    - Container path: /var/lib/postgresql/18/main
  - Environment variables: POSTGRES_DB={db name};POSTGRES_PASSWORD={database password};POSTGRES_USER={database username}
  - Run options: --network internal

- [Auth Service DB]
  - Name: auth-service-db
  - Container name: auth-service-db
  - Bind ports: {host port}:5432
  - Bind mounts:
    - Host path: {host path address}
    - Container path: /var/lib/postgresql/18/main
  - Environment variables: POSTGRES_DB={db name};POSTGRES_PASSWORD={database password};POSTGRES_USER={database username}

 - [API Gateway]
   - Name: api-gateway
   - Dockerfile: Dockerfile
   - Image ID or name: api-gateway:latest
   - Container name: api-gateway
   - Bind ports: {host port}:{container port}
   - Environment variables: "AUTH_SERVICE_URL=http://auth-service:{auth service port}"
   - Run options: --network internal

What I learned from this project:
I learned many industry concepts from this project and reinforced my knowledge on the microservices ecosystem.
 - Java and Spring Boot
   - Practiced Java and Spring Boot in other projects and had previous knowledge regarding controllers, services, repositories, building exception handlers, and dependency injection.
   - I learned more about the configuration settings around seperate modules for Spring Boot and how the framework allows for easy and quick configurations along with configurations built in.
   - I learned more about the data annotations within Spring Boot that help to identify commonly used services along with data validation for DTOs.
   - I learned more about the power of Hibernate and JPA to automatically generate easy/commonly used queries for a database. 
 - Security
   - I learned more about the security package within Spring that allows for JWT token authentication and authorization.
   - I learned how to setup token generation/validation along with configuring the token authentication/authorization to happen throughout all requests using the API gateway to filter traffic.
 - Docker
   - I learned more about how IntelliJ builds Docker containers within the IDE and how to easily setup images to build containers that interact with each other.
 - API Gateways
   - I learned how to build an API gateway within Spring Boot and how to configure the API gateway through an application.yml configuration file.
   - I learned more about how modules can be setup for a microservice ecosystem and how the API gateway can regulate and filter the traffic that wants to access these microservices.
 - gRPC
   - I learned the basics of gRPCs and how useful they can be for sending traffic through HTTP channels along with the differences of gRPCs compared to communcations for RESTful APIs.
   - I learned how to build and compile a .proto file for making a simple billing service that is used to send information whenever a patient gets created from the patient-service module.

  Overall, I loved building out this project and learning more about the system structures of microservices!

