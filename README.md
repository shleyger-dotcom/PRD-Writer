# PRD — spec writer for Claude Code

Turns a raw idea into a `PRD.md` that an AI coder can build from without
guessing. Claude interviews you one question at a time. Each question comes
with a recommended answer and alternatives. Claude flags weak spots and
records every decision. `/prd pack` adds `data-model.md`, `architecture.md`,
`adr/001-*.md` and `critique.md`.

## Installation

Requires [Claude Code](https://claude.com/claude-code). Run in PowerShell,
cmd or Git Bash (no admin rights needed):

```
claude plugin marketplace add shleyger-dotcom/PRD
claude plugin install prd@prd
```

Restart open Claude Code sessions to load the plugin.

Update:

```
claude plugin marketplace update prd
claude plugin update prd@prd
```

Uninstall:

```
claude plugin uninstall prd@prd
```

## Usage

Open Claude Code in your project folder and run:

| Command | Result |
|---|---|
| `/prd` | Interview → `PRD.md` |
| `/prd <idea>` | Same, starting from your idea |
| `/prd pack` | `PRD.md` + data model, architecture, ADR, critique |

The first question sets the language of the questions. The second sets the
language of the PRD. Run `/prd` again in the same folder to continue a draft.
Control words: `back`, `skip`, `status`, `draft`, `stop`.

When the PRD has `Status: Ready`, start a fresh session and give it only
`PRD.md`.

## License

[MIT](LICENSE) · Created by Shleyger Ram
