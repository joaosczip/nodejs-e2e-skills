# Logging fundamentals

Source:
- https://node.testcontainers.org/quickstart/logging/

Testcontainers uses the `debug` logger. Enable logs only when troubleshooting to keep test output clean.

## Enable all logs

```bash
DEBUG=testcontainers* npm test -- test/app.e2e-spec.ts
```

## Enable focused namespaces

```bash
DEBUG=testcontainers,testcontainers:containers,testcontainers:exec npm test -- -t "creates a transaction"
```

## Common namespaces

- `testcontainers*`: all logs
- `testcontainers`: core runtime logs
- `testcontainers:containers`: container lifecycle
- `testcontainers:compose`: compose lifecycle
- `testcontainers:build`: Docker image build logs
- `testcontainers:pull`: image pull logs
- `testcontainers:exec`: in-container command execution

## Guidance

- Start with `testcontainers:containers` for startup failures.
- Add `testcontainers:pull` when CI fails on image pulls.
