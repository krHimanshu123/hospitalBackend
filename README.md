# 🏥 Hospital Backend

A robust and secure backend API for hospital management systems built with Spring Boot.  This application provides authentication and user management capabilities with JWT-based security.

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.10-brightgreen. svg)](https://spring.io/projects/spring-boot)
[![MySQL](https://img.shields.io/badge/MySQL-8.0+-blue.svg)](https://www.mysql.com/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED.svg)](https://www.docker.com/)

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running with Docker](#running-with-docker)
- [API Endpoints](#api-endpoints)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Security](#security)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- **User Authentication & Authorization**
  - User registration and login
  - JWT-based authentication
  - Password encryption with BCrypt
  - Secure session management

- **RESTful API Design**
  - Clean and intuitive endpoints
  - CORS configuration for cross-origin requests
  - Comprehensive error handling

- **Database Integration**
  - MySQL database with JPA/Hibernate
  - Automatic schema management
  - Repository pattern implementation

- **Security**
  - Spring Security integration
  - Stateless authentication
  - Protected endpoints with role-based access

- **DevOps Ready**
  - Dockerized application
  - Multi-stage Docker build
  - Production-ready configuration

## 🛠 Tech Stack

- **Framework:** Spring Boot 3.4.10
- **Language:** Java 21
- **Database:** MySQL
- **Security:** Spring Security + JWT (JSON Web Tokens)
- **ORM:** Spring Data JPA / Hibernate
- **Build Tool:** Maven
- **Containerization:** Docker

### Dependencies

- `spring-boot-starter-web` - RESTful web services
- `spring-boot-starter-data-jpa` - Database operations
- `spring-boot-starter-security` - Authentication & Authorization
- `mysql-connector-j` - MySQL database driver
- `jjwt` (v0.11.5) - JWT token generation and validation
- `spring-boot-devtools` - Development utilities

## 🏗 Architecture

The application follows a layered architecture pattern:

```
┌─────────────────────────────────────┐
│         Controller Layer            │  (REST Endpoints)
├─────────────────────────────────────┤
│          Service Layer              │  (Business Logic)
├─────────────────────────────────────┤
│        Repository Layer             │  (Data Access)
├─────────────────────────────────────┤
│          Database (MySQL)           │
└─────────────────────────────────────┘
```

## 🚀 Getting Started

### Prerequisites

- **Java 21** or higher
- **Maven 3.9+**
- **MySQL 8.0+**
- **Docker** (optional, for containerized deployment)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/krHimanshu123/hospitalBackend.git
   cd hospitalBackend
   ```

2. **Set up MySQL Database**
   ```sql
   CREATE DATABASE hospital;
   ```

3. **Configure Database Connection**
   
   Update `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/hospital
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   ```

4. **Build the project**
   ```bash
   ./mvnw clean install
   ```

5. **Run the application**
   ```bash
   ./mvnw spring-boot:run
   ```

The application will start on `http://localhost:8081`

### Running with Docker

1. **Build the Docker image**
   ```bash
   docker build -t hospital-backend . 
   ```

2. **Run the container**
   ```bash
   docker run -p 8081:8081 \
     -e SPRING_DATASOURCE_URL=jdbc:mysql://host.docker.internal:3306/hospital \
     -e SPRING_DATASOURCE_USERNAME=root \
     -e SPRING_DATASOURCE_PASSWORD=root \
     hospital-backend
   ```

Alternatively, use Docker Compose for a complete setup with MySQL: 

```yaml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_DATABASE: hospital
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
  
  app:
    build:  .
    ports:
      - "8081:8081"
    environment:
      SPRING_DATASOURCE_URL: jdbc: mysql://mysql:3306/hospital
      SPRING_DATASOURCE_USERNAME:  root
      SPRING_DATASOURCE_PASSWORD: root
    depends_on:
      - mysql
```

## 📡 API Endpoints

### Authentication

#### Register User
```http
POST /auth/signup
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
"User registered successfully!"
```

#### Login
```http
POST /auth/login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "securePassword123"
}
```

**Response:**
```json
"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJqb2huX2RvZSIsImlhdCI6MTYxNj..."
```

### Public Endpoints
- `GET /` - Health check
- `GET /actuator/health` - Application health status

### Protected Endpoints
All other endpoints require authentication via JWT token in the Authorization header: 
```
Authorization: Bearer <jwt_token>
```

## ⚙️ Configuration

### Application Properties

```properties
# Application
spring.application.name=hospital
server.port=8081

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/hospital
spring.datasource. username=root
spring.datasource.password=root

# JPA/Hibernate
spring.jpa. hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Security Configuration

- **CORS:** Configured to allow all origins (modify for production)
- **Session Management:** Stateless
- **Password Encoding:** BCrypt
- **JWT Expiration:** 24 hours (86400000 ms)

⚠️ **Security Note:** Update the JWT secret key in `JwtUtil.java` before deploying to production! 

## 📁 Project Structure

```
hospitalBackend/
├── src/
│   ├── main/
│   │   ├── java/com/klu/hospital/
│   │   │   ├── HospitalApplication.java       # Main application
│   │   │   ├── controller/
│   │   │   │   └── AuthController.java        # Authentication endpoints
│   │   │   ├── entity/
│   │   │   │   └── User.java                  # User entity
│   │   │   ├── repository/
│   │   │   │   └── UserRepository.java        # Data access layer
│   │   │   ├── security/
│   │   │   │   ├── JwtUtil.java              # JWT utilities
│   │   │   │   ├── SecurityConfig.java       # Security configuration
│   │   │   │   └── UserDetailsServiceImpl.java
│   │   │   └── service/
│   │   │       └── UserService.java          # Business logic
│   │   └── resources/
│   │       └── application.properties        # Configuration
│   └── test/                                 # Test files
├── Dockerfile                                # Docker configuration
├── pom.xml                                   # Maven dependencies
└── README.md
```

## 🔐 Security

This application implements multiple security layers:

1. **Authentication:** JWT-based stateless authentication
2. **Password Security:** BCrypt encryption for stored passwords
3. **Authorization:** Role-based access control (RBAC)
4. **CORS:** Cross-Origin Resource Sharing configuration
5. **HTTPS Ready:** Can be configured with SSL/TLS certificates

### Best Practices

- Change the default JWT secret key
- Use environment variables for sensitive data
- Enable HTTPS in production
- Implement rate limiting for API endpoints
- Regular security audits and dependency updates

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Himanshu Kumar**
- GitHub: [@krHimanshu123](https://github.com/krHimanshu123)

## 🙏 Acknowledgments

- Spring Boot team for the excellent framework
- Spring Security for robust security features
- JWT. io for comprehensive JWT documentation

---

⭐ If you find this project useful, please consider giving it a star! 

## 📞 Support

For support, email your-email@example.com or open an issue in the repository. 
```
