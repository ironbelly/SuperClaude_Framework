# Templates Reference

Reference document for Wave 2: Planning & Template Selection. Contains the 4-tier template discovery search paths, version resolution rules, matching criteria, inline generation fallback, milestone count selection, and domain-specific milestone mapping.

---

## 4-Tier Template Discovery

Templates are discovered in priority order. The first tier to produce a match is used; lower tiers are not searched.

### Tier 1: Local (Project-Level)

**Search path**: `.dev/templates/roadmap/` in the current project directory.

**Discovery**: Glob for `*.md` and `*.yaml` files in this directory. Each file must contain YAML frontmatter with at least `name`, `type`, and `domains` fields to be considered a valid template.

### Tier 2: User (User-Level)

**Search path**: `~/.claude/templates/roadmap/`

**Discovery**: Same glob and validation rules as Tier 1.

### Tier 3: Plugin (Marketplace)

**Status**: `[future: v5.0 plugin marketplace — plumb in here when available]`

**Search path**: Will be defined when the plugin marketplace is implemented. Expected pattern: `~/.claude/plugins/*/templates/roadmap/`

**Current behavior**: This tier is always a no-op. Skip to Tier 4.

### Tier 4: Inline Generation (Fallback)

**Trigger**: No template found in Tiers 1-3, OR no template scores >= 0.6 in compatibility scoring, OR `--template` flag specifies a type with no matching file.

**Behavior**: Generate a milestone structure directly from the extraction data using the milestone count selection and domain-specific mapping rules below.

---

## Template File Format

Templates discovered in Tiers 1-3 must follow this format:

```yaml
---
name: <template-name>
type: <feature|quality|docs|security|performance|migration>
domains: [<domain1>, <domain2>]
target_complexity: <0.0-1.0>
min_version: "<semver>"
milestone_count_range: [<min>, <max>]
---
# Template body with milestone structure
```

**Required fields**: `name`, `type`, `domains`
**Optional fields**: `target_complexity` (default 0.5), `min_version` (default "1.0.0"), `milestone_count_range` (default from complexity class)

---

## Version Resolution Rules

When multiple template files match the same `type`:
1. Filter: exclude templates where `min_version` > current sc:roadmap version
2. Score: apply template compatibility scoring (see `refs/scoring.md`)
3. Select: highest-scoring template wins
4. Tie-break: if scores are equal, prefer Tier 1 over Tier 2 (local project customization wins)

---

## Matching Criteria

A template is considered a **candidate** if:
1. Its `type` field matches the spec's dominant requirement type OR the user's `--template` flag value
2. Its `min_version` is <= current sc:roadmap version
3. It has valid YAML frontmatter with required fields

Candidates are then scored using the template compatibility formula from `refs/scoring.md`. Only templates scoring >= 0.6 are selected.

---

## Inline Template Generation Fallback

When no template scores >= 0.6 (or no templates exist), generate the milestone structure algorithmically.

### Milestone Count Selection

| Complexity Class | Milestone Count | Rationale |
|-----------------|----------------|-----------|
| LOW (< 0.4) | 3-4 | Simple scope, few dependencies |
| MEDIUM (0.4-0.7) | 5-7 | Moderate scope, cross-domain work |
| HIGH (> 0.7) | 8-12 | Complex scope, many dependencies |

**Exact count within range**: `base + floor(domain_count / 2)`
- LOW: base = 3
- MEDIUM: base = 5
- HIGH: base = 8
- `domain_count` = number of domains with >= 10% representation

### Domain-Specific Milestone Mapping

Each domain detected with >= 10% representation generates at least one dedicated milestone. The milestone type and focus depends on the domain.

| Domain | Milestone Type | Typical Focus |
|--------|---------------|---------------|
| frontend | FEATURE | UI components, user flows, accessibility |
| backend | FEATURE | API endpoints, data models, service logic |
| security | SECURITY | Authentication, authorization, threat mitigation |
| performance | IMPROVEMENT | Optimization, caching, load testing |
| documentation | DOC | User guides, API docs, architecture docs |

### Milestone Generation Algorithm

1. **Foundation milestone** (always M1): Project setup, dependencies, architecture decisions
2. **Domain milestones**: One per domain with >= 10%, ordered by domain percentage (highest first)
3. **Integration milestone**: If domain_count >= 2, add an integration milestone after domain milestones
4. **Validation milestone**: Final milestone for end-to-end validation and acceptance testing

**Validation milestone interleaving**: Based on interleave ratio from complexity class (see `refs/scoring.md`):
- LOW (1:3): Insert validation milestone after every 3 work milestones
- MEDIUM (1:2): Insert validation milestone after every 2 work milestones
- HIGH (1:1): Insert validation milestone after every work milestone

### Required Sections Per Milestone

Every generated milestone must include these sections (matching the roadmap.md body template):

1. **Objective**: 1-2 sentence goal statement
2. **Deliverables**: Table with ID, description, acceptance criteria
3. **Dependencies**: List of prerequisite milestones or "None"
4. **Risk Assessment**: Table with risk, probability, impact, mitigation

### Dependency Mapping Rules

- M1 (Foundation) has no dependencies
- Domain milestones depend on M1
- Domain milestones for related domains may have inter-dependencies (e.g., backend milestone blocks frontend milestone if frontend requires API endpoints)
- Integration milestones depend on all domain milestones they integrate
- Validation milestones depend on the work milestones they validate

---

*Reference document for sc:roadmap v2.0.0 — loaded on-demand during Wave 2*
