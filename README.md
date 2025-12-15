# @manifesto-ai/bridge

[![CI](https://github.com/manifesto-ai/bridge/actions/workflows/ci.yml/badge.svg)](https://github.com/manifesto-ai/bridge/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> Bridge packages for Manifesto AI — Connect domain models to UI frameworks

## Overview

This monorepo contains **bridge packages** that connect [Manifesto Core](https://github.com/manifesto-ai/core)'s domain models to various UI frameworks and state management libraries. Bridges provide **two-way bindings** between your domain logic and UI components.

```
┌─────────────────────────────────────────────────────────────┐
│                    UI Frameworks                            │
│           (React, Vue, Svelte, etc.)                        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  @manifesto-ai/bridge-*                     │
│                                                             │
│   ┌────────────────┐ ┌────────────────┐ ┌────────────────┐  │
│   │     bridge     │ │ bridge-rhf     │ │ bridge-zustand │  │
│   └────────────────┘ └────────────────┘ └────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (two-way binding)
┌─────────────────────────────────────────────────────────────┐
│                    @manifesto-ai/core                       │
│               (Domain Models, Runtime, Effects)             │
└─────────────────────────────────────────────────────────────┘
```

## Packages

| Package | Description | npm |
|---------|-------------|-----|
| [`@manifesto-ai/bridge`](./packages/bridge) | Core bridge interfaces and Vanilla implementation | [![npm](https://img.shields.io/npm/v/@manifesto-ai/bridge)](https://www.npmjs.com/package/@manifesto-ai/bridge) |
| [`@manifesto-ai/bridge-react-hook-form`](./packages/bridge-react-hook-form) | React Hook Form integration | [![npm](https://img.shields.io/npm/v/@manifesto-ai/bridge-react-hook-form)](https://www.npmjs.com/package/@manifesto-ai/bridge-react-hook-form) |
| [`@manifesto-ai/bridge-zustand`](./packages/bridge-zustand) | Zustand state management integration | [![npm](https://img.shields.io/npm/v/@manifesto-ai/bridge-zustand)](https://www.npmjs.com/package/@manifesto-ai/bridge-zustand) |

## Key Principles

### 1. Two-Way Bindings

Bridges synchronize state between UI and domain:

```typescript
// UI → Domain
bridge.set('data.name', 'John');

// Domain → UI (reactive)
bridge.subscribe((snapshot) => {
  updateUI(snapshot);
});
```

### 2. Framework Adapters

Each bridge adapts to the target framework's patterns:

```typescript
// React Hook Form
const { register, handleSubmit } = useManifestoBridge(domain);

// Zustand
const useStore = createManifestoStore(domain);
```

### 3. One-Way Dependencies

```
bridge-* → @manifesto-ai/bridge → @manifesto-ai/core
```

## Installation

```bash
# Core bridge (required)
pnpm add @manifesto-ai/bridge @manifesto-ai/core

# Framework-specific bridges (choose one or more)
pnpm add @manifesto-ai/bridge-react-hook-form
pnpm add @manifesto-ai/bridge-zustand
```

## Quick Examples

### Vanilla Bridge

```typescript
import { createBridge } from '@manifesto-ai/bridge/vanilla';

const bridge = createBridge(domain, runtime);

// Set values
bridge.set('data.name', 'John');

// Get values
const name = bridge.get('data.name');

// Subscribe to changes
bridge.subscribe((snapshot) => {
  console.log('State changed:', snapshot);
});
```

### React Hook Form Bridge

```typescript
import { useManifestoBridge } from '@manifesto-ai/bridge-react-hook-form';

function MyForm() {
  const { register, handleSubmit, fieldStates } = useManifestoBridge(domain);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('data.name')}
        disabled={!fieldStates['data.name']?.enabled}
      />
    </form>
  );
}
```

### Zustand Bridge

```typescript
import { createManifestoStore } from '@manifesto-ai/bridge-zustand';

const useStore = createManifestoStore(domain);

function MyComponent() {
  const { data, set, fieldStates } = useStore();

  return (
    <input
      value={data.name}
      onChange={(e) => set('data.name', e.target.value)}
      disabled={!fieldStates['data.name']?.enabled}
    />
  );
}
```

## Development

### Prerequisites

- Node.js >= 22
- pnpm >= 9

### Setup

```bash
# Clone the repository
git clone https://github.com/manifesto-ai/bridge.git
cd bridge

# Install dependencies
pnpm install

# Build all packages
pnpm build

# Run tests
pnpm test

# Type check
pnpm typecheck
```

### Project Structure

```
bridge/
├── packages/
│   ├── bridge/                  # Core interfaces & Vanilla
│   ├── bridge-react-hook-form/  # React Hook Form adapter
│   └── bridge-zustand/          # Zustand adapter
├── .github/workflows/           # CI/CD
├── package.json                 # Root workspace config
├── pnpm-workspace.yaml
└── turbo.json                   # Build orchestration
```

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Related Projects

- [@manifesto-ai/core](https://github.com/manifesto-ai/core) - Core domain modeling and runtime
- [@manifesto-ai/projection](https://github.com/manifesto-ai/projection) - Read-only projections

## License

MIT © [Manifesto AI](https://github.com/manifesto-ai)
