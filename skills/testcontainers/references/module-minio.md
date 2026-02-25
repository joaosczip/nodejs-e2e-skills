# MinIO module (`@testcontainers/minio`)

Source:
- https://node.testcontainers.org/modules/minio/

Use this module for S3-compatible object storage tests when you do not need broader AWS emulation.

## Install

```bash
npm install --save-dev @testcontainers/minio
npm install minio
```

## Upload and verify object

```ts
import { MinioContainer } from "@testcontainers/minio";
import { Client } from "minio";

const minio = await new MinioContainer("minio/minio:RELEASE.2024-08-03T04-33-23Z").start();

const client = new Client({
  endPoint: minio.getHost(),
  port: minio.getPort(),
  useSSL: false,
  accessKey: "minioadmin",
  secretKey: "minioadmin",
});

await client.makeBucket("test-bucket");
await client.putObject("test-bucket", "hello.txt", "hello from e2e");
const stat = await client.statObject("test-bucket", "hello.txt");

expect(stat.size).toBeGreaterThan(0);

await minio.stop();
```

## Custom credentials

```ts
const minio = await new MinioContainer("minio/minio:RELEASE.2024-08-03T04-33-23Z")
  .withUsername("test-user")
  .withPassword("test-pass-123")
  .start();
```

## Notes

- MinIO is ideal for object-storage integration tests without AWS-specific APIs.
- Use deterministic bucket/object names per suite to simplify cleanup and assertions.
