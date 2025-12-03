# Scheduler

For [Vietnamese](./README.vi.md)

This is a project that demonstrates a scheduler application built using a microservices architecture. The project is organized into several services, each with a distinct responsibility.

## Architecture

The application is composed of the following microservices:

-   **`discovery-service`**: Acts as a service registry (Eureka) where all other microservices register themselves. This allows services to locate and communicate with each other dynamically.

-   **`api-gateway`**: The single entry point for all client requests. It routes requests to the appropriate microservice and is responsible for cross-cutting concerns like security and logging.

-   **`auth-service`**: Handles user authentication and authorization. It is responsible for issuing and validating security tokens.

-   **`user-service`**: Manages all user-related information and operations, such as teachers and students.

-   **`school-service`**: Manages data related to the school, which could include schedules, subjects, and student classes.

## Technologies Used

-   **Backend**: Java
-   **Framework**: Spring Boot & Spring Cloud
-   **Build Tool**: Apache Maven
-   **Architecture**: Microservices
-   **Database**: MySQL

## Getting Started

To get the application running locally, you will need to have Java (JDK) and Maven installed on your system.

### Cloning the Repository

This repository contains submodules for each microservice. To clone the repository and all its submodules.
```bash
git clone --recurse-submodules https://github.com/HLoc26/scheduler.git
```

If you have already cloned the repository without initializing the submodules, can run the following command from the root of the project:

```bash
git submodule update --init --recursive
```

### Updating Submodules

To pull the latest changes for the main project and all submodules, run:
```bash
git pull --recurse-submodules
```

### Prerequisites

-   Java JDK 17 or later
-   Apache Maven

### Docker Compose
Kafka and MySQL can be run using Docker Compose.

**Kafka**

- Runs in KRaft mode with the following exposed ports:
  - 9092 (PLAINTEXT)
  - 9093 (Controller)

**MySQL**

- Configured with a predefined root password:
  - Internal port: 3306
  - Exposed as: 3300

### Building the Services

Each service is a standalone Maven project. To build all services, you can navigate to each service's root directory and run the Maven wrapper script.

For each of the services (`discovery-service`, `api-gateway`, `auth-service`, `user-service`, `school-service`):

```bash
# Navigate to the service directory (e.g. school-service)
cd ./school-service

# On Windows
./mvnw.cmd clean install

# On macOS/Linux
./mvnw clean install
```

### Running the Application

The services should be started in a specific order to ensure proper registration and discovery.

1.  **Set up environment variables**: Create a `.env` file with keys in `.env.example`.
2.  **Start `docker-compose.yaml`**: Start Kafka and MySQL database with `docker-compose up -d`.
3.  **Start `discovery-service`**: This should be the first service to start.
4.  **Start other services**: Once the discovery service is running, you can start the `auth-service`, `user-service`, `school-service`.
5.  **Start `api-gateway`**: The gateway should be started after the other services it depends on are up and running.

To run a service, navigate to its directory and use the Spring Boot Maven plugin:

```bash
./mvnw.cmd spring-boot:run
```