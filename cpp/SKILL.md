---
name: cpp
description: C++23/CMake/Ninja/Clang standards for .cpp/.hpp, CMake, tests, dependencies, and scaffolding.
---

## Baseline

Default to these unless stricter local conventions exist:

- C++23, CMake, Ninja, Clang.
- Catch2 + CTest.
- Keep `.cpp` and `.hpp` together under `src/`; no separate `include/` tree unless explicitly requested.
- Use only `.cpp` and `.hpp` extensions.

## Project Layout

```text
project/
├── CMakeLists.txt
├── cmake/
├── external/
│   ├── CMakeLists.txt
│   ├── cmake/
│   └── modules/
├── src/
└── tests/
```

## Naming

- Classes, enums, enum classes, namespaces: `PascalCase`
- Files: `PascalCase.cpp` / `PascalCase.hpp`, matching the primary class
- Functions, methods, locals, folders: `camelCase`
- Getters and setters: prefix with `get` / `set`, e.g. `getValue()` / `setValue(...)`
- Members: `camelCase_`
- Constants: `SCREAMING_SNAKE_CASE`

## Formatting

- 4 spaces.
- Same-line opening braces.

## CMake Standards

- Target-based CMake; avoid global include paths, definitions, and options unless intentionally project-wide.
- Target-scoped C++23 compile features; expose as `PUBLIC` only when consumers need it.
- Add project warnings via an interface target such as `project_options`, or a `cmake/` helper.
- Warnings as errors: project-owned targets only, never third-party targets.
- Third-party deps: submodules in `external/modules/`, helpers in `external/cmake/`, entry in `external/CMakeLists.txt`.

## Testing

- Tests live under `tests/`.
- Name tests after the unit under test, e.g. `RendererTests.cpp`.
- Keep test-only helpers in `tests/`; promote only reusable production abstractions.

## Design Standards

- Use one class per `.hpp` / `.cpp` pair; do not place multiple classes in the same file.
- RAII and deterministic ownership; Rule of Zero unless ownership/lifetime requires special members.
- Prefer value semantics; encode ownership/borrowing in values, references, smart pointers, and `std::span`.
- PImpl only when it materially reduces compile-time coupling or ABI exposure.
- Prefer static polymorphism, values, templates, and function objects; use `virtual` only when runtime polymorphism is required.
- Make invalid states unrepresentable with stronger types, constructors, factories, and invariants.
- Prefer dependency injection for replaceable behavior and test seams.
- Use policy-based design for compile-time, allocation-free behavior variation.

## Error Handling

- `std::expected<T, E>` for recoverable/API-level failures.
- Explicit, small error types: enum classes or lightweight structs.
- Assertions for violated internal invariants.
- Exceptions only at boundaries, existing APIs, or documented integration constraints.
- No nullable raw pointers for errors; use `std::expected`, `std::optional`, references, or smart pointers by semantics.
