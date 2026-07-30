---
name: Build a Risk Cloud workflow with steps and paths
description: Create an Application, add a Workflow, populate it with Steps, and connect those Steps with Paths using the Risk Cloud API v2.
api: openapi/logicgate-risk-cloud-openapi-original.json
operations:
  - createApplication
  - createWorkflow
  - createStep
  - createEdgePath
  - createNextPath
  - readWorkflow
---

# Build a Risk Cloud workflow

Use the Risk Cloud API v2 to stand up a workflow structure programmatically.

## Prerequisites
- The paid **API Access** add-on must be enabled on the environment.
- Obtain a bearer token: `POST /api/v1/account/token` with `Authorization: Basic base64(client:secret)`. Use the returned `access_token` as `Authorization: Bearer {token}` on every call (see `authentication/logicgate-authentication.yml`).
- Base host: `https://{env}.logicgate.com` (your tenant environment).

## Steps
1. **Create the application** — `createApplication` (`POST /api/v2/applications`). Capture the returned application `id`.
2. **Create a workflow** — `createWorkflow` (`POST /api/v2/workflows`), referencing the application id. Capture the workflow `id`.
3. **Add steps** — `createStep` (`POST /api/v2/steps`) once per step in the workflow, referencing the workflow id.
4. **Connect steps with paths** — use `createEdgePath` (`POST /api/v2/paths/edge`) and/or `createNextPath` (`POST /api/v2/paths/next`) to wire the ordered flow between the step ids.
5. **Verify** — `readWorkflow` (`GET /api/v2/workflows/{id}`) to confirm the assembled structure.

## Rules
- Responses to list calls are paged (`page`/`size`, zero-indexed) with a `PageModelOut` envelope — see `conventions/logicgate-conventions.yml`.
- On `400` the body identifies the offending `property`/`value`/`message`; `403` usually means the token lacks the add-on or permission; `404` means a referenced id does not exist — see `errors/logicgate-problem-types.yml`.
- There is no idempotency-key support; guard retries yourself to avoid duplicate creates.
