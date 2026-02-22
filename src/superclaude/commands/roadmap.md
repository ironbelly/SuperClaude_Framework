---
name: sc:roadmap
description: Generate comprehensive project roadmaps from specification documents
allowed-tools: Read, Glob, Grep, Edit, Write, Bash, TodoWrite, Task
---

# /sc:roadmap — Roadmap Generator

## Trigger

When the user requests roadmap generation from a specification file. Requires a specification file path as mandatory input.

## Usage

```
/sc:roadmap <spec-file-path> [options]
/sc:roadmap --specs <spec1.md,spec2.md,...> [options]
```

## Core Flags

| Flag | Default | Description |
|------|---------|-------------|
| `--depth` | `standard` | Analysis depth: quick, standard, deep |
| `--template` | Auto-detect | Template type: feature, quality, docs, security, performance, migration |
| `--output` | `.dev/releases/current/<spec-name>/` | Output directory |
| `--specs` | - | Comma-separated spec paths for multi-spec consolidation |
| `--multi-roadmap` | `false` | Enable multi-roadmap adversarial generation |
| `--agents` | - | Agent specs for multi-roadmap: `model[:persona[:"instruction"]]` |
| `--interactive` | `false` | User approval at adversarial decision points |
| `--no-validate` | `false` | Skip Wave 4 validation |
| `--compliance` | Auto-detect | Force compliance tier: strict, standard, light |
| `--persona` | Auto-select | Override primary persona |
| `--dry-run` | `false` | Preview structure without writing files |

## Activation

Load and execute the full behavioral instructions from `src/superclaude/skills/sc-roadmap/SKILL.md`.

## Boundaries

- Requires specification file input — will not generate from ad-hoc descriptions
- Produces planning artifacts only — does not execute implementation
- Does not trigger downstream commands — user manually proceeds
