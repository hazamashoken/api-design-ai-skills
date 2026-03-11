# AI Skills

A collection of reusable agent skills for coding agents.

This repository currently focuses on API design and review workflows. The skills follow the [Agent Skills](https://agentskills.io/) format and are organized so they can be discovered, installed, and used by compatible tooling.

## Available Skills

### `api-design`

Design and review API contracts across:

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

- [`skills/api-design/SKILL.md`](./skills/api-design/SKILL.md)

Supporting references:

- [`skills/api-design/references/rest.md`](./skills/api-design/references/rest.md)
- [`skills/api-design/references/graphql.md`](./skills/api-design/references/graphql.md)
- [`skills/api-design/references/websocket.md`](./skills/api-design/references/websocket.md)
- [`skills/api-design/references/openapi.md`](./skills/api-design/references/openapi.md)
- [`skills/api-design/references/versioning.md`](./skills/api-design/references/versioning.md)

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

Once installed, the `api-design` skill should activate when the task involves API design or API review.

Example prompts:

```text
Design a REST API for order checkout with idempotency and async payment processing.
```

```text
Review this GraphQL schema for backwards-compatibility and error-handling problems.
```

```text
Compare REST and WebSocket for a collaborative presence feature.
```

## Repository Structure

```text
.claude-plugin/
  marketplace.json
skills/
  api-design/
    .claude-plugin/
      plugin.json
    SKILL.md
    references/
    assets/
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
