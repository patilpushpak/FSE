# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`tweetapp` is a Spring Boot 2.4.3 / Java 8 console application (CLI) — not a REST API. It implements a tweet platform driven entirely through `System.in` via a `CommandLineRunner`. The H2 in-memory database is used; schema is auto-managed by Hibernate (`ddl-auto=update`).

## Commands

All commands run from the `tweetapp/` directory.

```bash
# Build (skip tests)
./mvnw clean package -DskipTests

# Run
./mvnw spring-boot:run

# Run all tests
./mvnw test

# Run a single test class
./mvnw test -Dtest=TweetappApplicationTests

# Build fat JAR and run it
./mvnw package -DskipTests && java -jar target/tweetapp-0.0.1-SNAPSHOT.jar
```

H2 console is available at `http://localhost:8080/h2-console` when the app is running (JDBC URL: `jdbc:h2:mem:testdb`, user: `sa`, no password).

## Architecture

The layered architecture follows: `CommandLineRunner (main)` → `Service` → `DAO` → `Repository` → H2.

```
TweetappApplication      — entry point; all user interaction via Scanner loop
  ├── UserService / UserServiceImpl
  │     └── UserDao / UserDaoImpl  →  UserRepository (JpaRepository<User, String>)
  └── TweetService / TweetServiceImpl
        └── TweetDao / TweetDaoImpl  →  TweetRepository (JpaRepository<Tweet, Long>)
```

**Entities:**
- `User` — PK is `userId` (a String, email-style). Holds `name`, `password` (plaintext), `isLoggedIn` flag, and a `@OneToMany` list of tweets.
- `Tweet` — auto-generated `Long` PK. Holds a `@ManyToOne` reference to `User` (column `user_id`) and `tweetDescription`.

**Key design notes:**
- `User.userId` is the natural key (email) rather than a generated ID — `UserRepository` uses `JpaRepository<User, String>`.
- Login state (`isLoggedIn`) is persisted to the database by calling `userService.registerUser(user)` again (reuses the save path for updates).
- Password reset (option 3 from main menu) does **not** require the current password; it is an unauthenticated reset by userId.
- The `IncorrectPasswordException` and `UserNotFoundException` are checked exceptions but are thrown from `CommandLineRunner.run()` and will terminate the app if uncaught.
- There are no REST controllers — `spring-boot-starter-web` is included but unused beyond enabling the H2 console endpoint.
