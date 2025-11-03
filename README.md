# KSP Dashboard Backend API

Backend API untuk KSP Dashboard - Sistem Koleksi Data Multi-Kementerian & Lembaga.

## 🚀 Quick Start

### Prerequisites
- Java 21+
- Maven 3.6+

### Running the Application

```bash
# Build and run
.\mvnw.cmd spring-boot:run

# Or using Maven
mvn spring-boot:run

# Or build JAR and run
mvn clean package
java -jar target/ksp-0.0.1-SNAPSHOT.jar
```

Application akan berjalan di: **http://localhost:8080**

### Redis Setup

Redis diperlukan untuk caching. Pilih salah satu opsi:

**Opsi 1: Docker (Recommended)**
```bash
docker-compose up -d redis
```

**Opsi 2: Manual Install**
- Download Memurai: https://www.memurai.com/get-memurai
- Or use WSL/Redis Cloud

Lihat `REDIS-SETUP.md` untuk instruksi lengkap.

## 📖 Swagger UI Documentation

Swagger UI tersedia di:
- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **API Docs (JSON)**: http://localhost:8080/v3/api-docs
- **API Docs (YAML)**: http://localhost:8080/v3/api-docs.yaml

Di Swagger UI Anda dapat:
- Melihat semua endpoint API yang tersedia
- Melihat contoh request dan response
- Melakukan test API langsung dari browser
- Menggunakan JWT authentication dengan tombol "Authorize"

## 📋 API Endpoints

### Authentication
- `POST /api/auth/login` - Login dan mendapatkan JWT token
- `POST /api/auth/logout` - Logout

### Users (CRUD)
- `GET /api/users` - List semua users (pagination)
- `GET /api/users/{id}` - Detail user
- `POST /api/users` - Create user baru
- `PUT /api/users/{id}` - Update user
- `DELETE /api/users/{id}` - Delete user

### Health
- `GET /api/health` - Health check

## 🔐 Default Credentials

```
Username: admin
Password: admin123
```

## 📁 Project Structure

```
src/main/java/go/id/ksp/ksp/
├── config/              # Configuration classes
│   ├── SecurityConfig.java
│   └── DataInitializer.java
├── controller/          # REST controllers
│   ├── AuthController.java
│   ├── UserController.java
│   └── HealthController.java
├── dto/                 # Data Transfer Objects
│   ├── LoginRequest.java
│   ├── JwtResponse.java
│   ├── UserRequest.java
│   └── UserResponse.java
├── entity/              # JPA entities
│   ├── User.java
│   └── Role.java
├── exception/           # Exception handlers
│   ├── ResourceNotFoundException.java
│   └── GlobalExceptionHandler.java
├── repository/          # JPA repositories
│   ├── UserRepository.java
│   └── RoleRepository.java
├── security/            # Security utilities
│   └── JwtUtils.java
└── service/             # Business logic
    ├── AuthService.java
    ├── UserService.java
    ├── UserDetailsServiceImpl.java
    └── UserDetailsImpl.java
```

## 🔧 Configuration

### Database
Default: H2 in-memory database (untuk development)

Untuk menggunakan PostgreSQL, uncomment di `application.properties`:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/ksp_db
spring.datasource.username=postgres
spring.datasource.password=root
```

### JWT
```properties
app.jwt.secret=your-secret-key
app.jwt.expiration=86400000  # 24 hours in ms
```

## 🔑 Roles

Available roles:
- `ADMIN` - Full access
- `USER` - Standard user
- `ANALYST` - Data analyst
- `VIEWER` - Read-only access

## 📝 Example Usage

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}'
```

Response:
```json
{
  "token": "eyJhbGci...",
  "type": "Bearer",
  "id": 1,
  "username": "admin",
  "email": "admin@ksp.go.id",
  "fullName": "Administrator",
  "roles": ["ROLE_ADMIN", "ROLE_USER", "ROLE_ANALYST", "ROLE_VIEWER"]
}
```

### Create User
```bash
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "username": "newuser",
    "email": "user@example.com",
    "password": "password123",
    "fullName": "New User",
    "roles": ["USER"]
  }'
```

### Get All Users
```bash
curl -X GET http://localhost:8080/api/users?page=0&size=10 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## 🛠️ Technology Stack

- **Spring Boot 3.3.6**
- **Spring Security** - Authentication & Authorization
- **JWT (jjwt)** - Token-based auth
- **Spring Data JPA** - Data persistence
- **Spring Data Redis** - Caching layer
- **SpringDoc OpenAPI 3** - API documentation
- **H2 Database** - In-memory DB (dev)
- **PostgreSQL** - Production DB
- **Redis** - Cache & Session storage
- **Lombok** - Boilerplate reduction
- **Java 21**

## 📦 Dependencies

Key dependencies:
- spring-boot-starter-security
- spring-boot-starter-data-jpa
- spring-boot-starter-web
- spring-boot-starter-data-redis (Redis caching)
- springdoc-openapi-starter-webmvc-ui (Swagger UI)
- jjwt (JWT library)
- h2 or postgresql
- lombok
- commons-pool2 (Redis connection pooling)

## 🔍 Development Tips

1. **Swagger UI**: http://localhost:8080/swagger-ui.html
   - Interactive API documentation
   - Test endpoints directly from browser
   - JWT authentication supported

2. **H2 Console**: http://localhost:8080/h2-console
   - JDBC URL: jdbc:h2:mem:kspdb
   - Username: sa
   - Password: (empty)

3. Enable SQL logging:
   ```properties
   spring.jpa.show-sql=true
   ```

4. Default admin user is automatically created on first startup.

## 📖 Documentation

Complete documentation available in `/doc` folder.

## 🐛 Troubleshooting

- Port 8080 already in use: Change `server.port` in application.properties
- Database connection errors: Check database credentials
- JWT errors: Verify secret key configuration

"# oli-backend" 
