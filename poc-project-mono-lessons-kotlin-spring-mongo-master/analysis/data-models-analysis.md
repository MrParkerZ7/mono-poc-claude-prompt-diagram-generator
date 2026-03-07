# Data Models / ERD Analysis

## Database Overview

| Attribute | Value |
|-----------|-------|
| **Database Type** | MongoDB (Document Database) |
| **Access Library** | Spring Data MongoDB |
| **Connection** | localhost:27017 |

## Databases

| Database | Module | Purpose |
|----------|--------|---------|
| MongoKt | mongo-kt | Standard blocking access demo |
| MongoReactiveKt | mongo-reactive-kt | Reactive access demo |
| MongoGridFsKt | mongo-grid-fs-kt | GridFS file storage demo |

---

## Module 1: mongo-kt Entities

### Person (Document)

| Field | Type | Annotations | Description |
|-------|------|-------------|-------------|
| id | ObjectId? | @Id, @Indexed | Primary identifier |
| name | String? | @Indexed | Person's name |
| age | Int? | - | Person's age |
| address | Address? | - | Embedded address object |
| vehicle | Vehicle? | @DBRef | Reference to Vehicle document |

### Address (Embedded)

| Field | Type | Description |
|-------|------|-------------|
| Country | String | Country name |
| city | String | City name |

### Vehicle (Document)

| Field | Type | Annotations | Description |
|-------|------|-------------|-------------|
| id | String? | @Id, @Indexed | Primary identifier |
| type | String? | - | Vehicle type (VehicleType enum value) |
| brand | String? | - | Vehicle brand (VehicleBrand enum value) |

### Relationships

```
Person (1) ──@DBRef──> (1) Vehicle
Person (1) ──embedded──> (1) Address
```

---

## Module 2: mongo-reactive-kt Entities

### Raider (Document)

| Field | Type | Annotations | Description |
|-------|------|-------------|-------------|
| id | ObjectId | @Id (auto-generated) | Primary identifier |
| name | String? | @Indexed | Raider's name |
| score | Int | - | Raider's score |

### RaiderReactive (DTO)

| Field | Type | Description |
|-------|------|-------------|
| raider | Optional<Raider> | Wrapped raider entity |
| date | Date | Timestamp of response |

---

## Module 3: mongo-grid-fs-kt Files

### GridFS Collections

| Collection | Purpose |
|------------|---------|
| fs.files | File metadata (filename, contentType, uploadDate, metadata) |
| fs.chunks | File binary data in chunks |

### File Metadata

| Field | Type | Description |
|-------|------|-------------|
| filename | String | Original filename |
| contentType | String | MIME type |
| uploadDate | Date | Upload timestamp |
| metadata.Owner | String | File owner (hardcoded "Park") |
| metadata.content_type | String | MIME type (duplicate) |

---

## Repository Interfaces

### mongo-kt Repositories

```kotlin
interface PersonRepository : MongoRepository<Person, ObjectId> {
    fun findByName(name: String?): Person
}

interface VehicleRepository : MongoRepository<Vehicle, String>
```

### mongo-reactive-kt Repositories

```kotlin
interface RaiderReactiveRepository : ReactiveMongoRepository<Raider, ObjectId> {
    fun findByName(name: String): Flux<Raider>?
    fun findById(id: ObjectId?): Mono<Raider>
}

interface RaiderRepository : MongoRepository<Raider, ObjectId> {
    fun findByName(name: String): List<Raider>?
    fun deleteByName(name: String)
}
```

---

## Indexes

| Collection | Field | Type | Purpose |
|------------|-------|------|---------|
| Person | id | Indexed | Primary key lookup |
| Person | name | Indexed | Name search |
| Vehicle | id | Indexed | Primary key lookup |
| Raider | name | Indexed | Name search |

---

## Data Type Mappings

| Kotlin Type | MongoDB Type |
|-------------|--------------|
| ObjectId | ObjectId |
| String | String |
| Int | NumberInt |
| Date | ISODate |
| Embedded Object | Embedded Document |
| @DBRef | DBRef (reference) |

---

## Entity Groupings

### Core Entities
- **Person** - Main entity in mongo-kt
- **Raider** - Main entity in mongo-reactive-kt

### Reference Entities
- **Vehicle** - Referenced by Person via @DBRef

### Embedded Objects
- **Address** - Embedded in Person

### DTOs
- **RaiderReactive** - Response wrapper for SSE streaming
