# Global Agent Guidelines — Cross-Cutting Rules

Applies to all projects under this directory.
Stack-specific rules live in each project's own `AGENTS.md`.

---

## 1. Double Check

Before showing generated code, verify:
- No unused variables or imports.
- No phantom imports (referenced but not declared).
- All constants referenced are defined.

---

## 2. Dependency Check

Before adding a library, inspect the project manifest
(`pubspec.yaml` / `build.gradle.kts` / `pom.xml`) to confirm
no equivalent dependency already exists.

---

## 3. Documentation and External Context

- Prefer Context7 for external library and framework APIs before suggesting code that depends on them.
- For library-specific work, resolve the exact Context7 library ID first, then query the docs for the relevant API or pattern.
- Use the version already pinned in the repository when available (`package.json`, `pubspec.yaml`, etc.); otherwise, state which documented version you are using.
- Do not assume outdated patterns when the retrieved docs show a newer default approach.
- If the API surface is uncertain, verify it with Context7 before writing code and say so explicitly.

---

## 4. Simplicity First

Prefer the simplest implementation that solves the current requirement.

Avoid:
- overengineering
- premature abstractions
- speculative configuration
- indirection that makes code harder to read

Optimize for:
- readability
- maintainability
- explicit data flow
- keep classes, components, and services small and focused
- split them before they accumulate multiple responsibilities or become hard to scan
- when a repository integrates external commerce platforms, keep provider-specific code behind ports/adapters and preserve a provider-agnostic core domain/model
- for MongoDB reads, use projections or field-restricted queries when the full document is not needed; do not load full documents for list/search paths that only need a subset of fields
- prefer typed configuration over scattered environment/config reads in application code
- required external integration settings must fail fast at startup; do not rely on silent fallbacks for mandatory config
- treat shared keys, channels, DTO fields, external endpoints, and persisted IDs as public contracts; do not change them silently
- repository documentation files must be written in English; this includes `README.md`, `ARCHITECTURE.md`, `AGENTS.md`, roadmap files, and other persistent project docs

---

## 5. Impact Analysis

Before proposing any change, list:
- Files that will be affected.
- Whether the change breaks backward compatibility.

---

## 6. Two-Alternative Rule

When an architectural decision is required, always present:
- **Option A — Simple**: lower complexity, fewer moving parts.
- **Option B — Scalable**: higher complexity, better long-term fit.

Include concrete pros/cons for both. Let the user decide.

---

## 7. Strategic Logging

Every new flow must include log statements on:
- Happy path (INFO level or equivalent).
- Error path (ERROR/WARN level or equivalent).

Use the project's existing logger — do not introduce a new logging dependency.

---

## 8. Rule Suggestion

If a repeated correction pattern is detected across multiple interactions,
propose a new rule for the affected project's `AGENTS.md` before closing the session.
