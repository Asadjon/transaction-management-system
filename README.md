# Transaction Management System
A microservices-based financial API for managing user balances and transactions.



## FAQ

### 🏗️ What architecture is this project based on?

This project is based on the **Microservices Architecture**. Each major feature is developed as an independent service:

- **auth_service** – Manages user registration, login, email verification, and role-based access.
- **balance_service** – Handles operations related to user balance.
- **transaction_service** – Manages transactions between users and tracks the transaction history.
- **api_gateway** – Routes and filters requests to corresponding services via Spring Cloud Gateway.

### 🔐 What kind of authentication is used in API requests?

The project uses **JWT (JSON Web Token)** based authentication. After logging in, users receive a token which must be included in every authenticated API request:

```http
Authorization: Bearer <your_token_here>
```
Requests to secured endpoints without a valid token will return a 401 Unauthorized error.

## Docker Services

Once the application is up using Docker Compose, the following services will be available:

| Service                | Port   | Description                          |
|------------------------|--------|--------------------------------------|
| API Gateway            | 8080   | Entry point for all API requests     |
| Auth Service           | 8081   | User authentication and registration |
| Balance Service        | 8082   | User balance operations              |
| Transaction Service    | 8083   | Transaction handling                 |
| User Database          | 5434   | Databases for User service           |
| Balance Database       | 5433   | Databases for Balance service        |
| Transaction Database   | 5435   | Databases for Transaction service    |

## 📦 Environment Variables

Each microservice contains its own .env file inside its working directory. These files are automatically loaded during Docker Compose runtime.
## Features

- User creation, update, and retrieval
- Transaction creation
- Transaction list filtering (all, by user, by date range)
- PostgreSQL database integration
- Mock data loading at startup
- Dockerized application


## Technologies

**Java 21**

**Spring Boot 3.5.3:**

**Spring Security**

**PostgreSQL**

**Docker & Docker Compose**

**JWT Authentication**


## Run Locally

Clone the project

```bash
  git clone https://github.com/Asadjon/transaction-management-system.git
```

Go to the project directory

```bash
  cd cd transaction-management-system
```

Install dependencies

```bash
  docker compose up --build
```


## API Reference

- #### Authentication

#### Register

```http
  POST http://localhost:8080/api/v1/auth/register

  body: 
    {
        "firstName": "first name",
        "lastName": "last name",
        "email": "your_email@example.com",
        "password": "****",
        "role": "USER"
    }
```

After the registration request, a confirmation request will be sent to your email address

#### Login

```http
  POST http://localhost:8080/api/v1/auth/login

  body: 
    {
        "email": "your_email@example.com",
        "password": "****"
    }
```

#### Resend confirmation

```http
  POST http://localhost:8080/api/v1/auth/resend-verification

  body: 
    {
        "email": "your_email@example.com"
    }
```

#### Forgot password

```http
  POST http://localhost:8080/api/v1/auth/forgot-password

  body: 
    {
        "email": "your_email@example.com"
    }
```

- #### User balance

#### Get by user id

```http
  GET http://localhost:8080/api/v1/balance/{userId}

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

| Parameter  | Type     | Location | Description                       |
|------------|----------|----------|-----------------------------------|
| `userId`   | `long`   | `path`   | **Required**                      |

#### Change balance

```http
  POST http://localhost:8080/api/v1/balance/change

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...

  body: 
    {
        "userId": {user id},
        "amount": amount,
        "type": "INCREASE or DECREASE"
    }
```

- #### Transaction

#### Get all

```http
  GET http://localhost:8080/api/v1/transaction/all

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

#### Get by user id

```http
  GET http://localhost:8080/api/v1/transaction/user/{userId}

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

| Parameter  | Type     | Location | Description                       |
|------------|----------|----------|-----------------------------------|
| `userId`   | `long`   | `path`   | **Required**                      |

#### Get date range

```http
  GET http://localhost:8080/api/v1/transaction/range

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

| Parameter  | Type          | Location | Description                       |
|------------|---------------|----------|-----------------------------------|
| `from`     | `LocalDate`   | `param`  | **Required**                      |
| `to`       | `LocalDate`   | `param`  | **Required**                      |

#### Transfer

```http
  POST http://localhost:8080/api/v1/transaction/transfer

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...

  body: 
    {
        "fromUserId": {from user id},
        "toUserId": {to user id},
        "amount": {amount}
    }
```

#### Withdraw

```http
  POST http://localhost:8080/api/v1/transaction/withdraw

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...

  body: 
    {
        "userId": {user id},
        "amount": {amount}
    }
```

#### Deposit

```http
  POST http://localhost:8080/api/v1/transaction/deposit

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...

  body: 
    {
        "userId": {user id},
        "amount": {amount}
    }
```

