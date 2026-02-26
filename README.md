# nodejs-e2e-skills

`nodejs-e2e-skills` is a documentation-first repository of reusable skills for coding agents working on Node.js end-to-end testing.

## What this repository provides

- Practical `SKILL.md` playbooks for common e2e testing scenarios.
- Service-specific references with copy-paste-ready examples.
- Consistent patterns for setup, teardown, assertions, and troubleshooting.

## Available skills

### `testcontainers-e2e`

Path: `skills/testcontainers/SKILL.md`

Helps agents design reliable JS/TS backend e2e suites using Testcontainers with:

- Fundamentals: containers, images, networking, compose, logging.
- Service modules: LocalStack, MinIO, MySQL, NATS, PostgreSQL, RabbitMQ, Redis.
- Suite lifecycle examples (`beforeAll`, `afterAll`, runtime config wiring, expectations).

## Repository structure

- `README.md`: high-level overview and skill index.
- `skills/<skill-name>/SKILL.md`: canonical instructions for one skill.
- `skills/<skill-name>/references/*.md`: focused references and implementation examples.

## Notes

- This repo currently has no runnable Node.js app.
- Examples are designed to be adapted into downstream projects.
