# JAI: Diff Editor (lite-diff)

## 100% AI Code · Human Reviewed

[![version](https://img.shields.io/open-vsx/v/JeenyJAI/diff-editor?label=version)](https://open-vsx.org/extension/JeenyJAI/diff-editor)
[![VS Code](https://img.shields.io/visual-studio-marketplace/i/JeenyJAI.diff-editor?label=VS%20Code)](https://marketplace.visualstudio.com/items?itemName=JeenyJAI.diff-editor)
[![Open VSX](https://img.shields.io/open-vsx/dt/JeenyJAI/diff-editor?label=Open%20VSX)](https://open-vsx.org/extension/JeenyJAI/diff-editor)
[![spec: lite-diff v1.0](https://img.shields.io/badge/spec-lite--diff%20v1.0-419fff)](https://github.com/lite-diff/spec)
[![AI generated](https://img.shields.io/badge/AI%20generated-100%25-purple.svg)](https://github.com/JeenyJAI/jai-vscode-diff-editor-support)

**[Install from VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=JeenyJAI.diff-editor)**

### Working with AI assistants like Claude, Cursor, or ChatGPT? Tired of manually applying code patches?

This extension bridges unified diff format and your IDE. Apply AI-generated patches with preview, selective hunk application, automatic backups, and drift detection.

**Two ways to work:**
- **Manual** — Paste a lite-diff patch into the editor panel, preview in VS Code diff viewer, apply with one click
- **AI-assisted (MCP)** — Your AI agent reads code, generates patches, previews, and applies — all through MCP tools, no copy-paste needed

Based on the [lite-diff specification](https://github.com/lite-diff/spec).

## Features

- **In-file edits** — Replace, delete, insert with familiar unified diff syntax
- **Insert at file start/end** — Use `@@ BOF` and `@@ EOF` for file start/end
- **Boundary blocks** — Delete ranges between two marker lines using `...`
- **Global options** — Ignore whitespace, blank lines, apply to all matches
- **Preview mode** — See exact changes in VS Code diff viewer before applying
- **Multi-file support** — Process multiple files in a single patch
- **Automatic backups** — Every change creates a backup in `.ldiff/` directory
- **MCP Server** — Integrate with [Claude](https://github.com/JeenyJAI/jai-vscode-diff-editor-support/blob/main/docs/mcp/claude.md) and [Cursor IDE](https://github.com/JeenyJAI/jai-vscode-diff-editor-support/blob/main/docs/mcp/cursor.md) for AI-assisted patching

## Tutorial Videos

<table>
  <tr>
    <td align="center" width="33%">
      <a href="https://www.youtube.com/watch?v=8zqH6gGesec">
        <img src="https://raw.githubusercontent.com/JeenyJAI/jai-vscode-diff-editor-support/main/media/demo.gif" alt="Overview demo" width="100%">
      </a>
      <br><b>Overview Demo</b>
    </td>
    <td align="center" width="33%">
      <a href="https://youtu.be/eVA6jTJbRqs">
        <img src="https://img.youtube.com/vi/eVA6jTJbRqs/mqdefault.jpg" alt="Claude Desktop Chat" width="100%">
      </a>
      <br><b>Claude Desktop — Chat</b>
    </td>
    <td align="center" width="33%">
      <a href="https://youtu.be/XAV0g0eIny8">
        <img src="https://img.youtube.com/vi/XAV0g0eIny8/mqdefault.jpg" alt="Claude Desktop Code" width="100%">
      </a>
      <br><b>Claude Desktop — Code</b>
    </td>
  </tr>
  <tr>
    <td align="center" width="33%">
      <a href="https://youtu.be/De2HHm1QO9U">
        <img src="https://img.youtube.com/vi/De2HHm1QO9U/mqdefault.jpg" alt="Claude Code" width="100%">
      </a>
      <br><b>Claude Code (VS Code)</b>
    </td>
    <td align="center" width="33%">
      <a href="https://youtu.be/JAOP2e0elHY">
        <img src="https://img.youtube.com/vi/JAOP2e0elHY/mqdefault.jpg" alt="Cursor IDE" width="100%">
      </a>
      <br><b>Cursor IDE</b>
    </td>
    <td width="33%"></td>
  </tr>
</table>

## Quick Start

1. Open Command Palette (`Ctrl/Cmd`+`Shift`+`P`)
2. Run `JAI: Diff Editor (lite-diff)`
3. Paste your lite-diff patch
4. Review changes in diff viewer, then apply

## Syntax Guide

### Basic Edit

```diff
--- file.txt
+++ file.txt
@@
 context line (unchanged)
-old line
+new line
```

### Insert at File Start

```diff
--- file.txt
+++ file.txt
@@ BOF
+first line
+second line
```

### Insert at File End

```diff
--- file.txt
+++ file.txt
@@ EOF
+last line
```

> **Note:** After `@@ BOF` and `@@ EOF` only `+` lines are allowed. Context (` `) or deletion (`-`) lines will cause error **E411**.

### Delete Range (Boundary Block)

```diff
--- file.txt
+++ file.txt
@@
-start boundary
...
-end boundary
+replacement
```

Deletes everything from "start boundary" to "end boundary" inclusive, then inserts replacement.

**Rules for boundary blocks:**
- `...` cannot appear at the start or end of the block
- Two `...` in a row without `-` line between them are forbidden
- Each boundary must have `-` lines on both sides

## Global Options

Add at the beginning of your patch, before any `diff` block:

```diff
--ignore-space-at-eol
--ignore-blank-lines

--- file.txt
+++ file.txt
@@
 context line
-old line
+new line
```

| Option | Description |
|--------|-------------|
| `--ignore-space-at-eol` | Ignore trailing whitespace at end of lines |
| `--ignore-space-change` | Ignore changes in whitespace quantity (but not presence) |
| `--ignore-blank-lines` | Ignore blank lines when matching context |
| `--ignore-all-space` | Ignore all whitespace differences |
| `--apply-all-matches` | Apply hunk to all occurrences (single hunk only) |

## Important Rules

- All structural elements must begin **at the first position** (no indentation): `---`, `+++`, `@@`, `-`, `+`, ` ` (context), `...`, `#`, `--`
- No blank lines allowed inside hunk body (from `@@` to the next `@@` or end of diff block)
- Comments start with `#` at the first position on a separate line
- Preview creates directories `.ldiff/<TIMESTAMP>/original/` and `.ldiff/<TIMESTAMP>/preview/`

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `diffEditor.verboseLogging` | `false` | Enable verbose logging for debugging |
| `diffEditor.backupLimit` | `5` | Max backup packages to keep (0 = only active) |

## Telemetry

This extension collects minimal anonymous telemetry (feature usage counts, no personal data) to help prioritize development. Telemetry respects the VS Code `telemetry.telemetryLevel` setting — set it to `"off"` to disable. See [PRIVACY.md](./PRIVACY.md) for full details.

## Contributing

Found a bug or have a feature request? Please open an issue on [GitHub](https://github.com/JeenyJAI/jai-vscode-diff-editor-support/issues).

For other inquiries: JeenyJAI@gmail.com

## License

This extension is proprietary software. See [LICENSE](./LICENSE) for details.

The [lite-diff specification](https://github.com/lite-diff/spec) is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). This license applies to the specification only and does not grant any rights to this extension's source code or binaries — you're free to create your own implementations of the spec.

---

🚀 **Created with [Claude Opus 4.6](https://claude.ai) · Early contributions by [ChatGPT 5 Pro](https://chat.openai.com)**
