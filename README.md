# AI Skills

A collection of reusable agent skills for coding agents.

This repository currently focuses on API design and review workflows. The skills follow the [Agent Skills](https://agentskills.io/) format and are organized so they can be discovered, installed, and used by compatible tooling.

## Available Skills

### `api-design-principles`

Framework-agnostic API design doctrine for:

- RESTful HTTP
- GraphQL
- WebSocket
- OpenAPI-first workflows

Use it when you need help with:

- choosing the right API style
- designing routes, operations, schemas, and message envelopes
- authentication and authorization boundaries
- structured errors and status handling
- versioning and backward compatibility
- pagination, filtering, and async workflows
- API RFCs and review checklists

Main entry point:

- [`skills/api-design-principles/SKILL.md`](./skills/api-design-principles/SKILL.md)

Supporting references:

- [`skills/api-design-principles/references/rest.md`](./skills/api-design-principles/references/rest.md)
- [`skills/api-design-principles/references/graphql.md`](./skills/api-design-principles/references/graphql.md)
- [`skills/api-design-principles/references/websocket.md`](./skills/api-design-principles/references/websocket.md)
- [`skills/api-design-principles/references/openapi.md`](./skills/api-design-principles/references/openapi.md)
- [`skills/api-design-principles/references/versioning.md`](./skills/api-design-principles/references/versioning.md)

### `api-design-drf`

Django REST Framework adapter for implementing the chosen API contract with:

- serializers
- views and viewsets
- permissions
- pagination, filtering, and ordering
- versioning
- drf-spectacular
- contract-focused API tests

Main entry point:

- [`skills/api-design-drf/SKILL.md`](./skills/api-design-drf/SKILL.md)

Supporting references:

- [`skills/api-design-drf/references/drf.md`](./skills/api-design-drf/references/drf.md)
- [`skills/api-design-drf/references/view-selection.md`](./skills/api-design-drf/references/view-selection.md)
- [`skills/api-design-drf/references/serializers.md`](./skills/api-design-drf/references/serializers.md)
- [`skills/api-design-drf/references/permissions.md`](./skills/api-design-drf/references/permissions.md)
- [`skills/api-design-drf/references/errors.md`](./skills/api-design-drf/references/errors.md)
- [`skills/api-design-drf/references/queries.md`](./skills/api-design-drf/references/queries.md)
- [`skills/api-design-drf/references/schema.md`](./skills/api-design-drf/references/schema.md)
- [`skills/api-design-drf/references/testing.md`](./skills/api-design-drf/references/testing.md)

## Installation

To install this repository with compatible Agent Skills tooling:

```bash
npx skills add hazamashoken/api-design-ai-skills
```

To inspect the repository before installing:

```bash
npx skills add hazamashoken/api-design-ai-skills -l
```

For tools that support plugin marketplaces, this repo also includes:

- [`.claude-plugin/marketplace.json`](./.claude-plugin/marketplace.json)
- [`skills/api-design/.claude-plugin/plugin.json`](./skills/api-design/.claude-plugin/plugin.json)

## Usage

Once installed, these skills should activate when the task involves API contract design or DRF implementation details.

Example prompts:

```text
Design a REST API for order checkout with idempotency and async payment processing.
```

```text
Review this GraphQL schema for backwards-compatibility and error-handling problems.
```

```text
Map this API contract into DRF serializers, permissions, and drf-spectacular schema output.
```

## Repository Structure

```text
.claude-plugin/
  marketplace.json
skills/
  api-design-principles/
    .claude-plugin/
      plugin.json
    SKILL.md
    references/
    assets/
    agents/
  api-design-drf/
    .claude-plugin/
      plugin.json
    SKILL.md
    references/
    agents/
```

Each skill folder contains:

- `SKILL.md`: the core skill instructions and trigger description
- `references/`: deeper guidance loaded only when needed
- `assets/`: reusable templates and supporting files
- `agents/`: product-specific metadata such as `openai.yaml`

## Notes

- This repository is structured to support additional skills over time.
- Relative paths inside each skill resolve from that skill's own directory.
