# Sequence Diagram Analysis

## Overview

This document describes key interaction sequences across the three modules.

---

## Sequence 1: Standard CRUD - Get Person by Name

**Module:** mongo-kt
**Endpoint:** GET /users/{name}

### Participants
1. Client (Browser/API Consumer)
2. PersonController
3. PersonRepository
4. MongoDB

### Sequence
```
1. Client sends GET /users/John
2. PersonController receives request
3. PersonController extracts path variable "John"
4. PersonController calls personRepository.findByName("John")
5. PersonRepository executes MongoDB query { name: "John" }
6. MongoDB returns Person document (with embedded Address)
7. If Person has @DBRef vehicle, Spring Data resolves reference
8. PersonRepository returns Person object
9. PersonController returns Person as JSON
10. Client receives JSON response
```

### Notes
- @DBRef resolution happens automatically by Spring Data
- Address is embedded, no separate query needed

---

## Sequence 2: Reactive Get All - Flux Stream

**Module:** mongo-reactive-kt
**Endpoint:** GET /reactive/all

### Participants
1. Client
2. RaiderReactiveController
3. RaiderReactiveRepository
4. ReactiveMongoTemplate
5. MongoDB

### Sequence
```
1. Client sends GET /reactive/all
2. RaiderReactiveController.getAllRaider() called
3. Controller calls raiderReactiveRepository.findAll()
4. ReactiveMongoRepository returns Flux<Raider>
5. Flux subscription begins (lazy evaluation)
6. MongoDB cursor opens (non-blocking)
7. Documents streamed as they arrive
8. Each Raider serialized to JSON
9. Response streamed to Client as JSON array
10. Connection closes when Flux completes
```

### Notes
- Non-blocking I/O throughout
- Backpressure supported by Project Reactor
- No thread blocking during database access

---

## Sequence 3: Server-Sent Events Stream

**Module:** mongo-reactive-kt
**Endpoint:** GET /reactive/action/{id}/{sec}

### Participants
1. Client (EventSource)
2. RaiderReactiveController
3. Flux.interval
4. RaiderReactiveRepository
5. MongoDB

### Sequence
```
1. Client opens SSE connection: GET /reactive/action/abc123/5
2. Controller creates Flux.interval(Duration.ofSeconds(2))
3. SSE connection established (text/event-stream)

   [Every 2 seconds - repeated until client disconnects]
   4. Flux.interval emits tick
   5. flatMap triggers raiderReactiveRepository.findById(ObjectId(id))
   6. MongoDB query executed (non-blocking)
   7. Mono<Raider> returned
   8. map creates RaiderReactive(raider, Date())
   9. RaiderReactive serialized to SSE event
   10. Event pushed to Client

11. After {sec} events, take(sec) completes Flux
12. Connection closes
```

### Notes
- Uses TEXT_EVENT_STREAM_VALUE media type
- Client uses EventSource API or fetch with streaming
- Connection kept alive during streaming

---

## Sequence 4: File Upload with GridFS

**Module:** mongo-grid-fs-kt
**Endpoint:** POST /files

### Participants
1. Client (Browser)
2. FilesController
3. GridFsTemplate
4. MongoDB GridFS (fs.files, fs.chunks)

### Sequence
```
1. Client submits multipart form with file
2. Spring MVC parses MultipartFile
3. FilesController.addFile(file) called
4. Controller creates metadata DBObject
   - Owner: "Park"
   - content_type: file.contentType
5. Controller calls gridFsTemplate.store(
     file.inputStream,
     file.originalFilename,
     file.contentType,
     metadata
   )
6. GridFsTemplate creates fs.files document
   - _id: ObjectId
   - filename: originalFilename
   - contentType: MIME type
   - uploadDate: now
   - metadata: { Owner, content_type }
   - length: file size
   - chunkSize: 255KB (default)
7. GridFsTemplate splits file into chunks
8. Each chunk stored in fs.chunks
   - files_id: reference to fs.files._id
   - n: chunk number
   - data: BinData
9. GridFsTemplate returns ObjectId
10. Controller returns JavaScript redirect
11. Browser executes redirect to refresh page
```

---

## Sequence 5: File Download with GridFS

**Module:** mongo-grid-fs-kt
**Endpoint:** GET /files/{name}

### Participants
1. Client (Browser)
2. FilesController
3. GridFsTemplate
4. GridFSDBFile
5. MongoDB GridFS

### Sequence
```
1. Client sends GET /files/document.pdf
2. FilesController.getFile("document.pdf") called
3. Controller calls gridFsTemplate.findOne(
     Query.query(GridFsCriteria.whereFilename().is(name))
   )
4. GridFsTemplate queries fs.files for filename
5. MongoDB returns fs.files document
6. GridFSDBFile object created
7. Controller checks if file exists (null check)
8. If null: return 404 "Not Found"
9. Controller extracts content type from file
10. Controller creates HttpHeaders
    - Content-Disposition: inline
    - Content-Type: from file
    - Content-Length: file.length
11. Controller reads file.inputStream to ByteArray
12. GridFS retrieves chunks from fs.chunks
13. Chunks reassembled into complete file
14. HttpEntity created with headers and body
15. Client receives file with proper headers
16. Browser displays or downloads based on content-type
```

---

## Sequence 6: Insert with @DBRef Resolution

**Module:** mongo-kt
**Endpoint:** PUT /users (with Vehicle reference)

### Participants
1. Client
2. PersonController
3. VehicleRepository
4. PersonRepository
5. MongoDB

### Sequence
```
1. Client sends PUT /users with JSON:
   {
     "name": "John",
     "age": 30,
     "address": { "Country": "USA", "city": "NYC" },
     "vehicle": { "id": "v1" }
   }
2. PersonController.insertPerson(person) called
3. Jackson deserializes JSON to Person object
4. Person.vehicle contains Vehicle with only ID
5. PersonRepository.insert(person) called
6. Spring Data stores Person document:
   {
     "_id": ObjectId(...),
     "name": "John",
     "age": 30,
     "address": { "Country": "USA", "city": "NYC" },
     "vehicle": { "$ref": "vehicle", "$id": "v1" }
   }
7. @DBRef stored as reference, not embedded
8. MongoDB confirms insert
9. PersonRepository returns saved Person
10. Controller returns Person as JSON
```

### Notes
- Vehicle must exist for @DBRef to resolve on read
- On read, Spring Data automatically fetches referenced Vehicle
