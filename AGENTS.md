# AGENTS Instructions

This repository is a monorepo. Each library lives in a subdirectory under `libs/`.

## Repository overview

LangGraph is a low-level orchestration framework for building, managing, and deploying long-running, stateful agents. The repository contains several Python and JavaScript/TypeScript libraries.

Main libraries:

- **checkpoint** – base interfaces for LangGraph checkpointers.
- **checkpoint-postgres** – Postgres implementation of the checkpoint saver.
- **checkpoint-sqlite** – SQLite implementation of the checkpoint saver.
- **cli** – official command-line interface for LangGraph.
- **langgraph** – core framework for building stateful, multi-actor agents.
- **prebuilt** – high-level APIs for creating and running agents and tools.
- **sdk-js** – JavaScript/TypeScript SDK for interacting with the LangGraph REST API.
- **sdk-py** – Python SDK for the LangGraph Platform API.

## Working rules for Codex

- Keep changes small and focused.
- Do not refactor unrelated files.
- Preserve existing public APIs unless the task explicitly asks for an API change.
- Before adding a new dependency, explain why it is needed.
- Prefer following the existing style in the nearest package.
- Do not commit secrets, API keys, tokens, `.env` files, or generated credentials.
- When unsure, inspect nearby tests and existing implementation patterns before changing code.

## Python libraries

Most Python packages live under `libs/`.

When modifying a Python library, run these commands from that library directory before opening a pull request:

```bash
make format
make lint
make test
```

To run a specific test file:

```bash
TEST=path/to/test.py make test
```

Other pytest arguments can also be passed through the `TEST` variable.

Python style expectations:

- Use `uv` for Python environment and command execution when existing commands use it.
- Follow Ruff formatting rules.
- Keep line length and import style consistent with existing package configuration.
- Prefer `typing_extensions.TypedDict` instead of `typing.TypedDict`.
- Add or update tests when changing behavior.

## JavaScript/TypeScript libraries

The JavaScript/TypeScript SDK lives under `libs/sdk-js`.

When modifying `libs/sdk-js`, use the package scripts from that directory:

```bash
yarn format
yarn lint
yarn test
```

JS/TS style expectations:

- Preserve ESM package conventions.
- Keep exported API compatibility unless the task explicitly asks for a breaking change.
- Update types when changing public behavior.
- Add or update Vitest tests when changing behavior.

## Review guidelines

When reviewing pull requests, focus on serious issues first.

Flag as high priority:

- Incorrect state handling or broken graph execution behavior.
- Bugs that can corrupt checkpoints, persistence, memory, or resume behavior.
- Backward-incompatible API changes that are not clearly intentional.
- Missing tests for behavior changes.
- Type errors or lint failures that would block CI.
- Security issues such as leaking secrets, unsafe token handling, or logging sensitive data.
- Changes that affect downstream libraries without updating the dependent code or tests.

Do not over-comment on minor style issues if formatters or linters already handle them.

## Dependency impact map

Changes to a library may affect its dependents:

```text
checkpoint
├── checkpoint-postgres
├── checkpoint-sqlite
├── prebuilt
└── langgraph

prebuilt
└── langgraph

sdk-py
├── langgraph
└── cli

sdk-js (standalone)
```

When modifying a dependency, consider whether dependent libraries also need tests or updates.
