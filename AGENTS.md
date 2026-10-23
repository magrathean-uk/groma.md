# Groma project context

This file describes the project, its concepts and available commands.

## Prototype status

Groma is an early experimental prototype used by its developers. It has no external users or released data, storage, CLI or API contracts. The current filesystem integration is delivery plumbing for a direction that is moving towards a filesystem-independent model.

## Architecture foundations

- Open Knowledge Format (OKF) 0.2 describes readable, linked and portable knowledge through standard metadata, ordinary Markdown and Markdown links. Groma-specific metadata lives under `groma`.
- C4 describes actors, systems, containers, components and relationships. Supporting knowledge and OKF concepts are distinct from C4 elements and containment levels.
- Groma's application profile gives additional meaning to its supported behaviour.
- [The architecture Markdown contract](docs/component-markdown.md) describes the product's Markdown representation.
- The [OKF specification](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md) and [C4 model](https://c4model.com) are the external standards references.
- [Manifesto](MANIFESTO.md#principles-that-hold-across-projects-and-languages) describes reasoning across projects and languages.

Hand-authored architecture Markdown and its rendered view describe the architecture's meaning. The scanner reproduces that meaning from source. Files, directories, imports, line counts, framework internals and containment provide implementation evidence.

## Backlog.md

The project uses Backlog.md for task and project management. CLI references:

```bash
backlog instructions overview
backlog instructions task-creation
backlog instructions task-execution
backlog instructions task-finalization
backlog <command> --help
backlog task view TASK-N --plain
backlog task edit TASK-N --modified-file <existing-path> --modified-file <new-path>
```

`--modified-file` replaces the task's complete modified-file list. The web map places a task's pin on the element owning the newest recorded file. References can contain exact element IDs or repository-relative source paths; modified source files link their current owners.

## Architecture commands

`groma scan`, `groma view`, `groma web` and their watchers can rewrite the source's `groma/` architecture records. A new or renamed source file can become a stand-alone component; relationship rows can also change.

```bash
groma agent-instructions
git status --short groma/
groma edit <owner> --combine <id>
groma edit <owner> --parent <container> --combine <id>
```

`--combine` folds a component into its responsibility owner. `--parent` changes containment. `groma agent-instructions` describes the installed tool's architecture operations.

## Repository checks

`bun run check` combines Biome linting, TypeScript checks and the Node and Bun test suites. Biome uses recommended lint rules and reports cognitive complexity above 15; its formatting and import assist are disabled.

Tests cover scanner inference, ownership, navigation state, projection and layout, camera behaviour, world immutability and lifecycle. Architecture fixtures live in `test/fixtures/`; the live architecture lives in `groma/`. Bun exposes `test.concurrent`; test concurrency within a file differs from isolation and parallel execution between files.

[TUI map validation](docs/viewers/tui/validation.md) describes the `tui-test` procedure.

## Rendering context

The web map has a camera-transformed architecture group. Viewport-sized SVG surfaces with patterns or filters can sit beside that group, avoiding repaint work during pan and zoom. Frame-rate measurements can help investigate rendering regressions.
