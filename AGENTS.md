# AGENTS Guide

This repository stores reusable documentation skills for coding agents.

## Repository intent

- Primary domain: Node.js e2e testing workflows.
- Content type: Markdown skills and references.
- No runnable application is included in this repository.

## Repository layout

- `README.md`: repository overview and available skills.
- `skills/<skill-name>/SKILL.md`: canonical skill playbook.
- `skills/<skill-name>/references/*.md`: focused reference docs with examples.

## Authoring rules

- Keep docs operational and concise.
- Prefer concrete defaults over generic advice.
- Every code snippet should be realistic and runnable with minimal adaptation.
- Use fenced code blocks with language tags (`bash`, `ts`).
- Use inline code for commands, paths, and package names.

## Skill conventions

- `SKILL.md` must start with YAML frontmatter:
  - `name`: kebab-case identifier.
  - `description`: one sentence describing concrete scope.
- Keep skill scope narrow and reusable.
- If behavior changes, update references in the same change.

## Quality checklist

- Instructions and examples are consistent across `SKILL.md` and references.
- Commands are copy-paste ready.
- Test examples show explicit setup and teardown.
- Avoid hardcoded host ports in containerized examples.
- Troubleshooting guidance is actionable.
