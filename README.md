**Employee Management System**.

---

```markdown
# 🧑‍💼 Employee Management System (Dockerized)

This repository contains a full-stack **Employee Management System** composed of a Spring Boot backend, a modern frontend (e.g., React), and PostgreSQL for data persistence — all containerized using Docker.

---

## 📁 Project Structure

```

.
├── .vscode/
├── ems-backend/        # Spring Boot backend (port 8080)
├── ems-frontend/       # React frontend (default port 3000)
└── docker-compose.yml

````

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/andremugabo/DevOps-Dockerization.git
cd ems-dockerized
````

---

### 2. Environment Configuration

#### 📦 Backend - `ems-backend/src/main/resources/application.properties`

Update your backend config to use PostgreSQL container:

```properties
spring.application.name=ems-backend
spring.datasource.url=jdbc:postgresql://localhost:5432/ems
spring.datasource.username=postgres
spring.datasource.password=postgres


spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
```

---

### 3. Dockerfiles

#### 🐳 Backend - `ems-backend/Dockerfile`

```Dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

> 🔧 Make sure to build the JAR first using `mvn clean package -DskipTests`

---

#### 🐳 Frontend - `ems-frontend/Dockerfile`

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

> If using `Vite`, update `.env` in `ems-frontend` to point to backend:

```
VITE_API_URL=http://localhost:8080
```

---

### 4. 🧩 Docker Compose Setup

Create `docker-compose.yml` in the root:

```yaml
version: '3.9'

services:
  backend:
    build: ./ems-backend
    container_name: ems-backend
    ports:
      - "8080:8080"
    depends_on:
      - postgres
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://localhost:5432/ems
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: postgres

  frontend:
    build: ./ems-frontend
    container_name: ems-frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    environment:
      VITE_API_URL: http://localhost:8080

  postgres:
    image: postgres:17
    container_name: ems-postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: employees
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

---

### 5. 🏁 Run the Application

Make sure the backend JAR is built first:

```bash
cd ems-backend
./mvnw clean package -DskipTests
cd ..

docker-compose up --build
```

---

## 🌐 Access the Apps

* Frontend: [http://localhost:3000](http://localhost:3000)
* Backend API: [http://localhost:8080](http://localhost:8080)
* PostgreSQL: [localhost:5432](postgresql://postgres:postgres@localhost:5432/ems)

---

## 🛠 Tech Stack

* **Backend:** Java 17, Spring Boot, Hibernate
* **Frontend:** React + Vite 
* **Database:** PostgreSQL 17
* **DevOps:** Docker, Docker Compose

---

## 📄 License

MIT

---

## 👨‍💻 Author

[andremugabo](https://github.com/andremugabo)

```



