---
name: roadmap
description: "Generate comprehensive project roadmaps from specification documents"
category: planning
complexity: advanced
mcp-servers: [sequential, context7, serena]
personas: [architect, scribe, analyzer]
---

# /sc:roadmap - Roadmap Generator

## Required Input
- Specification file: $ARGUMENTS
- File exists: !`test -f ${ARGUMENTS%% *} && echo "YES" || echo "NO - file not found"`
- File type: !`file --brief ${ARGUMENTS%% *} 2>/dev/null || echo "unknown"`
- File size: !`wc -l < ${ARGUMENTS%% *} 2>/dev/null || echo "0"` lines

## Usage
```
/sc:roadmap <spec-file-path> [--template feature|quality|docs|security|performance|migration] [--depth quick|standard|deep] [--output <dir>]
```

### Arguments
- **spec-file-path**: (MANDATORY) Path to specification document (.md, .txt, .yaml, .json)
- **--template**: Template type for roadmap generation (default: auto-detect from spec content)
- **--depth**: Analysis depth - `quick` (overview), `standard` (full), `deep` (with validation)
- **--output**: Output directory (default: `.dev/releases/current/<spec-name>/`)
- **--dry-run**: Preview roadmap structure without generating files
- **--compliance**: Force compliance tier (`strict`, `standard`, `light`)

## Behavioral Summary

5-wave orchestration: Wave 0 (prerequisite validation and spec parsing), Wave 1 (domain analysis and template selection), Wave 2 (milestone extraction and dependency mapping), Wave 3 (tasklist generation with effort estimates), Wave 4 (multi-agent validation and quality gates). Requires specification file as mandatory input. Outputs milestone-based roadmap package to `.dev/releases/current/` directory with tasklists, dependency graphs, and risk assessments.

## Examples

### Generate Roadmap from Feature Spec
```
/sc:roadmap specs/auth-system.md
```
Parses the specification, auto-detects domain (security), selects appropriate template, and generates a complete roadmap package with milestones, tasklists, and validation results.

### Deep Analysis with Security Template
```
/sc:roadmap specs/migration-plan.md --template security --depth deep
```
Runs deep analysis with security-focused template. Includes threat modeling milestones and compliance validation gates.

### Quick Preview
```
/sc:roadmap specs/quick-fix.md --dry-run
```
Shows roadmap structure and milestone breakdown without writing files.

## Boundaries

**Will:**
- Parse specification documents and extract actionable requirements
- Generate milestone-based roadmaps with dependency mapping and effort estimates
- Validate roadmap completeness using multi-agent quality gates
- Write roadmap artifacts to `.dev/releases/current/` directory

**Will Not:**
- Execute without a specification file — input is mandatory
- Generate roadmaps from ad-hoc descriptions without documented requirements
- Modify the source specification file
- Implement the roadmap tasks — output is planning artifacts only

## CRITICAL BOUNDARIES

**SPECIFICATION-DRIVEN ROADMAP GENERATION**

**Explicitly Requires**:
- A valid specification file path as first argument
- File must exist and be readable

**Output**: Roadmap artifacts written to `.dev/releases/current/<spec-name>/` directory:
- Milestone breakdown with dependency graph
- Per-milestone tasklists with effort estimates
- Risk assessment and mitigation strategies
- Validation report (if --depth deep or --validate)

**Next Step**: Review generated roadmap, then use `/sc:task` to begin executing milestones.
