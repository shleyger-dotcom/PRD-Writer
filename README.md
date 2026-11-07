# PRD-Writer — spec writer for Claude Code

Turns a raw idea into a `PRD.md` that an AI coder can build from without
guessing. Claude interviews you one question at a time. Each question comes
with a recommended answer and alternatives. Claude flags weak spots and
records every decision. `/prd-writer pack` adds `data-model.md`, `architecture.md`,
`adr/001-*.md` and `critique.md`.

## Installation

Requires [Claude Code](https://claude.com/claude-code). Run in PowerShell,
cmd or Git Bash (no admin rights needed):

```
claude plugin marketplace add shleyger-dotcom/PRD-Writer
claude plugin install prd-writer@prd-writer
```

Restart open Claude Code sessions to load the plugin.

Update:

```
claude plugin marketplace update prd-writer
claude plugin update prd-writer@prd-writer
```

Uninstall:

```
claude plugin uninstall prd-writer@prd-writer
```

## Usage

Open Claude Code in your project folder and run:

| Command | Result |
|---|---|
| `/prd-writer` | Interview → `PRD.md` |
| `/prd-writer <idea>` | Same, starting from your idea |
| `/prd-writer pack` | `PRD.md` + data model, architecture, ADR, critique |

The first question sets the language of the questions. The second sets the
language of the PRD. Run `/prd-writer` again in the same folder to continue a draft.
Control words: `back`, `skip`, `status`, `draft`, `stop`.

When the PRD has `Status: Ready`, start a fresh session and give it only
`PRD.md`.

## License

[MIT](LICENSE) · Created by Shleyger Ram
