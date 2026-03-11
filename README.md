# AI Skills

Agent skill repository for reusable Codex and Agent Skills workflows.

This repo is structured so each skill lives under `skills/` and can be discovered, reviewed, and installed as a self-contained package.

## Repository Structure

```text
skills/
  api-design/
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

## Available Skills

### `api-design`

Design and review API contracts across:

- RESTful HTTP
- GraphQL
- WebSocket
- OpenAPI-first workflows

The skill includes guidance for:

- contract design and review
- authentication and authorization boundaries
- structured error design
- versioning and backward compatibility
- pagination, filtering, and async workflows
- RFC and review templates

Skill path:

- [`skills/api-design/`](./skills/api-design/)

Key files:

- [`skills/api-design/SKILL.md`](./skills/api-design/SKILL.md)
- [`skills/api-design/references/rest.md`](./skills/api-design/references/rest.md)
- [`skills/api-design/references/graphql.md`](./skills/api-design/references/graphql.md)
- [`skills/api-design/references/websocket.md`](./skills/api-design/references/websocket.md)
- [`skills/api-design/references/openapi.md`](./skills/api-design/references/openapi.md)
- [`skills/api-design/references/versioning.md`](./skills/api-design/references/versioning.md)

## Using A Skill

If your agent tooling supports skill repositories, point it at this repo and select a skill from `skills/`.

For local Codex-style usage, the installable unit is the individual skill folder, for example:

- `skills/api-design/`

## Notes

- This repository currently contains one skill and is structured to support more.
- Relative paths inside each skill resolve from that skill's own directory.
