---
name: Read and export Risk Cloud records
description: Page through Records for a workflow, read an individual record and its linked records, and enumerate the fields behind them using the Risk Cloud API v2.
api: openapi/logicgate-risk-cloud-openapi-original.json
operations:
  - readAllRecords
  - readRecord
  - readAllLinkedRecords
  - readAllFields
  - readAllAccessAudits
---

# Read and export Risk Cloud records

Use the Risk Cloud API v2 to extract record data for reporting, sync, or audit.

## Prerequisites
- Bearer token as in the auth profile (`authentication/logicgate-authentication.yml`); API Access add-on required.

## Steps
1. **List records** — `readAllRecords` (`GET /api/v2/records`). Page with `page`/`size`; follow `links.next` in the `PageModelOut` envelope until exhausted.
2. **Read a record** — `readRecord` (`GET /api/v2/records/{id}`) for full field values on a single record.
3. **Follow relationships** — `readAllLinkedRecords` (`GET /api/v2/records/{id}/linked`) to traverse records linked across workflows.
4. **Resolve fields** — `readAllFields` (`GET /api/v2/fields`) to map field ids to labels/types when building an export schema.
5. **Audit access (optional)** — `readAllAccessAudits` (`GET /api/v2/audits/access`) to capture who accessed what for compliance evidence.

## Rules
- Everything is read-only here; these are `GET` operations classified `connected`/`read` in `agentic-access/logicgate-agentic-access.yml`.
- Honor pagination — never assume a single page; `page.totalPages` tells you when to stop.
- See `data-model/logicgate-data-model.yml` for how Records relate to Workflows and Fields.
