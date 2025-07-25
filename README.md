# Transaction Management System

## 📜 Overview


**Transaction Management System** is a microservices-based financial platform that manages users and their balances through secure, validated transactions.
The system supports **deposit**, **withdraw**, and **transfer** operations, with detailed tracking of each transaction.

This project was developed as part of a test assignment using best practices in Spring Boot and microservices architecture.

---

## 🏗️ Architecture
This project follows Microservices Architecture and consists of the following core services:

| Service                                                                   | Description                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [auth_service](https://github.com/Asadjon/auth_service.git)               | Manages user registration, login, and token validation.                                 |
| [balance_service](https://github.com/Asadjon/balance_service.git)         | Handles balance tracking and updates for each user.                                     |
| [transaction_service](https://github.com/Asadjon/transaction_service.git) | Records all user transactions and manages the logic of deposit, withdraw, and transfer. |
| [api_gateway](https://github.com/Asadjon/api_gateway.git)                 | Routes client requests to appropriate services and validates authorization tokens.      |

All services are containerized and orchestrated using **Docker Compose**.

---

## 🛠️ Tech Stack

| Layer            | Technologies                                                 |
|------------------|--------------------------------------------------------------|
| Backend          | Java 21, Spring Boot 3, Spring Cloud Gateway, Spring WebFlux |
| Authentication   | JWT (JSON Web Tokens)                                        |
| Database         | PostgreSQL                                                   |
| Communication    | REST API, WebClient (non-blocking)                           |
| Containerization | Docker, Docker Compose                                       |

---

## ⚙️ Setup Instructions

1. **Clone All Repositories**\
   Clone each microservice repository individually into a common directory:
    ```bash
    # Create a parent directory
    mkdir transaction-management-system
    cd transaction-management-system
    
    # Clone each service
    git clone https://github.com/Asadjon/api_gateway.git
    git clone https://github.com/Asadjon/auth_service.git
    git clone https://github.com/Asadjon/balance_service.git
    git clone https://github.com/Asadjon/transaction_service.git
    ```
2. **Environment Configuration**\
   Each service contains its own .env file inside its folder. These files are automatically loaded at runtime.
   Make sure to review or update these .env files as needed:
    * `auth_service/.env`
    * `balance_service/.env`
    * `transaction_service/.env`
    * `api_gateway/test.env`
3. **🔌 Create Shared Docker Network**
   Before running the services, you must create a shared Docker network manually:
   ```bash
   docker network create app-network
   ```
   > This command ensures all microservices can communicate with each other using the `app-network`.
   
   You only need to run this once before starting any of the services.

5. **🐳 Running Services Individually with Docker Compose**\
   Each service has its own `docker-compose.yml` file located inside its respective directory. Use the following commands to start each service individually:
    ```bash
    # Start Auth Service
    cd auth_service
    docker-compose up --build
    
    # Start Balance Service
    cd balance_service
    docker-compose up --build
    
    # Start Transaction Service
    cd transaction_service
    docker-compose up --build
    
    # Start API Gateway
    cd api_gateway
    docker-compose up --build
    ```
   
---

## FAQ
**Q:** _What architecture is used in this project?_ \
**A:** _This service is part of a microservices architecture. The API Gateway acts as the single entry point for clients and handles routing and security._

**Q:** _What type of authentication is used?_ \
**A:** _**JWT (JSON Web Token)** based authentication is implemented via the custom **AuthenticationFilter**, which communicates with the [auth_service](https://github.com/Asadjon/auth_service.git)._

**Q:** _What happens if the user tries to transfer more than their balance?_\
**A:** _The transaction will fail, and the response will contain a message like `"Amount must be greater than current balance"`._

**Q:** _What happens if the **JWT** token is invalid or expired?_\
**A:** _The request will be blocked at the **API Gateway** level with a `401 Unauthorized` response._

---

## 📌 Author's Note
> This project was completed as part of a test assignment during personal off-hours. While every effort was made to cover all requirements, feel free to reach out in case of questions or feedback.
