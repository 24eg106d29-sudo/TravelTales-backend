# TravelTales — Backend

Java 21, Spring Boot, Spring Data JPA, and MySQL REST API for TravelTales. Frontend source and deployment instructions are in [TravelTales-frontend](https://github.com/24eg106d29-sudo/TravelTales-frontend).

## Requirements

- Java 21
- Maven 3.9+
- MySQL 8

## Configure and run

Create a MySQL database and apply `database/schema.sql`. For an existing database, inspect and apply the appropriate scripts under `database/migrations/` after taking a backup. Copy `.env.example` to `.env` and fill in local values; never commit `.env`.

Required environment variables: `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `JWT_SECRET` (Base64, at least 32 random bytes). `CORS_ALLOWED_ORIGINS` should be set to the exact frontend origin. `ADMIN_EMAIL`, `ADMIN_NAME`, and `ADMIN_PASSWORD` optionally provision the initial admin account at startup.

```sh
mvn clean test
mvn clean package
mvn spring-boot:run
```

## Deployment

Connect this repository as a separate Railway service, using the repository root as its build context. Add a Railway MySQL service and configure the variables above in the backend service. Set `DB_URL` to the Railway MySQL JDBC URL with database name and `serverTimezone=UTC&characterEncoding=UTF-8`; set `DB_USERNAME` and `DB_PASSWORD` from the MySQL service. Set `CORS_ALLOWED_ORIGINS` to the frontend's public HTTPS origin. Configure the health check to `/actuator/health` only if actuator is enabled; otherwise use an available API health endpoint or add a platform TCP health check.

Database migrations are manual and must be backed up and reviewed before applying. Do not use Hibernate schema update against production data.

## Scope

This repository contains the REST API, database schema/migration scripts, and backend deployment files. It does not make real third-party travel reservations or process payments.
