# Networking fundamentals

Source:
- https://node.testcontainers.org/features/networking/

Use networking primitives when containers must communicate directly (broker + worker, DB + migration sidecar, etc.).

## Shared network with alias

```ts
import { GenericContainer, Network } from "testcontainers";

const network = await new Network().start();

const redis = await new GenericContainer("redis:7.4-alpine")
  .withExposedPorts(6379)
  .withNetwork(network)
  .withNetworkAliases("redis")
  .start();

const app = await new GenericContainer("alpine:3.20")
  .withCommand(["sh", "-c", "apk add --no-cache redis && redis-cli -h redis ping"])
  .withNetwork(network)
  .start();

await app.stop();
await redis.stop();
await network.stop();
```

## Expose host port to containers

```ts
import { GenericContainer, TestContainers } from "testcontainers";

await TestContainers.exposeHostPorts(3001);

const container = await new GenericContainer("curlimages/curl:8.10.1")
  .withCommand(["sleep", "infinity"])
  .start();

const result = await container.exec([
  "curl",
  "-s",
  "http://host.testcontainers.internal:3001/health",
]);

expect(result.exitCode).toBe(0);
```

## Guidance

- Prefer aliases on a custom network for container-to-container DNS.
- Avoid relying on container IP addresses directly.
