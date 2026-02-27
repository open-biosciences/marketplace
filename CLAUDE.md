# CLAUDE.md — marketplace

## Session Priming (REQUIRED)

Session priming is required before any work in this repo. See the workspace-level
[`../CLAUDE.md`](../CLAUDE.md) for the mandatory Graphiti queries.

## Purpose

Plugin marketplace packaging all Open Biosciences platform assets into Anthropic's `.claude-plugin/` format for community distribution. Owned by **Quality & Skills Engineer** (Agent 8).

## Key Files

| File | Purpose |
|------|---------|
| `.claude-plugin/marketplace.json` | Central registry — 15 plugin entries |
| `CONNECTORS.md` | Database documentation for all 12 life sciences APIs |
| `hgnc/` through `clinicaltrials/` | 12 MCP server plugin directories |
| `domain-skills/` | 6 life sciences research skills |
| `platform-tools/` | FastMCP scaffolding commands + security review |
| `speckit/` | 9 SpecKit SDLC commands |

## Conventions

- This repo contains **no code** — only JSON manifests, markdown skills/commands, and documentation
- All MCP server plugins point to the single gateway URL: `https://biosciences-mcp.fastmcp.app/mcp`
- Skills and commands are **copied** (not symlinked) from source repos for standalone distribution
- Plugin manifests follow the `.claude-plugin/plugin.json` format

## Source Repos

| Plugin | Source |
|--------|--------|
| 12 MCP servers | `biosciences-mcp` (gateway.py tool mappings) |
| domain-skills | `biosciences-skills` (.claude/skills/biosciences-*) |
| platform-tools | `platform-skills` (.claude/commands + .claude/skills) |
| speckit | `biosciences-program` (.claude/commands/speckit.*) |
