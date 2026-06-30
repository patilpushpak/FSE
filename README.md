# TweetApp

A console-based tweet platform built with Spring Boot and an in-memory H2 database.

## Tech Stack

- Java 8
- Spring Boot 2.4.3
- Spring Data JPA (Hibernate)
- H2 (in-memory database)
- Lombok

## Getting Started

### Prerequisites

- Java 8+
- Maven (or use the included `mvnw` wrapper)

### Run the application

```bash
cd tweetapp
./mvnw spring-boot:run
```

The app starts an interactive CLI menu in the terminal.

### Build

```bash
./mvnw clean package -DskipTests
java -jar target/tweetapp-0.0.1-SNAPSHOT.jar
```

### Run tests

```bash
./mvnw test
```

## Features

| Menu Option | Description |
|---|---|
| Register | Create a new user account (userId is email) |
| Login | Authenticate and access user features |
| Forgot Password | Reset password by userId (no auth required) |
| Post a Tweet | Post a new tweet while logged in |
| View my tweets | List all tweets by the logged-in user |
| View all tweets | List every tweet in the system |
| View all users | List all registered users |
| Reset password | Change password while logged in |
| Logout | End the session |

## H2 Console

When the app is running, the H2 web console is available at:

```
http://localhost:8080/h2-console
JDBC URL : jdbc:h2:mem:testdb
Username : sa
Password : (leave blank)
```

## Project Structure

```
tweetapp/
├── src/main/java/com/tweetapp/
│   ├── TweetappApplication.java   # Entry point & CLI menu (CommandLineRunner)
│   ├── entity/                    # JPA entities: User, Tweet
│   ├── repository/                # Spring Data JPA repositories
│   ├── dao/                       # DAO interfaces + implementations
│   ├── service/                   # Service interfaces + implementations
│   └── exception/                 # Custom checked exceptions
└── src/main/resources/
    └── application.properties     # H2 datasource & JPA config
```
