# Data Flow Analysis

## Overview

This project demonstrates three different data flow patterns for MongoDB access in Spring Boot with Kotlin.

---

## Flow 1: Standard Blocking Data Flow (mongo-kt)

### Person CRUD Operations

#### GET /users/all - List All Persons
```
Client Request
    │
    ▼
PersonController.getAllPerson()
    │
    ▼
PersonRepository.findAll()
    │
    ▼
MongoDB (MongoKt.Person)
    │
    ▼
List<Person> Response
```

#### GET /users/{name} - Get Person by Name
```
Client Request ──> Path Variable: name
    │
    ▼
PersonController.getPersonByName(name)
    │
    ▼
PersonRepository.findByName(name)
    │
    ▼
MongoDB Query: { name: {name} }
    │
    ▼
Person Response (with embedded Address, DBRef Vehicle)
```

#### PUT /users - Insert Person
```
Client Request ──> JSON Body: Person
    │
    ▼
PersonController.insertPerson(person)
    │
    ▼
PersonRepository.insert(person)
    │
    ▼
MongoDB Insert
    │
    ▼
Person Response (with generated ObjectId)
```

#### POST /users - Save/Update Person
```
Client Request ──> JSON Body: Person
    │
    ▼
PersonController.savePerson(person)
    │
    ▼
PersonRepository.save(person)
    │
    ▼
MongoDB Upsert (insert if new, update if exists)
    │
    ▼
Person Response
```

#### DELETE /users/{id} - Delete Person
```
Client Request ──> Path Variable: id
    │
    ▼
PersonController.deletePerson(id)
    │
    ▼
PersonRepository.delete(id)
    │
    ▼
MongoDB Delete
    │
    ▼
Void Response
```

---

## Flow 2: Reactive Data Flow (mongo-reactive-kt)

### Reactive CRUD Operations

#### GET /reactive/all - List All Raiders (Reactive)
```
Client Request
    │
    ▼
RaiderReactiveController.getAllRaider()
    │
    ▼
RaiderReactiveRepository.findAll()
    │
    ▼
MongoDB Reactive Driver (Non-blocking)
    │
    ▼
Flux<Raider> ──> Streamed Response (JSON Array)
```

#### GET /reactive/name/{name} - Get Raiders by Name (Reactive)
```
Client Request ──> Path Variable: name
    │
    ▼
RaiderReactiveController.getRaiderByName(name)
    │
    ▼
RaiderReactiveRepository.findByName(name)
    │
    ▼
MongoDB Reactive Query
    │
    ▼
Flux<Raider>? ──> Streamed Response
```

#### GET /reactive/id/{id} - Get Raider by ID (Reactive)
```
Client Request ──> Path Variable: id (ObjectId)
    │
    ▼
RaiderReactiveController.getRaiderById(id)
    │
    ▼
RaiderReactiveRepository.findById(ObjectId(id))
    │
    ▼
MongoDB Reactive Query
    │
    ▼
Mono<Raider> ──> Single Response
```

### Server-Sent Events Flow

#### GET /reactive/action/{id}/{sec} - SSE Streaming
```
Client Request (Accept: text/event-stream)
    │
    ▼
RaiderReactiveController.getAction(id, sec)
    │
    ├──> Flux.interval(Duration.ofSeconds(2))
    │         │
    │         ▼ (Every 2 seconds)
    │
    ├──> RaiderReactiveRepository.findById(ObjectId(id))
    │         │
    │         ▼
    │    Mono<Raider>
    │         │
    │         ▼
    └──> RaiderReactive(raider, Date())
              │
              ▼
         SSE Event Stream
              │
              ▼
         Client receives events every 2 seconds
```

### POST /reactive/post - Save Raider (Reactive)
```
Client Request ──> JSON Body: Raider
    │
    ▼
RaiderReactiveController.saveRaider(raider)
    │
    ▼
RaiderReactiveRepository.save(raider)
    │
    ▼
MongoDB Reactive Insert
    │
    ▼
Mono<Raider> ──> Single Response
```

---

## Flow 3: GridFS File Flow (mongo-grid-fs-kt)

### File Upload Flow

#### POST /files - Upload File
```
Client (Browser/API)
    │
    ▼
Multipart Form Data (file)
    │
    ▼
FilesController.addFile(file)
    │
    ▼
GridFsTemplate.store(
    inputStream,
    filename,
    contentType,
    metadata { Owner: "Park" }
)
    │
    ├──> fs.files (metadata document)
    │
    └──> fs.chunks (binary chunks)
    │
    ▼
JavaScript Redirect Response
```

### File Download Flow

#### GET /files/{name} - Download File
```
Client Request ──> Path Variable: name (with extension)
    │
    ▼
FilesController.getFile(name)
    │
    ▼
GridFsTemplate.findOne(Query.query(
    GridFsCriteria.whereFilename().is(name)
))
    │
    ▼
GridFSDBFile
    │
    ▼
HttpHeaders (Content-Type, Content-Length)
    │
    ▼
ByteArray Response
```

### File List Flow

#### GET /files - List All Files
```
Client Request
    │
    ▼
FilesController.getAllFile()
    │
    ▼
GridFsTemplate.find(Query.query(
    GridFsCriteria.where(null)
))
    │
    ▼
List<GridFSDBFile>
    │
    ▼
List<String> (filenames)
```

### File Delete Flow

#### DELETE /files/delete/{name} - Delete File
```
Client Request ──> Path Variable: name
    │
    ▼
FilesController.deleteFile(name)
    │
    ▼
GridFsTemplate.delete(Query.query(
    GridFsCriteria.whereFilename().is(name)
))
    │
    ▼
"Delete Success" Response
```

---

## Web UI Flow (mongo-grid-fs-kt)

```
Browser
    │
    ▼
GET / ──> index.html (Static)
    │
    ▼
JavaScript fetch("/files")
    │
    ▼
Display file list with download links
    │
    ├──> User clicks file ──> GET /files/{name} ──> Download
    │
    └──> User uploads file ──> POST /files ──> Refresh page
```

---

## Data Transformation Points

| Point | Transformation |
|-------|----------------|
| Controller Input | JSON → Kotlin Object (Jackson) |
| Controller Output | Kotlin Object → JSON (Jackson) |
| Reactive Stream | Flux/Mono → JSON Array/Object |
| SSE Stream | RaiderReactive → text/event-stream |
| File Upload | MultipartFile → GridFS chunks |
| File Download | GridFS chunks → ByteArray |
| @DBRef Resolution | ObjectId → Referenced Document |
