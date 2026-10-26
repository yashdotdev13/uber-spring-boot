# Uber Backend — Spring Boot

<p align="center">
  <strong>A RESTful ride-hailing backend built with Java and Spring Boot.</strong><br/>
  Rider and driver workflows, ride lifecycle management, JWT-based authentication, ratings, and wallet/payment components.
</p>

<p align="center">
  <a href="https://github.com/yashdotdev13/uber-spring-boot"><img src="https://img.shields.io/badge/Repository-uber--spring--boot-181717?logo=github" alt="GitHub repository"></a>
  <img src="https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white" alt="Java 21">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.3.1-6DB33F?logo=springboot&logoColor=white" alt="Spring Boot 3.3.1">
  <img src="https://img.shields.io/badge/Build-Maven-C71A36?logo=apachemaven&logoColor=white" alt="Maven">
  <img src="https://img.shields.io/badge/Database-PostgreSQL-4169E1?logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/API-REST-009688" alt="REST API">
  <img src="https://img.shields.io/badge/Authentication-JWT-000000?logo=jsonwebtokens&logoColor=white" alt="JWT authentication">
</p>

<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#features">Features</a> ·
  <a href="#technology-stack">Tech Stack</a> ·
  <a href="#getting-started">Getting Started</a> ·
  <a href="#api-overview">API Overview</a> ·
  <a href="#testing">Testing</a> ·
  <a href="#contributing">Contributing</a>
</p>

---

## Overview

**Uber Backend** is a Spring Boot application that provides backend APIs for a ride-hailing platform. It models the core interaction between riders and drivers, including ride requests, ride acceptance, ride start and completion, cancellation, ride history, and feedback.

The application follows a layered structure with controllers, services, repositories, entities, DTOs, and configuration components. It uses PostgreSQL for persistence, Spring Data JPA for data access, Spring Security and JWT utilities for authentication, and strategy interfaces to keep selected business decisions extensible.

> **Project scope:** This repository contains the backend application. A frontend/mobile client and production deployment configuration are not included in this project directory.

## Features

### Rider workflows
- Register an account and sign in.
- Request a ride using pickup and destination information.
- Cancel a ride.
- Rate a driver after a ride.
- Retrieve the rider profile and paginated ride history.

### Driver workflows
- Access driver-specific routes protected by role-based authorization.
- Accept ride requests.
- Start a ride using an OTP.
- End or cancel a ride.
- Rate a rider.
- Retrieve the driver profile and paginated ride history.

### Authentication and authorization
- JWT-based authentication utilities and request filtering.
- Sign-up, login, and token-refresh endpoints.
- Role-based access for rider, driver, and administrative workflows.
- HTTP-only cookie usage for a token in the login flow.

### Ride and platform components
- Ride request and ride lifecycle services.
- Driver matching strategy abstraction.
- Fare calculation strategy abstraction.
- Payment strategy abstraction and payment service.
- Wallet and wallet-transaction components.
- Rider/driver ratings.
- Email-sending service configuration.
- DTO-based request/response boundaries and ModelMapper configuration.
- Centralized API response and exception handling.
- Springdoc OpenAPI dependency for API documentation.
- Spring Boot Actuator dependency for application monitoring endpoints.

## Technology Stack

| Technology | Purpose |
| --- | --- |
| Java 21 | Application language and runtime |
| Spring Boot 3.3.1 | Application framework |
| Spring Web | REST API controllers |
| Spring Security | Authentication and authorization |
| JSON Web Token (JJWT) | JWT handling |
| Spring Data JPA / Hibernate | Persistence and ORM |
| PostgreSQL | Relational database |
| Hibernate Spatial | Spatial data support |
| ModelMapper | DTO/entity mapping |
| Spring Boot Starter Mail | SMTP email integration |
| Springdoc OpenAPI | OpenAPI specification and Swagger UI integration |
| Spring Boot Actuator | Application health and operational endpoints |
| Lombok | Boilerplate reduction |
| Maven Wrapper | Reproducible Maven invocation |
| Spring Boot Test | Testing support |
| Testcontainers for PostgreSQL | PostgreSQL-backed integration testing support |

## Architecture

The codebase is organized by responsibility:

