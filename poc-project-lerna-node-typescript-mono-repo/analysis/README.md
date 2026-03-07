# Project Analysis: Lerna TypeScript Monorepo

## Project Overview

| Attribute | Value |
|-----------|-------|
| **Project Name** | @lerna-ts/monorepo |
| **Project Type** | TypeScript Monorepo (Library + Service) |
| **Primary Language** | TypeScript 4.5.5 |
| **Runtime** | Node.js (18.0.0 - 20.10.0) |
| **Package Manager** | Yarn (Workspaces) |
| **Monorepo Tool** | Lerna 6.6.0 |
| **Module System** | CommonJS (ES2022 target) |
| **Architecture** | Modular Monorepo with Workspace Dependencies |

---

## Technology Stack

### Core Technologies

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| Language | TypeScript | 4.5.5 | Primary programming language |
| Runtime | Node.js | 18.x - 20.x | JavaScript runtime |
| Package Manager | Yarn | 1.x | Dependency management + workspaces |
| Monorepo | Lerna | 6.6.0 | Multi-package orchestration |
| Testing | Jest | 27.5.1 | Test runner with coverage |

### Development Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| typescript | ^4.5.5 | TypeScript compiler |
| jest | ^27.5.1 | Test framework |
| ts-jest | ^27.1.3 | Jest TypeScript preprocessor |
| tslint | ^6.1.3 | Code linting (legacy) |
| prettier | ^2.8.7 | Code formatting |
| cross-env | ^7.0.3 | Cross-platform env vars |

### TypeScript Configuration

| Setting | Value | Purpose |
|---------|-------|---------|
| target | ES2022 | ECMAScript target version |
| module | commonjs | Module output format |
| strict | true | Strict type checking |
| declaration | true | Generate .d.ts files |
| sourceMap | true | Enable source maps |

---

## Framework Patterns Demonstrated

### Monorepo Patterns

| Pattern | Implementation | Purpose |
|---------|----------------|---------|
| **Workspace Dependencies** | `@lerna-ts/sample-common` in package.json | Internal package linking |
| **Shared Configuration** | Root-level tsconfig.json | DRY configuration |
| **Package-Specific Overrides** | Package tsconfig.json | Output directory customization |
| **Unified Scripts** | Root package.json scripts | Single command for all packages |

### Package Organization

| Pattern | Example | Module |
|---------|---------|--------|
| Commons Layer | Shared utilities | packages/commons/sample |
| Services Layer | Consumer packages | packages/services/sample |
| Function Re-export | `export { fn } from '@pkg'` | sample-service |

---

## Business Target

| Aspect | Description |
|--------|-------------|
| **Purpose** | Demonstrate TypeScript monorepo setup with Lerna + Yarn Workspaces |
| **Domain** | Generic utility library pattern |
| **Use Case** | Shared code organization across multiple packages |
| **Complexity** | Simple domain model, focus on infrastructure patterns |

### Learning Objectives

1. **Monorepo Setup** - Lerna + Yarn Workspaces configuration
2. **TypeScript Compilation** - Multi-package TypeScript build
3. **Internal Dependencies** - Workspace package linking
4. **Shared Configuration** - Root-level config inheritance
5. **Test Coverage** - Jest with 100% coverage threshold
6. **Code Quality** - TSLint + Prettier integration

---

## Client Target

| Client Type | Interface | Description |
|-------------|-----------|-------------|
| **Node.js Application** | npm import | Import compiled packages |
| **TypeScript Project** | TypeScript import | Use with type definitions |
| **CLI Tool** | Function invocation | Use exported utilities |

### Client Examples

#### Node.js Consumer
```javascript
// Using sample-service (re-exports from sample-common)
const { sampleFunction } = require('@lerna-ts/sample-service');

const result = sampleFunction('Hello');
// Returns: "SampleFunction Print :: Hello"
```

#### TypeScript Consumer
```typescript
// With full type support
import { sampleFunction } from '@lerna-ts/sample-service';

const result: string = sampleFunction('World');
// Returns: "SampleFunction Print :: World"
```

---

## Module Summary

| Module | Location | Type | Output | Dependencies |
|--------|----------|------|--------|--------------|
| **@lerna-ts/sample-common** | packages/commons/sample | Library | lib/ | None |
| **@lerna-ts/sample-service** | packages/services/sample | Service | dist/ | sample-common |

### Package Details

| Package | Entry Point | Types | Exports |
|---------|-------------|-------|---------|
| sample-common | lib/index.js | lib/index.d.ts | `sampleFunction` |
| sample-service | dist/index.js | lib/index.d.ts | `sampleFunction` (re-export) |

---

## Analysis Documents

| Document | Description |
|----------|-------------|
| [architecture-analysis.md](./architecture-analysis.md) | Monorepo structure, packages, configuration, build pipeline |
| [c4-model-analysis.md](./c4-model-analysis.md) | C4 Context, Container, and Component level analysis |
| [dependency-analysis.md](./dependency-analysis.md) | Package dependencies, dev dependencies, dependency graph |
| [data-flow-analysis.md](./data-flow-analysis.md) | Function export flow, build pipeline, compilation order |

---

## Diagram Artifacts

| Diagram | Description |
|---------|-------------|
| [architecture-overview.drawio](../diagrams/architecture-overview.drawio) | Full monorepo structure with packages and configuration |
| [c4-1-context.drawio](../diagrams/c4-1-context.drawio) | C4 System Context diagram |
| [c4-2-container.drawio](../diagrams/c4-2-container.drawio) | C4 Container diagram |
| [c4-3-component-sample-common.drawio](../diagrams/c4-3-component-sample-common.drawio) | Component diagram for sample-common |
| [c4-3-component-sample-service.drawio](../diagrams/c4-3-component-sample-service.drawio) | Component diagram for sample-service |
| [module-dependencies.drawio](../diagrams/module-dependencies.drawio) | Dependency graph between packages |
| [data-flow.drawio](../diagrams/data-flow.drawio) | Data flow and build pipeline visualization |

