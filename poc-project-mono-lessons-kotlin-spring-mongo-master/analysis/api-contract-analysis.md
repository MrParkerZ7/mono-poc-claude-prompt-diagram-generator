# API Contract Analysis

## Overview

| Attribute | Value |
|-----------|-------|
| API Type | REST |
| Protocol | HTTP |
| Data Format | JSON |
| Versioning | None (demo application) |

---

## Module 1: mongo-kt API

**Base URL:** `http://localhost:8080`

### Person Endpoints

| Method | Endpoint | Description | Request | Response |
|--------|----------|-------------|---------|----------|
| GET | /users/all | List all persons | - | `List<Person>` |
| GET | /users/{name} | Get person by name | Path: name | `Person` |
| PUT | /users | Insert new person | Body: Person | `Person` |
| POST | /users | Save/update person | Body: Person | `Person` |
| DELETE | /users/{id} | Delete person by ID | Path: id (ObjectId) | void |

### Vehicle Endpoints

| Method | Endpoint | Description | Request | Response |
|--------|----------|-------------|---------|----------|
| GET | /vehicles/all | List all vehicles | - | `List<Vehicle>` |
| GET | /vehicles/{id} | Get vehicle by ID | Path: id | `Vehicle` |

### Schemas

#### Person
```json
{
  "id": "ObjectId (string)",
  "name": "string",
  "age": "integer",
  "address": {
    "Country": "string",
    "city": "string"
  },
  "vehicle": {
    "id": "string",
    "type": "string",
    "brand": "string"
  }
}
```

#### Vehicle
```json
{
  "id": "string",
  "type": "string (VehicleType)",
  "brand": "string (VehicleBrand)"
}
```

---

## Module 2: mongo-reactive-kt API

**Base URL:** `http://localhost:8081`

### Reactive Endpoints

| Method | Endpoint | Description | Request | Response |
|--------|----------|-------------|---------|----------|
| GET | /reactive/all | List all raiders (reactive) | - | `Flux<Raider>` |
| GET | /reactive/name/{name} | Get raiders by name | Path: name | `Flux<Raider>` |
| GET | /reactive/id/{id} | Get raider by ID | Path: id (ObjectId) | `Mono<Raider>` |
| GET | /reactive/action/{id}/{sec} | SSE stream | Path: id, sec | `Flux<RaiderReactive>` (SSE) |
| POST | /reactive/post | Save raider | Body: Raider | `Mono<Raider>` |

### Blocking Endpoints

| Method | Endpoint | Description | Request | Response |
|--------|----------|-------------|---------|----------|
| GET | /raiders/all | List all raiders | - | `List<Raider>` |
| GET | /raiders/name/{name} | Get raiders by name | Path: name | `List<Raider>` |
| GET | /raiders/id/{id} | Get raider by ID | Path: id | `Raider` |
| PUT | /raiders/put | Insert new raider | Body: Raider | `Raider` |
| POST | /raiders/post | Save/update raider | Body: Raider | `Raider` |
| DELETE | /raiders/name/{name} | Delete by name | Path: name | void |
| DELETE | /raiders/id/{id} | Delete by ID | Path: id | void |

### Schemas

#### Raider
```json
{
  "id": "ObjectId (string)",
  "name": "string",
  "score": "integer"
}
```

#### RaiderReactive (SSE Response)
```json
{
  "raider": {
    "id": "ObjectId (string)",
    "name": "string",
    "score": "integer"
  },
  "date": "ISO date string"
}
```

### SSE Endpoint Details

**Endpoint:** `GET /reactive/action/{id}/{sec}`

| Attribute | Value |
|-----------|-------|
| Content-Type | text/event-stream |
| Event Interval | 2 seconds |
| Total Events | {sec} parameter |
| Connection | Keep-alive until complete |

**Example SSE Response:**
```
data: {"raider":{"id":"...","name":"John","score":100},"date":"2024-01-01T12:00:00.000Z"}

data: {"raider":{"id":"...","name":"John","score":100},"date":"2024-01-01T12:00:02.000Z"}
```

---

## Module 3: mongo-grid-fs-kt API

**Base URL:** `http://localhost:8082`

### File Endpoints

| Method | Endpoint | Description | Request | Response |
|--------|----------|-------------|---------|----------|
| GET | /files | List all files | - | `List<String>` (filenames) |
| GET | /files/{name:.+} | Download file | Path: name (with ext) | Binary + Headers |
| POST | /files | Upload file | Multipart: file | JavaScript redirect |
| DELETE | /files/delete/{name:.+} | Delete file | Path: name (with ext) | "Delete Success" |

### Upload Request

**Content-Type:** `multipart/form-data`

| Field | Type | Description |
|-------|------|-------------|
| file | File | The file to upload |

**Constraints:**
- Max file size: 100MB
- Max request size: 100MB

### Download Response Headers

| Header | Value |
|--------|-------|
| Content-Disposition | inline |
| Content-Type | Original file MIME type |
| Content-Length | File size in bytes |

### Static Resources

| Path | Resource |
|------|----------|
| / | index.html (file upload/list UI) |

---

## Error Handling

### Common HTTP Status Codes

| Code | Description | When |
|------|-------------|------|
| 200 | OK | Successful GET/POST/PUT |
| 201 | Created | Successful insert (implicit) |
| 404 | Not Found | Resource not found |
| 500 | Server Error | Database or IO error |

### Error Response (GridFS)

```
"Not Found" (plain text)
```

---

## Content Types

| Operation | Content-Type |
|-----------|--------------|
| JSON Request | application/json |
| JSON Response | application/json |
| SSE Response | text/event-stream |
| File Upload | multipart/form-data |
| File Download | (varies by file type) |
| File List | application/json |

---

## API Usage Examples

### Create Person (mongo-kt)
```bash
curl -X PUT http://localhost:8080/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John",
    "age": 30,
    "address": {"Country": "USA", "city": "NYC"}
  }'
```

### Get All Raiders Reactive (mongo-reactive-kt)
```bash
curl http://localhost:8081/reactive/all
```

### Subscribe to SSE Stream
```javascript
const eventSource = new EventSource(
  'http://localhost:8081/reactive/action/abc123/10'
);
eventSource.onmessage = (e) => console.log(JSON.parse(e.data));
```

### Upload File (mongo-grid-fs-kt)
```bash
curl -X POST http://localhost:8082/files \
  -F "file=@document.pdf"
```

### Download File
```bash
curl -O http://localhost:8082/files/document.pdf
```
