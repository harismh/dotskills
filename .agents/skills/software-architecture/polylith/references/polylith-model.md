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

## Behavior-first composition

Shift design from **what kind of thing is this?** to **what can happen, who can do it, and how do those behaviors coordinate?** Use this vocabulary:

- **Operation:** an action—the smallest meaningful verb in the system, independent of an owning type.
- **Capability:** permission plus an implementation of an operation. It answers both “may this participant perform the action?” and “how is the action realized here?”
- **Protocol:** the coordination of capabilities, including sequencing, invariants, state transitions, failure semantics, and composition rules.
- **Meta-level protocol:** the rules and mechanisms by which protocols are defined, composed, inspected, validated, replaced, and evolved.

Use the progression in that order:

1. Name operations from observable behavior.
2. Define narrow capabilities that realize those operations without creating a type hierarchy.
3. Specify protocols where several capabilities must cooperate. Include laws and shared compliance tests, not only function signatures.
4. Package cohesive behavior into Polylith components and expose the necessary operations through their interfaces.
5. Assemble components in projects, inspect their relationships with `poly`, and evolve implementations without changing callers that depend on stable behavior.

In this sense, Polylith supplies a meta-level architectural protocol: its workspace structure, interfaces, project composition, dependency rules, reflection through `poly info`, `poly deps`, and `poly ws`, validation, and incremental tests govern how behavioral units are defined, composed, observed, and evolved.

Keep language mechanisms separate from the model. A Clojure `defprotocol` is often an effective implementation of a capability because it provides open behavioral dispatch over values. It is not automatically a coordination protocol in the sense above. Ordinary functions, maps of functions, multimethods, interpreters, or data-driven effect descriptions can also realize capabilities. Choose the simplest representation that preserves the behavioral contract.

Yggdrasil demonstrates the desired orientation. `Snapshotable`, `Branchable`, `Mergeable`, and the other “xyz-able” abstractions form a behavioral vocabulary across heterogeneous storage systems. Their composition and common compliance suite matter more than a taxonomy of Git, ZFS, Datahike, or other backend types. Prefer this style when it reveals stable operations and useful laws; reject “-able” names that merely disguise CRUD or speculative abstraction.

## Review questions

- Did the design begin with operations and coordination rather than a taxonomy of nouns or types?
- Are capabilities narrow combinations of authority and implementation?
- Do protocols make sequencing, invariants, failures, and behavioral laws explicit?
- Does each component package one coherent behavioral boundary?
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
- [Yggdrasil: Branching Protocols](https://datahike.io/notes/yggdrasil-unified-cow-protocols/)
- [Documentation](https://cljdoc.org/d/polylith/clj-poly)
  - [For LLMs](https://polylith.gitbook.io/polylith/llms.txt)
