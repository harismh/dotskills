---
name: polylith
description: Design, explain, create, inspect, test, and evolve Clojure or ClojureScript Polylith workspaces with the poly CLI. Use when working with Polylith architecture, workspaces, components, bases, projects, bricks, interfaces, workspace.edn, incremental tests, dependency diagrams, or poly commands; when deciding brick boundaries or migrating a backend toward Polylith; and when reviewing whether code preserves Polylith's simplicity and composability.
---

# Polylith

Treat Polylith first as a core architectural pattern and second as a CLI. Preserve its goal: a monolithic development experience made from simple, composable service-level building blocks.

## Start from the big ideas

- Keep all bricks together in one workspace so coordinated changes stay atomic and development uses the latest code.
- Model a cohesive capability as a component. Give it a small interface and keep its implementation ordinary code.
- Let components depend on other component interfaces and libraries, never on another component's implementation or identity.
- Use a base only at a system boundary: REST, GraphQL, gRPC, Lambda, CLI, messaging, or another public entry point. Delegate domain behavior from the base to components.
- Let a project choose the bricks and libraries that form one deployable artifact. Do not put reusable domain behavior in the project.
- Use the development project to work across all bricks with unified navigation, refactoring, debugging, tests, and a REPL.
- Prefer function-like qualities at the component level: one responsibility, explicit contract, low state, easy composition, and easy replacement.
- Separate **what** from **how**: interfaces and public APIs state what is available; implementations and production assembly decide how it works.

Read [references/polylith-model.md](references/polylith-model.md) when explaining the architecture, choosing boundaries, reviewing a design, or planning a migration.

## Work with the CLI

Use the installed CLI as the source of truth because commands and flags evolve:

```sh
poly version
poly help
poly help COMMAND
poly help create TYPE
```

If the workspace uses the Clojure CLI dependency instead of the standalone executable, substitute `clojure -M:poly` for `poly`. Run `poly` or `poly shell` for an interactive shell with history and completion; omit the `poly` prefix inside that shell.

Use the basic command loop:

```sh
poly info                 # summarize the workspace and surface validation messages
poly check                # validate architectural and workspace rules
poly deps                 # inspect workspace dependencies
poly diff                 # inspect changes since the stable point
poly test                 # test changed bricks/projects incrementally
poly libs                 # inspect external libraries
poly ws                   # expose the workspace model as data
```

Create only after checking exact local help:

```sh
poly help create component
poly create component name:orders
poly create base name:http-api
poly create project name:service
poly create workspace name:example top-ns:com.example
```

For any unfamiliar request, do not guess syntax. Run `poly help`, then `poly help COMMAND`; use `poly doc help:COMMAND` when browser-based detail is useful. Use shell completion to discover accepted arguments. Report the exact command form the installed version supports.

## Follow the operating workflow

1. Inspect `workspace.edn`, directory structure, and relevant `deps.edn` files.
2. Run `poly version`, `poly info`, and `poly check` before structural changes.
3. Identify whether the change belongs in a component, base, project, or workspace configuration.
4. Check command-specific help before creating or invoking a less familiar operation.
5. Make the smallest change that preserves interface-only component relationships.
6. Run `poly check`, inspect `poly deps` when relationships changed, and run the relevant `poly test` command.
7. Explain the result in architectural terms, not only as generated files or passing commands.

## Apply design judgment

- Extract a component around a stable capability, not merely because a namespace or layer exists.
- Keep an interface narrow. Avoid exposing implementation namespaces or data that couples callers to internals.
- Reuse living source bricks across projects; do not publish internal components as versioned libraries merely to share them inside the workspace.
- Keep bases thin. If business rules appear in endpoint handlers, move them behind a component interface.
- Treat circular or bidirectional dependencies as a boundary problem. Redesign responsibilities instead of bypassing validation.
- Do not introduce service boundaries, asynchronous messaging, dependency injection frameworks, or mutable component containers unless the problem truly requires them. Polylith's default connection is a direct function call.
- Avoid turning Polylith into a directory convention. The value comes from explicit interfaces, one-way dependencies, composable project assembly, and incremental feedback.

When recommending a structure, state why each proposed brick is cohesive, what its interface owns, which other interfaces it uses, and which projects include it.
