# Architecture Analysis

## Application Overview

| Attribute | Value |
|-----------|-------|
| **Name** | mono-lessons-kotlin-spring-mongo-master |
| **Type** | Multi-module Educational Demo Application |
| **Language** | Kotlin 1.2.10 |
| **Framework** | Spring Boot (2.0.0.M7 / 1.5.9.RELEASE) |
| **Build Tool** | Maven |
| **Database** | MongoDB (localhost:27017) |

## Architecture Pattern

**Pattern:** Modular Monolith with Layered Architecture per Module

Each module follows a simple 3-layer architecture:
- **Controller Layer** - REST endpoints
- **Repository Layer** - Data access (Spring Data MongoDB)
- **Model Layer** - Domain entities/documents

## Modules Overview

| Module | Port | Database | Purpose |
|--------|------|----------|---------|
| **mongo-kt** | 8080 | MongoKt | Standard blocking MongoDB access |
| **mongo-reactive-kt** | 8081 | MongoReactiveKt | Reactive MongoDB with Project Reactor |
| **mongo-grid-fs-kt** | 8082 | MongoGridFsKt | MongoDB GridFS file storage |

## Module 1: mongo-kt (Standard Blocking)

### Components
- **MongoKtApplication** - Spring Boot entry point
- **PersonController** - CRUD endpoints for Person entities
- **VehicleController** - CRUD endpoints for Vehicle entities
- **PersonRepository** - MongoRepository for Person
- **VehicleRepository** - MongoRepository for Vehicle
- **UserLoader** - CommandLineRunner for data initialization

### Package Structure
```
com.park.spring.mongo.kt.mongokt/
├── MongoKtApplication.kt
├── model/
│   └── User.kt (Person, Address, Vehicle, Repositories)
├── controller/
│   └── HomeController.kt (PersonController, VehicleController)
└── component/
    └── UserLoader.kt
```

## Module 2: mongo-reactive-kt (Reactive)

### Components
- **MongoReactiveKtApplication** - Spring Boot entry point
- **RaiderReactiveController** - Reactive endpoints returning Flux/Mono
- **RaiderController** - Blocking endpoints for comparison
- **RaiderReactiveRepository** - ReactiveMongoRepository
- **RaiderRepository** - Standard MongoRepository
- **UserLoader** - CommandLineRunner for data initialization

### Reactive Features
- **Flux<T>** - Multi-value reactive stream
- **Mono<T>** - Single-value reactive stream
- **Server-Sent Events (SSE)** - Real-time streaming endpoint

### Package Structure
```
com.park.mongo.reactive.kt.mongoreactivekt/
├── MongoReactiveKtApplication.kt
├── model/
│   └── User.kt (Raider, RaiderReactive, Repositories)
├── controller/
│   └── HomeController.kt (RaiderReactiveController, RaiderController)
└── component/
    └── UserLoader.kt
```

## Module 3: mongo-grid-fs-kt (File Storage)

### Components
- **MongoGridFsKtApplication** - Spring Boot entry point
- **FilesController** - File upload/download/list/delete endpoints
- **GridFsTemplate** - MongoDB GridFS operations

### Features
- File upload with metadata (owner, content-type)
- File download with proper content-type headers
- File listing and deletion
- Web UI (static HTML)

### Package Structure
```
com.park.spring.mongo.grid.fs.kt.mongogridfskt/
├── MongoGridFsKtApplication.kt
├── controller/
│   └── FilesController.kt
└── resources/
    ├── application.properties
    └── static/
        └── index.html
```

## Entry Points

| Module | Type | Endpoint Base |
|--------|------|---------------|
| mongo-kt | REST API | `/users`, `/vehicles` |
| mongo-reactive-kt | REST API | `/reactive`, `/raiders` |
| mongo-grid-fs-kt | REST API + Web UI | `/files`, `/` (index.html) |

## External Integrations

| Integration | Purpose | Configuration |
|-------------|---------|---------------|
| MongoDB | Document database | localhost:27017 |
| MongoDB GridFS | Large file storage | Via GridFsTemplate |

## Cross-Cutting Concerns

| Concern | Implementation |
|---------|----------------|
| Configuration | Spring Boot application.properties |
| Data Initialization | CommandLineRunner (UserLoader) |
| Serialization | Jackson JSON (Spring Boot default) |
| Reactive Streams | Project Reactor (Flux, Mono) |

## Technology Stack

### Dependencies (mongo-kt / mongo-reactive-kt)
- spring-boot-starter-data-mongodb / spring-boot-starter-data-mongodb-reactive
- spring-boot-starter-web / spring-boot-starter-webflux
- kotlin-stdlib-jre8
- kotlin-reflect

### Dependencies (mongo-grid-fs-kt)
- spring-boot-starter-data-mongodb
- spring-boot-starter-web
- kotlin-stdlib-jre8
- kotlin-reflect

### Build Plugins
- spring-boot-maven-plugin
- kotlin-maven-plugin with spring compiler plugin
