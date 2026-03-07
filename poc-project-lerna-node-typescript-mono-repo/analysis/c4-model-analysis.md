# C4 Model Analysis: Lerna TypeScript Monorepo

## Overview

This document provides C4 model analysis for the Lerna TypeScript Monorepo project, following the standard C4 hierarchy: Context → Container → Component.

---

## Level 1: System Context Diagram

### System in Scope

| Element | Type | Description |
|---------|------|-------------|
| **Lerna TypeScript Monorepo** | Software System | A modular TypeScript monorepo providing shared utilities and services |

### Actors (Users)

| Actor | Type | Description |
|-------|------|-------------|
| **Developer** | Person | Develops, tests, and maintains the monorepo packages |
| **Build System** | System | CI/CD or local build process that compiles and tests packages |
| **Consuming Application** | External System | External applications that import published packages |

### External Systems

| System | Description | Interaction |
|--------|-------------|-------------|
| **npm Registry** | Package registry (if published) | Publish/consume packages |
| **Git Repository** | Source control | Version control, collaboration |

### Context Relationships

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   ┌──────────┐           ┌─────────────────────────────────┐       │
│   │Developer │ ─────────▶│   Lerna TypeScript Monorepo    │       │
│   │ (Person) │           │      [Software System]          │       │
│   └──────────┘           │                                 │       │
│        │                 │  Provides shared utilities and  │       │
│        │                 │  services for TypeScript apps   │       │
│        ▼                 └─────────────────────────────────┘       │
│   ┌──────────┐                         │                           │
│   │  Build   │                         │                           │
│   │  System  │ ◀───────────────────────┘                           │
│   └──────────┘                         │                           │
│                                        ▼                           │
│                          ┌─────────────────────────────────┐       │
│                          │    Consuming Application        │       │
│                          │      [External System]          │       │
│                          └─────────────────────────────────┘       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Level 2: Container Diagram

### System Boundary

**System**: Lerna TypeScript Monorepo
**Boundary Description**: Node.js workspace-based monorepo managed by Lerna

### Containers

| Container | Technology | Description |
|-----------|------------|-------------|
| **sample-common** | TypeScript (CommonJS) | Shared library providing core utilities |
| **sample-service** | TypeScript (CommonJS) | Service package consuming and re-exporting commons |
| **Root Workspace** | Yarn Workspaces | Orchestration layer managing dependencies |

### Container Details

#### 1. @lerna-ts/sample-common

| Attribute | Value |
|-----------|-------|
| **Type** | Library Package |
| **Technology** | TypeScript 4.5.5 |
| **Output Format** | CommonJS |
| **Entry Point** | `lib/index.js` |
| **Exports** | `sampleFunction` |

#### 2. @lerna-ts/sample-service

| Attribute | Value |
|-----------|-------|
| **Type** | Service Package |
| **Technology** | TypeScript 4.5.5 |
| **Output Format** | CommonJS |
| **Entry Point** | `dist/index.js` |
| **Dependencies** | @lerna-ts/sample-common |

#### 3. Root Workspace

| Attribute | Value |
|-----------|-------|
| **Type** | Orchestration |
| **Technology** | Yarn Workspaces + Lerna |
| **Purpose** | Build, test, lint, format |
| **Package Discovery** | `packages/**` |

### Container Relationships

```
┌───────────────────────────────────────────────────────────────────────────┐
│  Lerna TypeScript Monorepo [System Boundary]                              │
│                                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      Root Workspace                                  │  │
│  │                   [Container: Yarn + Lerna]                          │  │
│  │                                                                      │  │
│  │  Orchestrates build, test, and dependency management                 │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                           │                                               │
│            ┌──────────────┼──────────────┐                                │
│            │              │              │                                │
│            ▼              ▼              ▼                                │
│  ┌───────────────┐              ┌───────────────────┐                     │
│  │ sample-common │◀─────────────│  sample-service   │                     │
│  │ [Container:   │   imports    │  [Container:      │                     │
│  │  TypeScript]  │              │   TypeScript]     │                     │
│  │               │              │                   │                     │
│  │ Shared utils  │              │ Re-exports from   │                     │
│  │ (sampleFunc)  │              │ sample-common     │                     │
│  └───────────────┘              └───────────────────┘                     │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## Level 3: Component Diagram

### Container: @lerna-ts/sample-common

#### Components

| Component | Type | Description |
|-----------|------|-------------|
| **index.ts** | Entry Point | Main module exporting all public functions |
| **sampleFunction** | Function | Core utility function |
| **index.test.ts** | Test Suite | Jest test cases |

#### Component Structure

```typescript
// src/index.ts
export const sampleFunction = (msg: string): string =>
  `SampleFunction Print :: ${msg}`
