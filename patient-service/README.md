# Patient Management System - Patient Service

A robust Spring Boot microservice for managing patient data in a healthcare management system. This service provides comprehensive CRUD operations for patient records with validation, exception handling, and modern API documentation.

## 🚀 Features

- **Complete Patient Management**: Create, read, update, and delete patient records
- **Email Validation**: Ensures unique email addresses across all patients
- **Data Validation**: Comprehensive input validation with custom validation groups
- **Exception Handling**: Global exception handling with meaningful error messages
- **API Documentation**: Interactive Swagger UI documentation
- **gRPC Support**: Built-in gRPC capabilities for inter-service communication
- **Kafka Integration**: Message broker support for event-driven architecture
- **Docker Ready**: Multi-stage Dockerfile for production deployment
- **PostgreSQL Integration**: Configured for PostgreSQL database

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [API Endpoints](#api-endpoints)
- [Configuration](#configuration)
- [Docker Deployment](#docker-deployment)
- [Development](#development)
- [Testing](#testing)
- [Technologies Used](#technologies-used)
- [Contributing](#contributing)

## 🔧 Prerequisites

- **Java 24** or higher
- **Maven 3.9+**
- **PostgreSQL 12+** (for production)
- **Docker** (optional, for containerized deployment)

## 🏁 Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd patient-service
```

### 2. Database Setup

#### PostgreSQL Setup
1. Install and start PostgreSQL
2. Create a database named `Patient-Management-System`
3. Update the database configuration in `application.properties`

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/Patient-Management-System
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
```

### 3. Run the Application

#### Using Maven
```bash
./mvnw spring-boot:run
```

#### Using Java
```bash
./mvnw clean package
java -jar target/patient-service-0.0.1-SNAPSHOT.jar
```

The application will start on `http://localhost:3000`

### 4. Access API Documentation

- **Swagger UI**: http://localhost:3000/swagger-ui/index.html
- **OpenAPI Spec**: http://localhost:3000/v3/api-docs

## 📁 Project Structure

```
patient-service/
├── src/
│   ├── main/
│   │   ├── java/com/slyder/patientservice/
│   │   │   ├── controller/          # REST Controllers
│   │   │   │   └── PatientController.java
│   │   │   ├── dto/                 # Data Transfer Objects
│   │   │   │   ├── PatientRequestDTO.java
│   │   │   │   ├── PatientResponseDTO.java
│   │   │   │   └── validators/
│   │   │   ├── exception/           # Exception Handling
│   │   │   │   ├── GlobalExceptionHandler.java
│   │   │   │   ├── PatientNotFoundException.java
│   │   │   │   └── EmailAlreadyExistsException.java
│   │   │   ├── mapper/              # Entity-DTO Mappers
│   │   │   │   └── PatientMapper.java
│   │   │   ├── model/               # JPA Entities
│   │   │   │   └── Patient.java
│   │   │   ├── repository/          # Data Access Layer
│   │   │   │   └── PatientRepository.java
│   │   │   ├── service/             # Business Logic
│   │   │   │   └── PatientService.java
│   │   │   └── PatientServiceApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
├── api-requests/                    # HTTP request examples
├── Dockerfile
├── pom.xml
└── README.md
```

## 🌐 API Endpoints

### Base URL: `http://localhost:3000`

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET    | `/patients` | Retrieve all patients | - |
| POST   | `/patients` | Create a new patient | PatientRequestDTO |
| PUT    | `/patients/{id}` | Update existing patient | PatientRequestDTO |
| DELETE | `/patients/{id}` | Delete a patient | - |

### Request/Response Examples

#### Create Patient
```http
POST /patients
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "address": "123 Main Street, City, State",
  "dateOfBirth": "1990-05-15",
  "registeredDate": "2025-09-13"
}
```

#### Response
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "address": "123 Main Street, City, State",
  "dateOfBirth": "1990-05-15"
}
```

## ⚙️ Configuration

### Application Properties

The application can be configured through `src/main/resources/application.properties`:

```properties
# Application Configuration
spring.application.name=patient-service
server.port=3000

# Database Configuration (Uncomment and configure for PostgreSQL)
# spring.datasource.url=jdbc:postgresql://localhost:5432/Patient-Management-System
# spring.datasource.username=postgres
# spring.datasource.password=your_password
# spring.jpa.hibernate.ddl-auto=update

# Logging Configuration
logging.level.root=info
```

### Environment Variables

For production deployment, use environment variables:

- `DB_URL`: Database connection URL
- `DB_USERNAME`: Database username  
- `DB_PASSWORD`: Database password
- `SERVER_PORT`: Application port (default: 3000)

## 🐳 Docker Deployment

### Build Docker Image

```bash
docker build -t patient-service:latest .
```

### Run with Docker

```bash
# Run the application
docker run -p 3000:8080 patient-service:latest

# Run with environment variables
docker run -p 3000:8080 \
  -e DB_URL=jdbc:postgresql://host.docker.internal:5432/Patient-Management-System \
  -e DB_USERNAME=postgres \
  -e DB_PASSWORD=your_password \
  patient-service:latest
```

### Docker Compose (Recommended)

Create a `docker-compose.yml` file:

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: Patient-Management-System
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: your_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  patient-service:
    build: .
    ports:
      - "3000:8080"
    depends_on:
      - postgres
    environment:
      - DB_URL=jdbc:postgresql://postgres:5432/Patient-Management-System
      - DB_USERNAME=postgres
      - DB_PASSWORD=your_password

volumes:
  postgres_data:
```

Run with: `docker-compose up -d`

## 🛠️ Development

### Running in Development Mode

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```

### Code Style

This project follows standard Java conventions:
- Use meaningful variable and method names
- Follow proper package structure
- Add comprehensive validation
- Include proper exception handling

### Adding New Features

1. Create/Update entity in `model/`
2. Add repository methods in `repository/`
3. Implement business logic in `service/`
4. Add REST endpoints in `controller/`
5. Create appropriate DTOs in `dto/`
6. Add validation and exception handling
7. Update API documentation

## 🧪 Testing

### Run Tests

```bash
# Run all tests
./mvnw test

# Run with coverage
./mvnw test jacoco:report
```

### API Testing

Use the provided HTTP request files in `api-requests/` directory:

- `create-patient.http` - Create patient example
- `get-patients.http` - Retrieve patients
- `update-patient.http` - Update patient example  
- `delete-patient.http` - Delete patient example

## 🛠️ Technologies Used

### Core Technologies
- **Spring Boot 3.5.5** - Application framework
- **Java 24** - Programming language
- **Maven** - Build tool and dependency management

### Spring Ecosystem
- **Spring Data JPA** - Data persistence layer
- **Spring Web** - REST API development
- **Spring Validation** - Input validation
- **Spring Kafka** - Message broker integration

### Database & Persistence
- **PostgreSQL** - Primary database
- **Hibernate** - JPA implementation

### Communication & Integration
- **gRPC** - Inter-service communication
- **Apache Kafka** - Event streaming platform

### Documentation & Development Tools
- **OpenAPI 3 / Swagger** - API documentation
- **Spring Boot DevTools** - Development productivity

### Containerization
- **Docker** - Containerization platform

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Guidelines

- Follow existing code style and conventions
- Add comprehensive tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting PR
- Add meaningful commit messages

## 📞 Support

If you encounter any issues or have questions:

1. Check the [API Documentation](http://localhost:3000/swagger-ui/index.html)
2. Review the logs for error details
3. Create an issue in the repository

---

**Built with ❤️ using Spring Boot**
