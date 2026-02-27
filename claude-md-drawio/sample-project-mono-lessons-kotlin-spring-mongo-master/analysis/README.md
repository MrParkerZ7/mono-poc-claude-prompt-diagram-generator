# Project Analysis: mono-lessons-kotlin-spring-mongo-master

## Project Overview

| Attribute | Value |
|-----------|-------|
| **Project Name** | mono-lessons-kotlin-spring-mongo-master |
| **Project Type** | Multi-module Educational Demo Application |
| **Primary Language** | Kotlin 1.2.10 |
| **Framework** | Spring Boot (2.0.0.M7 / 1.5.9.RELEASE) |
| **Database** | MongoDB 3.x |
| **Build Tool** | Maven (Multi-module) |
| **Architecture** | Modular Monolith with Layered Architecture |

---

## Technology Stack

### Core Technologies

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| Language | Kotlin | 1.2.10 | Primary programming language |
| Framework | Spring Boot | 2.0.0.M7 / 1.5.9 | Application framework |
| Database | MongoDB | 3.x | Document database |
| Reactive | Project Reactor | (via WebFlux) | Non-blocking streams |
| Build | Maven | 3.x | Dependency management |

### Spring Boot Starters

| Starter | Module | Purpose |
|---------|--------|---------|
| spring-boot-starter-web | mongo-kt, mongo-grid-fs-kt | REST API (blocking) |
| spring-boot-starter-webflux | mongo-reactive-kt | REST API (reactive) |
| spring-boot-starter-data-mongodb | mongo-kt, mongo-grid-fs-kt | MongoDB access (blocking) |
| spring-boot-starter-data-mongodb-reactive | mongo-reactive-kt | MongoDB access (reactive) |

### Key Libraries

| Library | Purpose |
|---------|---------|
| kotlin-stdlib-jre8 | Kotlin standard library |
| kotlin-reflect | Kotlin reflection support |
| jackson-module-kotlin | JSON serialization for Kotlin |
| reactor-core | Reactive streams (Flux, Mono) |

---

## Framework Patterns Demonstrated

### MongoDB Access Patterns

| Pattern | Module | Repository Type | Return Type |
|---------|--------|-----------------|-------------|
| **Blocking I/O** | mongo-kt | MongoRepository | `List<T>`, `T` |
| **Reactive I/O** | mongo-reactive-kt | ReactiveMongoRepository | `Flux<T>`, `Mono<T>` |
| **GridFS (Binary)** | mongo-grid-fs-kt | GridFsTemplate | `ByteArray`, `InputStream` |

### Spring Patterns

| Pattern | Example | Module |
|---------|---------|--------|
| REST Controller | `@RestController`, `@GetMapping` | All |
| Repository Pattern | `MongoRepository<T, ID>` | mongo-kt |
| Reactive Repository | `ReactiveMongoRepository<T, ID>` | mongo-reactive-kt |
| CommandLineRunner | Data initialization on startup | mongo-kt, mongo-reactive-kt |
| Server-Sent Events | `MediaType.TEXT_EVENT_STREAM_VALUE` | mongo-reactive-kt |

---

## Business Target

| Aspect | Description |
|--------|-------------|
| **Purpose** | Educational demonstration of MongoDB integration patterns |
| **Domain** | Generic CRUD operations (Person, Vehicle, Raider entities) |
| **Use Case** | Learning Spring Boot + Kotlin + MongoDB combinations |
| **Complexity** | Simple domain model, focus on infrastructure patterns |

### Learning Objectives

1. **Standard MongoDB Access** - Traditional blocking repository pattern
2. **Reactive MongoDB Access** - Non-blocking with Project Reactor
3. **Server-Sent Events (SSE)** - Real-time streaming over HTTP
4. **GridFS File Storage** - Large binary file handling in MongoDB
5. **Multi-module Maven** - Project organization strategies

---

## Client Target

| Client Type | Interface | Module | Port |
|-------------|-----------|--------|------|
| **REST API Client** | JSON over HTTP | mongo-kt | 8080 |
| **REST API Client** | JSON over HTTP | mongo-reactive-kt | 8081 |
| **SSE Client** | EventSource (Browser/JS) | mongo-reactive-kt | 8081 |
| **Browser User** | HTML Web UI | mongo-grid-fs-kt | 8082 |
| **File Upload Client** | Multipart/form-data | mongo-grid-fs-kt | 8082 |

### Client Examples

#### REST API Client (curl)
```bash
# Get all persons (blocking)
curl http://localhost:8080/users/all

# Get all raiders (reactive)
curl http://localhost:8081/reactive/all

# Upload file (GridFS)
curl -X POST http://localhost:8082/files -F "file=@document.pdf"
```

#### SSE Client (JavaScript)
```javascript
const eventSource = new EventSource(
  'http://localhost:8081/reactive/action/abc123/10'
);
eventSource.onmessage = (e) => {
  const data = JSON.parse(e.data);
  console.log(data.raider.name, data.date);
};
```

---

## Module Summary

