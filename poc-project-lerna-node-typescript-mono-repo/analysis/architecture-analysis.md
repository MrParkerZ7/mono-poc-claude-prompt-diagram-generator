# Architecture Analysis: Lerna TypeScript Monorepo

## 1. Application Overview

| Attribute | Value |
|-----------|-------|
| **Project Name** | @lerna-ts/monorepo |
| **Project Type** | TypeScript Monorepo |
| **Language** | TypeScript 4.5.5 |
| **Runtime** | Node.js (18.0.0 - 20.10.0) |
| **Package Manager** | Yarn (Workspaces) |
| **Monorepo Tool** | Lerna 6.6.0 |
| **Module System** | CommonJS (ES2022 target) |

## 2. Architecture Pattern

**Pattern**: Modular Monorepo with Workspace-Based Package Management

This project follows a **monorepo architecture** using Lerna and Yarn Workspaces, organizing code into:
- **Commons packages** (`packages/commons/`) - Shared libraries and utilities
- **Services packages** (`packages/services/`) - Application services consuming commons

### Key Characteristics
- Single repository containing multiple packages
- Shared configuration at root level
- Internal dependency linking via workspace protocols
- Unified version management (all packages share version 1.0.0)

## 3. Project Structure

```
sample-lerna-ts-mono-repo/
├── .env                          # Environment config (RUN=TEST)
├── .jest/
│   └── setup.ts                  # Shared Jest setup (dotenv loading)
├── lerna.json                    # Lerna monorepo configuration
├── package.json                  # Root workspace + scripts
├── tsconfig.json                 # Base TypeScript configuration
├── tslint.json                   # Source code linting rules
├── tslint.test.json              # Test code linting rules
├── .prettierrc                   # Code formatting rules
│
└── packages/
    ├── commons/
    │   └── sample/               # @lerna-ts/sample-common
    │       ├── src/index.ts      # Source: sampleFunction export
    │       ├── src/index.test.ts # Tests
    │       ├── lib/              # Compiled output (CommonJS)
    │       ├── package.json
    │       ├── tsconfig.json
    │       └── jest.config.ts
    │
    └── services/
        └── sample/               # @lerna-ts/sample-service
            ├── src/index.ts      # Source: re-exports from sample-common
            ├── src/index.test.ts # Tests
            ├── dist/             # Compiled output (CommonJS)
            ├── package.json
            ├── tsconfig.json
            └── jest.config.ts
```

## 4. Packages

### 4.1 @lerna-ts/sample-common

| Attribute | Value |
|-----------|-------|
| **Location** | `packages/commons/sample` |
| **Type** | Shared Library |
| **Entry Point** | `lib/index.js` |
| **Type Definitions** | `lib/index.d.ts` |
| **Output Directory** | `lib/` |

**Purpose**: Core shared functionality and utilities

**Exports**:
```typescript
export const sampleFunction = (msg: string): string =>
  `SampleFunction Print :: ${msg}`
```

### 4.2 @lerna-ts/sample-service

| Attribute | Value |
|-----------|-------|
| **Location** | `packages/services/sample` |
| **Type** | Application Service |
| **Entry Point** | `dist/index.js` |
| **Type Definitions** | `lib/index.d.ts` |
| **Output Directory** | `dist/` |
| **Dependencies** | `@lerna-ts/sample-common` |

**Purpose**: Service layer consuming shared utilities

**Exports**:
```typescript
export { sampleFunction } from '@lerna-ts/sample-common'
```

## 5. Configuration Layers

### 5.1 Root Configuration (Shared)

| File | Purpose |
|------|---------|
| `tsconfig.json` | Base TypeScript compiler options |
| `tslint.json` | Source code linting rules |
| `tslint.test.json` | Test code linting rules |
| `.prettierrc` | Code formatting (no semi, single quotes) |
| `lerna.json` | Monorepo package discovery |
| `.jest/setup.ts` | Shared test setup (env loading) |

### 5.2 Package-Level Configuration

Each package overrides:
- **tsconfig.json**: Output directory (`lib/` vs `dist/`)
- **jest.config.ts**: Test patterns and coverage thresholds

## 6. Build Pipeline

### 6.1 TypeScript Compilation

| Package | Source | Output | Format |
|---------|--------|--------|--------|
| sample-common | `src/**/*.ts` | `lib/` | CommonJS + .d.ts |
| sample-service | `src/**/*.ts` | `dist/` | CommonJS + .d.ts |

### 6.2 Build Commands

```bash
yarn setup           # Full setup: install, bootstrap, link, compile, test
yarn tsc             # Compile all packages (dependency order)
yarn tsc:watch       # Watch mode compilation
yarn clean           # Remove all generated files
```

## 7. Testing Infrastructure

| Tool | Version | Purpose |
|------|---------|---------|
| Jest | 27.5.1 | Test runner |
| ts-jest | 27.1.3 | TypeScript preprocessor |

**Coverage Requirements**: 100% (branches, functions, lines, statements)

## 8. Code Quality

| Tool | Purpose |
|------|---------|
| TSLint | Code linting (deprecated, legacy support) |
| Prettier | Code formatting |

**Formatting Rules**:
- No semicolons
- Single quotes
- No trailing commas

## 9. Development Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                     Development Workflow                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  [yarn setup] ──▶ Install ──▶ Bootstrap ──▶ Link ──▶ Compile   │
│                                                                 │
│  [yarn tsc:watch] ──▶ Continuous TypeScript Compilation        │
│                                                                 │
│  [yarn test] ──▶ Jest with Coverage                            │
│                                                                 │
│  [yarn format:fix] ──▶ Lint + Prettier                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 10. Key Architectural Decisions

| Decision | Rationale |
|----------|-----------|
| **Lerna + Yarn Workspaces** | Efficient monorepo management with native workspace support |
| **TypeScript with CommonJS** | Broad Node.js compatibility while maintaining type safety |
| **Separate output directories** | Clear distinction between library (`lib/`) and service (`dist/`) outputs |
| **100% test coverage** | High code quality enforcement |
| **Shared configuration** | DRY principle for tooling configuration |
| **Workspace dependencies** | Local package linking without publishing |

## 11. Scalability Considerations

The architecture supports scaling by:
1. Adding new packages under `packages/commons/` for shared libraries
2. Adding new packages under `packages/services/` for services
3. Creating new category directories for different package types
4. Lerna's `packages/**` glob automatically discovers new packages

## 12. Diagram Requirements

For architecture visualization:
- **C4 Context**: Single system with no external dependencies
- **C4 Container**: Two packages (commons, services)
- **Dependency Graph**: Package relationships
- **Data Flow**: Function export chain
