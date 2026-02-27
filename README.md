# Hierarchical Conventional Commits (HCC)

> A strict superset of Conventional Commits introducing hierarchical, path-based scopes.

---

## 📌 Overview

Hierarchical Conventional Commits (HCC) extends the Conventional Commits specification by introducing structured scopes using `/` as a hierarchical separator.

It preserves full compatibility with existing tooling while enabling scalable, architecture-aligned commit semantics.

HCC allows commit messages to reflect real project structure:

Instead of:

    fix(ci): update dependency

You write:

    fix(ci/tauri/build): update libwebkit2gtk from 4.0 to 4.1

This makes commit metadata structurally meaningful.

---

## 🎯 Motivation

Flat scopes do not scale in:

- Large codebases
- Monorepos
- Layered architectures
- Multi-domain systems
- Toolchain-heavy projects

Software systems are hierarchical.

Commit semantics should be too.

HCC introduces structural depth without breaking ecosystem compatibility.

---

## 🧱 Specification

### Commit Format

    <type>(<scope>[/<subscope>...])<!?>: <description>

### Components

#### 1. Type (required)

Indicates the category of change.

Common types:

- feat      → new feature
- fix       → bug fix
- docs      → documentation changes
- style     → formatting, missing semi-colons, etc
- refactor  → code change that neither fixes a bug nor adds a feature
- perf      → performance improvements
- test      → adding or correcting tests
- build     → build system or dependency changes
- ci        → CI configuration changes
- chore     → maintenance tasks

Custom types are allowed.

---

#### 2. Scope (required)

Scopes are hierarchical and use `/` as a separator.

Rules:

- Must not be empty
- Must not contain spaces
- Ordered from most general → most specific
- Unlimited depth
- Recommended: kebab-case
- Case-sensitive (recommended lowercase)

Valid examples:

    fix(core/parser)
    feat(api/rest/user-controller)
    chore(deps/node/npm)
    ci/github/actions/build)
    perf(engine/rendering/rasterizer)

Invalid examples:

    fix()
    fix(core parser)
    fix(core//parser)
    fix(/parser)
    fix(core/)

---

#### 3. Breaking Change Indicator (optional)

Add `!` before the colon to indicate a breaking change:

    feat(core/parser/ast)!: change traversal algorithm

Footers remain supported:

    BREAKING CHANGE: AST nodes are now immutable

---

#### 4. Description (required)

- Short
- Imperative mood
- No trailing period recommended
- Clear and specific

Good:

    fix(ci/tauri/build): update libwebkit2gtk to 4.1

Bad:

    stuff
    fixing things
    update

---

## 🔬 Formal Grammar (EBNF)

    commit        ::= type "(" scope ")" breaking? ":" space description
    type          ::= identifier
    scope         ::= identifier ( "/" identifier )*
    breaking      ::= "!"
    description   ::= text
    identifier    ::= ( letter | digit | "-" )+
    space         ::= " "

---

## ✅ Compatibility

HCC is fully compatible with:

- Conventional Commits parsers
- Semantic versioning automation
- Changelog generators
- Commitlint (with minimal extension)
- CI/CD pipelines

Since `/` is a valid scope character in Conventional Commits,
existing tools will not break.

---

## 📊 Benefits

### 1. Architectural Precision

Commits reflect real system boundaries.

### 2. Advanced Filtering

You can filter by:

- All `ci/*`
- Only `ci/tauri/*`
- Only `core/parser/*`

### 3. Better Changelogs

Structured changelog sections become possible:

- Core
  - Parser
  - AST
- CI
  - Tauri
  - Docker

### 4. Monorepo Scalability

Ideal for:

- Multi-package repositories
- Layered systems
- Plugin ecosystems

---

## 📁 Examples

### Dependency Update

    chore(deps/node/npm): upgrade npm to v10

### CI Change

    fix(ci/github/actions/build): cache rust dependencies

### Feature

    feat(core/compiler/optimizer): add constant folding

### Refactor

    refactor(engine/rendering/pipeline): simplify shader binding logic

### Breaking Change

    feat(api/rest/v2)!: remove legacy authentication

---

## 🚀 Roadmap

- [ ] JSON Schema definition
- [ ] Commitlint plugin
- [ ] ESLint-style linter
- [ ] Changelog generator with hierarchical grouping
- [ ] CLI validator
- [ ] VSCode extension

---

## 🧠 Design Principles

1. Structural clarity over flat labeling
2. Zero ecosystem breakage
3. Architectural alignment
4. Unlimited depth
5. Minimal syntax extension

---

## 📜 License

MIT

---

## ✨ Summary

Hierarchical Conventional Commits brings structural semantics to commit messages.

Flat scopes were enough.

Now they scale.