```text
uperApp/
├── pom.xml
├── mvnw
├── mvnw.cmd
└── src/
    ├── main/
    │   ├── java/com/company/project/uber/uperApp/
    │   │   ├── Security/       # JWT service and authentication filter
    │   │   ├── advices/        # Common API responses and exception handling
    │   │   ├── configs/        # Security and mapping configuration
    │   │   ├── controllers/    # Authentication, rider, and driver endpoints
    │   │   ├── dto/            # Request and response objects
    │   │   ├── entities/       # Persistence model
    │   │   ├── exceptions/     # Domain and resource exceptions
    │   │   ├── repositories/   # Spring Data repositories
    │   │   ├── services/       # Application/business services
    │   │   ├── strategies/     # Matching, fare, and payment strategies
    │   │   └── utils/          # Geometry and supporting utilities
    │   └── resources/
    │       ├── application.properties
    │       ├── application-dev.properties
    │       ├── application-prod.properties
    │       └── data.sql
    └── test/
        └── java/com/company/project/uber/uperApp/
```

The structure above is representative of the current source tree; individual classes and responsibilities may evolve as the project develops.

## Getting Started

### Prerequisites

Install the following before running the project:

- **JDK 21**
- **PostgreSQL** (a local instance or a reachable database)
- **Git**
- An SMTP account only if you need email functionality

### 1. Clone the repository

From a terminal, run:

```bash
git clone https://github.com/yashdotdev13/uber-spring-boot.git
cd uber-spring-boot/uperApp
```

### 2. Create a PostgreSQL database

Create a database for local development. For example, using the PostgreSQL CLI:

```sql
CREATE DATABASE uber_app;
```

Use a database user that has permission to connect to and modify this database.

### 3. Configure environment variables

The application has development and production property profiles. Configure database credentials, the JWT signing secret, and SMTP credentials outside source control. **Do not commit real passwords, API keys, mail app passwords, or JWT secrets.**

For a production-style profile, configure these variables to match `application-prod.properties`:

| Variable | Description |
| --- | --- |
| `DB_HOST_URL` | PostgreSQL host name |
| `DB_NAME` | Database name |
| `DB_USERNAME` | Database user |
| `DB_PASSWORD` | Database password |
| `JWT_SECRET_KEY` | Secret used for signing/verifying JWTs; use a strong, unique value |
| `SMTP_PASSWORD` | SMTP app password or credential |

The checked-in development configuration expects `POSTGRES_PASSWORD` for its database password. Review `src/main/resources/application-dev.properties` before starting the development profile and ensure the database URL and username match your local setup.

For a local shell session, set the values using your operating system's environment-variable syntax. On PowerShell, for example:

```powershell
$env:POSTGRES_PASSWORD = "replace-with-your-local-db-password"
```

For the production-style profile, set the variables in the same terminal or deployment environment. Do not paste real values into README files, issue reports, screenshots, or commits.

### 4. Run the application

The project includes Maven Wrapper scripts, so a separate Maven installation is not required.

**Windows (PowerShell / Command Prompt):**

```powershell
.\mvnw.cmd spring-boot:run
```

**macOS / Linux:**

```bash
chmod +x mvnw
./mvnw spring-boot:run
```

To run with a specific Spring profile, activate it explicitly:

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```

On Windows, use `.\mvnw.cmd spring-boot:run -Dspring-boot.run.profiles=dev`.

Make sure the active profile's required environment variables are defined and the PostgreSQL server is running before starting the application.

### 5. Build the application

```bash
./mvnw clean package
```

On Windows:

```powershell
.\mvnw.cmd clean package
```

The packaged artifact is normally generated under `target/`.

## API Overview

The following routes are declared in the current controllers. Request bodies and authentication requirements depend on the DTOs and Spring Security configuration.

### Authentication

Base path: `/auth`

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/auth/signup` | Register a user |
| `POST` | `/auth/login` | Authenticate and receive login token response |
| `POST` | `/auth/refresh` | Refresh an access token using a refresh-token cookie |
| `POST` | `/auth/onBoardNewDriver/{userId}` | Onboard a driver (admin role required) |

### Rider

Base path: `/riders` — requires the rider role.

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/riders/requestRide` | Request a ride |
| `POST` | `/riders/cancelRide/{rideId}` | Cancel a ride |
| `POST` | `/riders/rateDriver` | Submit a driver rating |
| `GET` | `/riders/getMyProfile` | Retrieve the current rider's profile |
| `GET` | `/riders/getMyRides` | Retrieve paginated ride history |

### Driver

Base path: `/drivers` — requires the driver role.

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/drivers/acceptRide/{rideRequestId}` | Accept a ride request |
| `POST` | `/drivers/startRide/{rideRequestId}` | Start a ride using the OTP |
| `POST` | `/drivers/endRide/{rideId}` | Complete a ride |
| `POST` | `/drivers/cancelRide/{rideId}` | Cancel a ride |
| `POST` | `/drivers/rateRider` | Submit a rider rating |
| `GET` | `/drivers/getMyProfile` | Retrieve the current driver's profile |
| `GET` | `/drivers/getMyRides` | Retrieve paginated ride history |