| Module | Port | Database | Pattern | Entities |
|--------|------|----------|---------|----------|
| **mongo-kt** | 8080 | MongoKt | Blocking | Person, Vehicle, Address |
| **mongo-reactive-kt** | 8081 | MongoReactiveKt | Reactive + SSE | Raider |
| **mongo-grid-fs-kt** | 8082 | MongoGridFsKt | GridFS | fs.files, fs.chunks |

---

## Analysis Documents

| Document | Description |
|----------|-------------|
| [architecture-analysis.md](./architecture-analysis.md) | System architecture, modules, layers, components |
| [data-models-analysis.md](./data-models-analysis.md) | Entity definitions, MongoDB documents, relationships |
| [data-flow-analysis.md](./data-flow-analysis.md) | Request flows, blocking vs reactive patterns |
| [sequence-analysis.md](./sequence-analysis.md) | Sequence diagrams for key operations |
| [c4-model-analysis.md](./c4-model-analysis.md) | C4 Context and Container level analysis |
| [api-contract-analysis.md](./api-contract-analysis.md) | REST API endpoints, schemas, examples |

---

## Diagram Artifacts

| Diagram | Description |
|---------|-------------|
| [architecture-overview.drawio](../diagrams/architecture-overview.drawio) | High-level system architecture |
| [entity-erd.drawio](../diagrams/entity-erd.drawio) | MongoDB document relationships |
| [data-flow.drawio](../diagrams/data-flow.drawio) | Data flow through the system |
| [sequence-reactive-sse.drawio](../diagrams/sequence-reactive-sse.drawio) | SSE streaming sequence |
| [c4-1-context.drawio](../diagrams/c4-1-context.drawio) | C4 System Context diagram |
| [c4-2-container.drawio](../diagrams/c4-2-container.drawio) | C4 Container diagram |

---

## Quick Start

### Prerequisites
- JDK 8+
- Maven 3.x
- MongoDB 3.x running on localhost:27017

### Run Each Module

```bash
# Module 1: Blocking MongoDB
cd mongo-kt
mvn spring-boot:run
# Access: http://localhost:8080/users/all

# Module 2: Reactive MongoDB
cd mongo-reactive-kt
mvn spring-boot:run
# Access: http://localhost:8081/reactive/all

# Module 3: GridFS File Storage
cd mongo-grid-fs-kt
mvn spring-boot:run
# Access: http://localhost:8082/ (Web UI)
```

---

## Sample Code Snippets

### Blocking Repository (mongo-kt)
```kotlin
interface PersonRepository : MongoRepository<Person, ObjectId> {
    fun findByName(name: String): Person
}

@RestController
@RequestMapping("/users")
class PersonController(val personRepository: PersonRepository) {
    @GetMapping("/all")
    fun getAll(): List<Person> = personRepository.findAll()
}
```

### Reactive Repository (mongo-reactive-kt)
```kotlin
interface RaiderReactiveRepository : ReactiveMongoRepository<Raider, ObjectId> {
    fun findByName(name: String): Flux<Raider>
}

@RestController
@RequestMapping("/reactive")
class RaiderReactiveController(val raiderReactiveRepository: RaiderReactiveRepository) {
    @GetMapping("/all")
    fun getAll(): Flux<Raider> = raiderReactiveRepository.findAll()
}
```

### SSE Streaming (mongo-reactive-kt)
```kotlin
@GetMapping("/action/{id}/{sec}", produces = [MediaType.TEXT_EVENT_STREAM_VALUE])
fun action(@PathVariable id: String, @PathVariable sec: Int): Flux<RaiderReactive> {
    return Flux.interval(Duration.ofSeconds(2))
        .take(sec.toLong())
        .flatMap { raiderReactiveRepository.findById(ObjectId(id)) }
        .map { RaiderReactive(it, Date()) }
}
```

### GridFS File Operations (mongo-grid-fs-kt)
```kotlin
@PostMapping
fun addFile(@RequestParam file: MultipartFile): String {
    val metadata = DBObject()
    metadata.put("owner", "park")
    metadata.put("content-type", file.contentType)
    gridFsTemplate.store(file.inputStream, file.originalFilename, metadata)
    return "<script>window.location='/';</script>"
}
```

---

## Configuration

### mongo-kt (application.properties)
```properties
server.port=8080
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=MongoKt
```

### mongo-reactive-kt (application.properties)
```properties
server.port=8081
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=MongoReactiveKt
```

### mongo-grid-fs-kt (application.properties)
```properties
server.port=8082
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=MongoGridFsKt
spring.http.multipart.max-file-size=100MB
spring.http.multipart.max-request-size=100MB
```

---

## Key Takeaways

| Topic | Lesson |
|-------|--------|
| **Blocking vs Reactive** | Use blocking for simple CRUD, reactive for streaming/high-concurrency |
| **Repository Choice** | `MongoRepository` for blocking, `ReactiveMongoRepository` for reactive |
| **SSE Streaming** | Use `Flux.interval()` + `TEXT_EVENT_STREAM_VALUE` for real-time updates |
| **GridFS** | Use `GridFsTemplate` for files > 16MB, stores in fs.files + fs.chunks |
| **Multi-module** | Separate concerns by MongoDB access pattern, each with own database |
