# C4 Model Analysis

## Level 1: System Context

### System
**Kotlin Spring MongoDB Demo System**
- Multi-module educational application demonstrating MongoDB access patterns
- Written in Kotlin with Spring Boot framework

### Actors

| Actor | Type | Description |
|-------|------|-------------|
| Developer | Person | Runs and tests the demo applications |
| API Consumer | Person/System | Calls REST APIs to perform CRUD operations |
| Browser User | Person | Uses web UI for file management (GridFS module) |

### External Systems

| System | Description |
|--------|-------------|
| MongoDB Server | Document database running on localhost:27017 |

### Context Relationships

```
Developer ──> [uses] ──> Demo System
API Consumer ──> [calls API] ──> Demo System
Browser User ──> [uploads/downloads files] ──> Demo System
Demo System ──> [reads/writes data] ──> MongoDB Server
```

---

## Level 2: Container Diagram

### Containers

| Container | Technology | Port | Database | Description |
|-----------|------------|------|----------|-------------|
| **mongo-kt** | Kotlin, Spring Boot 2.0, Spring MVC | 8080 | MongoKt | Standard blocking MongoDB access |
| **mongo-reactive-kt** | Kotlin, Spring Boot 2.0, WebFlux | 8081 | MongoReactiveKt | Reactive MongoDB with Flux/Mono |
| **mongo-grid-fs-kt** | Kotlin, Spring Boot 1.5, Spring MVC | 8082 | MongoGridFsKt | GridFS file storage |
| **MongoDB** | MongoDB 3.x | 27017 | (all DBs) | Document database server |

### Container Relationships

```
API Consumer ──[HTTP/REST]──> mongo-kt
API Consumer ──[HTTP/REST]──> mongo-reactive-kt
Browser User ──[HTTP/HTML]──> mongo-grid-fs-kt

mongo-kt ──[MongoDB Protocol]──> MongoDB (MongoKt DB)
mongo-reactive-kt ──[MongoDB Reactive]──> MongoDB (MongoReactiveKt DB)
mongo-grid-fs-kt ──[GridFS Protocol]──> MongoDB (MongoGridFsKt DB)
```

---

## Level 3: Component Diagrams

### Container: mongo-kt

| Component | Type | Responsibility |
|-----------|------|----------------|
| MongoKtApplication | Spring Boot App | Application entry point |
| PersonController | REST Controller | Person CRUD endpoints |
| VehicleController | REST Controller | Vehicle CRUD endpoints |
| PersonRepository | Repository | Person data access |
| VehicleRepository | Repository | Vehicle data access |
| Person | Document | Person entity with embedded Address |
| Vehicle | Document | Vehicle entity |
| Address | Embedded | Embedded address object |
| UserLoader | Component | Data initialization |

#### Component Relationships
```
API Consumer ──> PersonController ──> PersonRepository ──> MongoDB
API Consumer ──> VehicleController ──> VehicleRepository ──> MongoDB
Application Startup ──> UserLoader ──> PersonRepository/VehicleRepository
```

---

### Container: mongo-reactive-kt

| Component | Type | Responsibility |
|-----------|------|----------------|
| MongoReactiveKtApplication | Spring Boot App | Application entry point |
| RaiderReactiveController | REST Controller | Reactive endpoints (Flux/Mono) |
| RaiderController | REST Controller | Blocking endpoints |
| RaiderReactiveRepository | Reactive Repository | Non-blocking data access |
| RaiderRepository | Repository | Blocking data access |
| Raider | Document | Raider entity |
| RaiderReactive | DTO | SSE response wrapper |
| UserLoader | Component | Data initialization |

#### Component Relationships
```
API Consumer ──[REST]──> RaiderReactiveController ──[Reactive]──> RaiderReactiveRepository ──> MongoDB
API Consumer ──[REST]──> RaiderController ──[Blocking]──> RaiderRepository ──> MongoDB
SSE Client ──[EventStream]──> RaiderReactiveController ──> Flux.interval + Repository
```

---

### Container: mongo-grid-fs-kt

| Component | Type | Responsibility |
|-----------|------|----------------|
| MongoGridFsKtApplication | Spring Boot App | Application entry point |
| FilesController | REST Controller | File CRUD endpoints |
| GridFsTemplate | Spring Component | GridFS operations |
| index.html | Static Resource | Web UI for file management |

#### Component Relationships
```
Browser ──[HTTP]──> index.html (static)
Browser ──[Fetch API]──> FilesController ──> GridFsTemplate ──> MongoDB GridFS
Upload ──> FilesController.addFile() ──> GridFsTemplate.store() ──> fs.files + fs.chunks
Download ──> FilesController.getFile() ──> GridFsTemplate.findOne() ──> ByteArray response
```

---

## Technology Choices

### Why Multiple Modules?

| Pattern | Module | Purpose |
|---------|--------|---------|
| Blocking I/O | mongo-kt | Traditional Spring Data MongoDB |
| Reactive I/O | mongo-reactive-kt | Non-blocking with Project Reactor |
| Binary Storage | mongo-grid-fs-kt | Large file handling with GridFS |

### Framework Versions

| Module | Spring Boot | Reason |
|--------|-------------|--------|
| mongo-kt | 2.0.0.M7 | Latest features |
| mongo-reactive-kt | 2.0.0.M7 | WebFlux support |
| mongo-grid-fs-kt | 1.5.9 | Stable GridFS support |

---

## Deployment View

```
┌─────────────────────────────────────────────────────────┐
│                    Developer Machine                     │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │  mongo-kt   │  │mongo-react- │  │mongo-grid-  │     │
│  │   :8080     │  │   ive-kt    │  │   fs-kt     │     │
│  │             │  │   :8081     │  │   :8082     │     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
│         │                │                │             │
│         └────────────────┼────────────────┘             │
│                          │                              │
│                          ▼                              │
│                  ┌───────────────┐                      │
│                  │    MongoDB    │                      │
│                  │    :27017     │                      │
│                  │               │                      │
│                  │ ┌───────────┐ │                      │
│                  │ │  MongoKt  │ │                      │
│                  │ ├───────────┤ │                      │
│                  │ │MongoReact │ │                      │
│                  │ │   iveKt   │ │                      │
│                  │ ├───────────┤ │                      │
│                  │ │MongoGrid  │ │                      │
│                  │ │   FsKt    │ │                      │
│                  │ └───────────┘ │                      │
│                  └───────────────┘                      │
│                                                         │
└─────────────────────────────────────────────────────────┘
```
