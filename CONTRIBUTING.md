# Contributing

Guidelines for adding plugins to the Open Biosciences Marketplace.

---

## Adding a New MCP Server Plugin

1. **Create a directory** at the repo root named after the database (lowercase):

```
newserver/
└── .claude-plugin/
    └── plugin.json
```

2. **Write the plugin.json manifest**:

```json
{
  "name": "newserver",
  "version": "1.0.0",
  "description": "Brief description — what the database provides and what CURIEs it uses",
  "author": { "name": "Open Biosciences" },
  "repository": "https://github.com/open-biosciences/biosciences-mcp",
  "mcp": {
    "url": "https://biosciences-mcp.fastmcp.app/mcp"
  },
  "tools": [
    {
      "name": "newserver_search_entities",
      "description": "Fuzzy search description. Returns ranked candidates with CURIEs."
    },
    {
      "name": "newserver_get_entity",
      "description": "Strict lookup description. Requires resolved CURIE."
    }
  ]
}
```

3. **Add the entry to `.claude-plugin/marketplace.json`** in the `plugins` array:

```json
{
  "name": "newserver",
  "source": "./newserver",
  "description": "Same description as plugin.json",
  "category": "life-sciences",
  "tags": ["relevant", "tags"]
}
```

4. **Add the database to `CONNECTORS.md`** under the appropriate section with base URL, auth, rate limits, and CURIE format.

5. **The server implementation** belongs in [biosciences-mcp](https://github.com/open-biosciences/biosciences-mcp), not this repo. This marketplace contains only manifests and documentation.

---

## Adding a New Skill

1. **Choose the appropriate plugin bundle**:
   - `domain-skills/` — Research-facing skills (e.g., a new biosciences domain)
   - `platform-tools/` — Developer-facing tools (e.g., scaffolding, review)
   - `speckit/` — SpecKit SDLC commands only

2. **Create the skill file** following the existing pattern:

```
domain-skills/skills/biosciences-newdomain/SKILL.md
```

3. **Update the plugin's README.md** to include the new skill in the table.

4. **Copy, don't symlink** — skills in this marketplace must be self-contained. Copy the file from its source repo so the marketplace can be cloned and used independently.

---

## plugin.json Schema Reference

### MCP Server Plugin

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Lowercase identifier matching the directory name |
| `version` | Yes | Semver version string |
| `description` | Yes | One-line description of the database and its CURIE format |
| `author.name` | Yes | `"Open Biosciences"` for org plugins |
| `repository` | Yes | GitHub URL of the source repo |
| `mcp.url` | Yes | Gateway URL: `https://biosciences-mcp.fastmcp.app/mcp` |
| `tools` | Yes | Array of `{ name, description }` objects |

### Skill Bundle Plugin

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Bundle identifier (`domain-skills`, `platform-tools`, `speckit`) |
| `version` | Yes | Semver version string |
| `description` | Yes | One-line description of the bundle contents |
| `author.name` | Yes | `"Open Biosciences"` for org plugins |
| `repository` | Yes | GitHub URL of the source repo for the skill content |

---

## PR Process

1. Fork the repository
2. Create a feature branch (`feat/add-newserver-plugin`)
3. Add your plugin files following the patterns above
4. Verify: `find . -name 'plugin.json' | wc -l` should increase by 1
5. Verify: `grep -r '/home/' . --include='*.json'` should return empty (no private paths)
6. Open a PR with a clear description of the database or skill being added

---

## Design Principles

- **No code in this repo** — only JSON manifests, markdown, and documentation
- **Copy, don't symlink** — the marketplace must be self-contained
- **Single gateway URL** — all MCP server plugins point to the unified gateway
- **Fuzzy-to-Fact compliance** — every server must implement fuzzy search + strict lookup