---

## Quick Start

### Prerequisites
- Node.js 18.x - 20.x
- Yarn 1.x
- Git

### Installation & Setup

```bash
# Clone the repository
git clone <repository-url>
cd sample-lerna-ts-mono-repo

# Full setup (install, bootstrap, link, compile, test)
yarn setup

# Or step by step:
yarn install           # Install root dependencies
yarn link:force        # Bootstrap and link packages
yarn tsc               # Compile TypeScript
yarn test              # Run tests
```

### Development Commands

```bash
# Watch mode compilation
yarn tsc:watch

# Run tests with coverage
yarn test

# Code quality
yarn format:fix        # Lint + Prettier fix

# Clean everything
yarn clean

# Full reset
yarn reset             # Clean + setup
```

---

## Sample Code Snippets

### Shared Library (sample-common)

```typescript
// packages/commons/sample/src/index.ts
export const sampleFunction = (msg: string): string =>
  `SampleFunction Print :: ${msg}`
```

### Service Package (sample-service)

```typescript
// packages/services/sample/src/index.ts
export { sampleFunction } from '@lerna-ts/sample-common'
```

### Test Suite

```typescript
// src/index.test.ts (both packages)
import { sampleFunction } from './index'

describe('Test Sample', () => {
  test('Case Sample', () => {
    expect(sampleFunction('Mock')).toEqual('SampleFunction Print :: Mock')
  })
})
```

### Jest Configuration

```typescript
// jest.config.ts
import type { Config } from '@jest/types'

const config: Config.InitialOptions = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testRegex: '(/__tests__/.*|(\\.|/)(test|spec))\\.tsx?$',
  coverageThreshold: {
    global: {
      branches: 100,
      functions: 100,
      lines: 100,
      statements: 100
    }
  },
  setupFilesAfterEnv: ['../../../.jest/setup.ts']
}

export default config
```

---

## Configuration

### Root package.json (Workspace)

```json
{
  "name": "@lerna-ts/monorepo",
  "private": true,
  "workspaces": ["packages/**"],
  "engines": {
    "node": ">= 18.0.0 <= 20.10.0"
  }
}
```

### lerna.json

```json
{
  "npmClient": "yarn",
  "packages": ["packages/**"],
  "version": "1.0.0"
}
```

### Root tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "lib": ["ES2022"],
    "strict": true,
    "declaration": true,
    "sourceMap": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

### .prettierrc

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none"
}
```

---

## Directory Structure

```
sample-lerna-ts-mono-repo/
├── .env                          # Environment (RUN=TEST)
├── .jest/
│   └── setup.ts                  # Shared Jest setup
├── lerna.json                    # Lerna configuration
├── package.json                  # Root workspace
├── tsconfig.json                 # Base TypeScript config
├── tslint.json                   # Source linting
├── tslint.test.json              # Test linting
├── .prettierrc                   # Formatting rules
│
└── packages/
    ├── commons/
    │   └── sample/               # @lerna-ts/sample-common
    │       ├── src/
    │       │   ├── index.ts
    │       │   └── index.test.ts
    │       ├── lib/              # Compiled output
    │       ├── coverage/         # Test coverage
    │       ├── package.json
    │       ├── tsconfig.json
    │       └── jest.config.ts
    │
    └── services/
        └── sample/               # @lerna-ts/sample-service
            ├── src/
            │   ├── index.ts
            │   └── index.test.ts
            ├── dist/             # Compiled output
            ├── coverage/         # Test coverage
            ├── package.json
            ├── tsconfig.json
            └── jest.config.ts
```

---

## Key Takeaways

| Topic | Lesson |
|-------|--------|
| **Monorepo Setup** | Use Lerna + Yarn Workspaces for multi-package TypeScript projects |
| **Workspace Dependencies** | Use `^version` constraint for internal packages in package.json |
| **Output Directories** | Use `lib/` for libraries, `dist/` for services (convention) |
| **Shared Configuration** | Place common config at root, override in packages as needed |
| **Build Order** | Lerna respects dependency order (commons before services) |
| **Test Coverage** | Enforce 100% coverage threshold for high code quality |
| **Type Definitions** | Generate .d.ts files alongside compiled JavaScript |
| **Code Quality** | Combine TSLint (or ESLint) + Prettier for consistent code |

---

## Dependency Graph

```
@lerna-ts/sample-service
    │
    └── imports ────▶ @lerna-ts/sample-common
                           │
                           └── (no external dependencies)
```

### Compilation Order

1. **@lerna-ts/sample-common** - Compiles first (no dependencies)
2. **@lerna-ts/sample-service** - Compiles second (depends on sample-common)

---

## Scripts Reference

| Script | Description |
|--------|-------------|
| `yarn setup` | Full setup: install, bootstrap, link, compile, test |
| `yarn clean` | Remove all generated files and node_modules |
| `yarn tsc` | Compile all packages (respects dependency order) |
| `yarn tsc:watch` | Watch mode compilation |
| `yarn test` | Run all tests with coverage |
| `yarn lint` / `yarn lint:fix` | Check/fix code quality |
| `yarn prettier` / `yarn prettier:fix` | Check/fix code formatting |
| `yarn format` / `yarn format:fix` | Combined lint + prettier |
| `yarn packages` | Full pipeline: setup + format + test |
| `yarn reset` | Clean + packages |