The ride-history endpoints accept `pageOffset` (default `0`) and `pageSize` (default `10`) query parameters.

### Example: Request a ride

The endpoint expects a request body matching `RideRequestDto`. Check that DTO before sending a request, since its fields are the source of truth for the current API contract.

```http
POST /riders/requestRide
Content-Type: application/json
Authorization: Bearer <access-token>
```

### API documentation

The project includes the Springdoc OpenAPI WebMVC UI dependency. After starting the application, try the Swagger UI endpoint:

```text
http://localhost:8080/swagger-ui.html
```

The exact URL can depend on Springdoc configuration and the application's context path. If it does not open, check the startup logs and Springdoc configuration.

## Configuration Profiles

| File | Intended purpose |
| --- | --- |
| `application.properties` | Default application configuration |
| `application-dev.properties` | Local/development configuration |
| `application-prod.properties` | Production-oriented configuration using environment placeholders |

Review these files before running the application. Do not assume a profile is production-ready without validating database schema strategy, secret management, logging, CORS, cookie settings, and deployment-specific configuration.

## Testing

Run the test suite with the Maven Wrapper:

```bash
./mvnw test
```

On Windows:

```powershell
.\mvnw.cmd test
```

The project includes Spring Boot testing dependencies and Testcontainers' PostgreSQL module. Integration tests that use Testcontainers may require Docker to be available.

Before submitting a change, run the relevant tests and then the full test suite where practical. Avoid documenting a test badge as passing unless a corresponding CI workflow reports a successful run.

## Security Notes

- Keep database credentials, SMTP credentials, and JWT signing keys out of source control.
- Use a strong, unique JWT secret supplied through an environment variable or a secret manager.
- Use HTTPS in deployed environments and validate secure cookie settings.
- Review token expiration, refresh-token handling, and logout/revocation behavior before production use.
- Follow the principle of least privilege for database users and application roles.
- Review Actuator exposure and restrict operational endpoints as appropriate.
- The repository configuration should be audited to ensure no real secrets are stored in tracked files. If any credentials have been committed, rotate them; removing them from the latest version alone does not invalidate exposed credentials.

## Troubleshooting

**The application cannot connect to PostgreSQL**
- Confirm PostgreSQL is running and reachable.
- Verify the database name, host, username, and password for the active profile.
- Ensure the relevant environment variables are set in the same terminal where the app is launched.

**The application fails to start with a port-in-use error**
- Stop the process already using the configured port or set a different server port.

**Swagger UI is unavailable**
- Confirm the application started successfully and check the Springdoc dependency/configuration.
- Try the configured Swagger UI URL and inspect application logs for the effective context path.

**Tests fail during Testcontainers startup**
- Confirm Docker is installed, running, and accessible to the current user.
- Check the test logs for container startup or database initialization errors.

## Contributing

Contributions, bug reports, and improvement ideas are welcome.

1. Fork the repository.
2. Create a focused branch: `git checkout -b feature/your-change`.
3. Make your change and add or update tests where appropriate.
4. Run `./mvnw test` (or `.\mvnw.cmd test` on Windows).
5. Commit your changes with a descriptive message.
6. Open a pull request that explains the motivation, implementation, and testing performed.

Please avoid committing secrets, build output, IDE settings, or unrelated formatting changes.

## Roadmap Ideas

Potential future improvements for a more production-ready ride-hailing backend include:

- Add API contract examples and a Postman collection.
- Add integration tests for authentication, ride state transitions, role authorization, wallet transactions, and failure cases.
- Strengthen input validation and standardize API error payloads.
- Add CI workflows for build and test verification.
- Improve secret management and review JWT cookie/token handling.
- Add containerized local development with PostgreSQL and documented setup.
- Document fare calculation, driver matching, payment behavior, and ride state transitions.

These are suggestions, not claims that the features already exist.

## License

No license is specified in this project README. Please check the repository's license file and project ownership terms before redistributing or using the code.

---

<p align="center">
  Built with Java and Spring Boot · <a href="https://github.com/yashdotdev13">Yash Chauhan on GitHub</a>
</p>
