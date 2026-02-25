# AGENTS Guide

This file defines how coding agents should work in `nodejs-e2e-skills`.

## Repository Intent

- This repository is a skill library for coding agents.
- Primary domain: Node.js end-to-end testing workflows.
- Current content is documentation-first (Markdown skills + references).
- The repo does not currently include a runnable Node.js project.

## Repository Layout

- `README.md`: high-level project intention.
- `skills/<skill-name>/SKILL.md`: canonical instructions for one skill.
- `skills/<skill-name>/references/*.md`: supporting references and templates.
- Add new skills under `skills/` with a focused, reusable scope.

## Cursor/Copilot Rules

- `.cursor/rules/`: not present at time of writing.
- `.cursorrules`: not present at time of writing.
- `.github/copilot-instructions.md`: not present at time of writing.
- If any of these files are later added, treat them as higher-priority constraints.

## Build, Lint, and Test Commands

Because this repo is documentation-focused, there is no required build/lint/test pipeline yet.

### Current status (in this repository)

- Build: none configured.
- Lint: none configured.
- Test: none configured.

### Optional local validation commands

Run these only if the tools are installed in your environment:

- Markdown lint (entire repo): `npx markdownlint-cli "**/*.md"`
- Markdown lint (single file): `npx markdownlint-cli "skills/testcontainers/SKILL.md"`
- Link check (entire repo): `npx markdown-link-check README.md`
- Spell check (optional): `npx cspell "**/*.md"`

### Single-test command guidance (for skill examples)

When writing commands inside skills, include at least one single-test example.
Prefer explicit file-path execution.

- Jest (single file): `npm test -- test/app.e2e-spec.ts`
- Jest (single test name): `npm test -- -t "creates a transaction"`
- Vitest (single file): `npx vitest run test/app.e2e-spec.ts`
- Vitest (single test name): `npx vitest run -t "creates a transaction"`
- Playwright (single file): `npx playwright test tests/checkout.spec.ts`
- Playwright (single test): `npx playwright test -g "guest checkout"`
- Cypress (single spec): `npx cypress run --spec cypress/e2e/checkout.cy.ts`

## Documentation Style Rules

These rules apply to all `README.md`, `SKILL.md`, and reference docs.

### Writing style

- Be operational and imperative ("Do X", "Avoid Y").
- Prefer concise paragraphs and short bullet lists.
- Keep claims specific and testable.
- Avoid marketing language and vague adjectives.
- Prefer practical defaults over exhaustive theory.

### Markdown structure

- Use one H1 per file.
- Use H2/H3 headings in a logical progression.
- Keep sections scannable with bullets.
- Use fenced code blocks with language tags (for example `bash`, `ts`).
- Use inline code for paths, package names, and commands.

### Frontmatter conventions for `SKILL.md`

Each `SKILL.md` must start with YAML frontmatter:

- `name`: kebab-case skill identifier.
- `description`: one sentence, concrete scope, includes target stack.

Example:

```yaml
---
name: testcontainers-e2e
description: Arranges JS/TS backend e2e tests with Testcontainers for isolated infra.
---
```

### Skill content template

Each skill should include these sections in order:

1. Purpose and when to use.
2. Scope and non-goals.
3. Mandatory behavior/rules.
4. Step-by-step workflow.
5. Implementation rules and defaults.
6. Troubleshooting checklist.
7. References.

## Technical Content Conventions

### Imports and code snippets

- Use ESM-style imports in snippets unless a tool requires CJS.
- Keep import lists minimal and relevant to the example.
- Prefer named imports when clarity improves readability.
- Do not include unused imports in examples.

### Type usage

- Prefer explicit types in non-trivial TypeScript examples.
- Use concrete container/test framework types when available.
- Avoid `any`; use framework-provided types.
- Show nullable cleanup patterns when resources may be undefined.

### Naming conventions

- File/folder names: kebab-case.
- Skill identifiers: kebab-case.
- Test files in examples: `*.e2e-spec.ts` (Jest/Nest style) unless context differs.
- Variable names: descriptive (`postgresContainer`, `moduleFixture`, `response`).
- Function names: action-oriented (`startPostgresContainer`, `resetDatabaseState`).

### Error handling and reliability guidance

- Fail fast on infrastructure startup errors.
- Never swallow container startup/teardown failures.
- Always show cleanup in `afterAll`/equivalent hooks.
- Prefer deterministic waits and health checks over hardcoded sleeps.
- Do not hardcode host ports when using containers.
- Surface actionable troubleshooting steps for common failures.

## Agent Behavior Expectations

- Preserve the repository's documentation-first nature.
- Keep changes narrowly scoped to the requested skill/problem.
- When editing a skill, update references if behavior changes.
- If adding new commands, verify they are valid for the named tool.
- Do not invent repository scripts that do not exist.
- If no runnable project exists, clearly state assumptions.

## Quality Checklist Before Finishing

- Frontmatter exists and is valid YAML (for `SKILL.md`).
- Commands are copy-paste ready.
- At least one single-test command is documented where relevant.
- Examples use deterministic setup and explicit cleanup.
- Terminology is consistent across skill and references.
- No contradictions between `SKILL.md` and reference docs.

## When Adding New Skills

- Create `skills/<new-skill>/SKILL.md` first.
- Add at least one `references/*.md` document.
- Keep skill scope narrow; split if it becomes multi-domain.
- Include framework-specific commands only when verified.
- Prefer patterns that are CI-friendly and reproducible locally.

## Change Management Notes

- This file should evolve with the repository.
- If build/lint/test tooling is introduced, update command sections immediately.
- If Cursor/Copilot rule files are added, mirror key constraints here.
