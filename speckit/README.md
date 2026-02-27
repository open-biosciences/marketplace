# SpecKit

Nine specification-driven development commands implementing the SpecKit SDLC workflow (ADR-003).

## Commands

| Command | Stage | Description |
|---------|-------|-------------|
| `speckit.constitution` | Setup | Create or update project principles |
| `speckit.specify` | Specification | Create feature specification from requirements |
| `speckit.clarify` | Specification | Surface underspecified areas with targeted questions |
| `speckit.plan` | Design | Create implementation plan from specification |
| `speckit.tasks` | Planning | Generate dependency-ordered task list |
| `speckit.analyze` | Quality | Cross-artifact consistency check |
| `speckit.implement` | Execution | Execute bounded implementation tasks |
| `speckit.checklist` | Quality | Generate custom feature checklist |
| `speckit.taskstoissues` | Tracking | Convert tasks to GitHub issues |

## Workflow

```
constitution -> specify -> [clarify] -> plan -> tasks -> [analyze] -> implement
```

Commands in brackets are optional quality gates.

## Source

Copied from [`biosciences-program`](https://github.com/open-biosciences/biosciences-program) `.claude/commands/` directory.

## License

MIT — see [LICENSE](../LICENSE) in the repository root.
