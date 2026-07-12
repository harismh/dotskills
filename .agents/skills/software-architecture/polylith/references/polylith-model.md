# Polylith model and simplicity

Use this reference for conceptual explanations and architectural decisions. Verify CLI syntax with the installed `poly` tool rather than this file.

## Concept map

- **Function:** the smallest building block; hides implementation behind a signature and is easiest to compose when focused and stateless.
- **Library:** externally versioned code resolved through the language ecosystem.
- **Component:** a service-level, domain, infrastructure, or integration capability expressed as ordinary code behind an interface.
- **Base:** an inbound system boundary that exposes a public API and delegates behavior to components.
- **Brick:** the common term for a component or base.
- **Project:** a declaration of the bricks and libraries assembled into a deployable artifact.
- **Development project:** the unified environment containing all bricks and libraries for navigation, refactoring, debugging, testing, and REPL work.
- **Workspace:** the monorepo location that stores bricks and configures projects.

## Why the constraints simplify the system

1. A monorepo keeps changes coordinated and all projects on current source.
2. One copy of each component and interface removes internal version drift and duplication.
3. Ordinary, preferably stateless code avoids component lifecycle machinery.
4. A separate source root makes each capability independently selectable and reusable.
5. Interface-only component dependencies decouple callers from implementations and make bricks replaceable.
6. One development project keeps contracts aligned and detects circular dependencies across project boundaries.
7. Projects assemble living source bricks directly, maximizing reuse without prematurely publishing libraries.
8. Interfaces, base APIs, and project composition describe **what**; implementations and deployment describe **how**.
9. A small standardized vocabulary reduces the number of architectural concepts a developer must hold at once.

The intended result is a modular monolith: simple direct calls and unified development locally, with multiple independently assembled deployables where needed.

## Review questions

- Does each component represent one coherent capability?
- Can a caller use it knowing only its interface?
- Does any component import another component's implementation namespace?
- Is a base limited to protocol translation, validation, and delegation?
- Does project configuration assemble behavior instead of implementing it?
- Can shared behavior remain a living component instead of becoming a versioned internal library?
- Are all dependencies directional and free of cycles?
- Is added machinery solving a demonstrated need, or obscuring a direct function call?

## Primary sources

- [Polylith in a Nutshell](https://polylith.gitbook.io/polylith/introduction/polylith-in-a-nutshell)
- [Simplicity](https://polylith.gitbook.io/polylith/architecture/simplicity)
- [Documentation](https://cljdoc.org/d/polylith/clj-poly)
  - [For LLMs](https://polylith.gitbook.io/polylith/llms.txt)