```

#### Component Diagram

```
┌────────────────────────────────────────────────────────────────────────┐
│  @lerna-ts/sample-common [Container Boundary]                          │
│                                                                        │
│  ┌────────────────────────────────────────────────────────────────┐   │
│  │                        src/                                     │   │
│  │  ┌──────────────────┐         ┌──────────────────────────┐     │   │
│  │  │    index.ts      │         │    index.test.ts         │     │   │
│  │  │  [Entry Point]   │ ◀─tests─│    [Test Suite]          │     │   │
│  │  │                  │         │                          │     │   │
│  │  │  ┌────────────┐  │         │  - Test sampleFunction   │     │   │
│  │  │  │sampleFunc  │  │         │  - 100% coverage         │     │   │
│  │  │  │ (exported) │  │         │                          │     │   │
│  │  │  └────────────┘  │         └──────────────────────────┘     │   │
│  │  └──────────────────┘                                           │   │
│  └────────────────────────────────────────────────────────────────┘   │
│                                                                        │
│  Compiled Output: lib/                                                 │
│  ├── index.js      (CommonJS)                                         │
│  └── index.d.ts    (Type Definitions)                                 │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

---

### Container: @lerna-ts/sample-service

#### Components

| Component | Type | Description |
|-----------|------|-------------|
| **index.ts** | Entry Point | Re-exports from sample-common |
| **index.test.ts** | Test Suite | Jest test cases |

#### Component Structure

```typescript
// src/index.ts
export { sampleFunction } from '@lerna-ts/sample-common'
```

#### Component Diagram

```
┌────────────────────────────────────────────────────────────────────────┐
│  @lerna-ts/sample-service [Container Boundary]                         │
│                                                                        │
│  ┌────────────────────────────────────────────────────────────────┐   │
│  │                        src/                                     │   │
│  │  ┌──────────────────┐         ┌──────────────────────────┐     │   │
│  │  │    index.ts      │         │    index.test.ts         │     │   │
│  │  │  [Entry Point]   │ ◀─tests─│    [Test Suite]          │     │   │
│  │  │                  │         │                          │     │   │
│  │  │  Re-exports:     │         │  - Test sampleFunction   │     │   │
│  │  │  sampleFunction  │         │  - 100% coverage         │     │   │
│  │  │  from commons    │         │                          │     │   │
│  │  └──────────────────┘         └──────────────────────────┘     │   │
│  │          │                                                      │   │
│  │          │ imports                                              │   │
│  │          ▼                                                      │   │
│  │  ┌──────────────────────────────┐                               │   │
│  │  │  @lerna-ts/sample-common     │                               │   │
│  │  │  [External Dependency]       │                               │   │
│  │  └──────────────────────────────┘                               │   │
│  └────────────────────────────────────────────────────────────────┘   │
│                                                                        │
│  Compiled Output: dist/                                                │
│  ├── index.js      (CommonJS)                                         │
│  └── index.d.ts    (Type Definitions)                                 │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

---

## Relationship Summary

| Source | Target | Relationship | Technology |
|--------|--------|--------------|------------|
| Developer | Monorepo | Develops | IDE, CLI |
| Build System | Monorepo | Builds/Tests | Yarn, Jest |
| sample-service | sample-common | Imports | TypeScript import |
| Root Workspace | All Packages | Manages | Lerna, Yarn |

---

## Diagram Generation Notes

### C4 Context Diagram
- Show Developer and Consuming Application as actors
- Show Monorepo as single system
- Optional: npm Registry as external system

### C4 Container Diagram
- System boundary: Lerna TypeScript Monorepo
- Two containers: sample-common, sample-service
- Show dependency arrow from service to common

### C4 Component Diagram (sample-common)
- Container boundary: @lerna-ts/sample-common
- Components: index.ts (with sampleFunction), index.test.ts
- Output artifacts: lib/index.js, lib/index.d.ts

### C4 Component Diagram (sample-service)
- Container boundary: @lerna-ts/sample-service
- Components: index.ts, index.test.ts
- External dependency: @lerna-ts/sample-common
- Output artifacts: dist/index.js, dist/index.d.ts
