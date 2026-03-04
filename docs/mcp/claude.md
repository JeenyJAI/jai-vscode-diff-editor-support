# Claude

Connect lite-diff to Claude for AI-assisted code patching.

## Supported Modes

| Mode | Config file | Transport | Notes |
|------|-------------|-----------|-------|
| **Claude Desktop — Chat** | `claude_desktop_config.json` | stdio (via launcher) | Global config, no `--workspace` arg |
| **Claude Desktop — Code** | `claude_desktop_config.json` | stdio (via launcher) | Same config as Chat mode |
| **Claude Code (VS Code ext)** | `.claude/settings.local.json` | stdio (direct) | Project-level config with `--workspace` arg |

> **Note:** When you select "Claude" in lite-diff, the extension registers in **both** configs simultaneously. No need to choose between modes.

---

## Quick Start

### 1. Start MCP Server in VS Code

**Option A: MCP Menu**

1. Click `MCP` button in lite-diff panel
2. Select **Claude**

**Option B: Status Bar**

Click `MCP` in Status Bar (bottom of VS Code) to toggle server on/off.

> **Note:** Uses last selected client. To change client, use MCP Menu or Command Palette.

**Option C: Command Palette**

1. Open Command Palette (`Ctrl+Shift+P`)
2. Run: `Diff Editor: Start MCP Server`
3. Select: **Claude**

Status Bar will show: `● MCP`

The extension automatically updates both config files:
- **Global:** `claude_desktop_config.json` — for Claude Desktop (Chat & Code)
- **Project:** `.claude/settings.local.json` — for Claude Code in VS Code

### 2. Connect Your Claude Client

**Claude Desktop (Chat or Code mode):**

1. Fully close Claude Desktop
2. Start it again
3. Claude will automatically connect to MCP Server

> **Important:** Clicking "X" minimizes the app to system tray. To fully close: right-click the Claude icon in the tray (near the clock) → **Quit**.

**Claude Code (VS Code extension):**

No restart needed — Claude Code reads `.claude/settings.local.json` automatically.

### Try It

Ask Claude:
```
What litediff tools are available?
```

Claude should list 19 tools including `litediff_preview`, `litediff_apply`, `litediff_validate`.

---

## Configuration

### Config File Locations

**Global config** (`claude_desktop_config.json`):

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| Linux | `~/.config/Claude/claude_desktop_config.json` |

**Project config** (`.claude/settings.local.json`):

Located at `<workspace>/.claude/settings.local.json` — created automatically when workspace is open.

The extension manages both files automatically. Manual editing is not required.

---

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| "MCP server litediff not found" | Node.js not installed | Install [Node.js](https://nodejs.org/) |
| Tools don't work | VS Code not running | Open VS Code with the same workspace |
| Tools unavailable after restart | Config not updated | Restart MCP Server, then restart Claude Desktop |
| Claude Code doesn't see tools | No workspace open | Open a folder in VS Code before starting MCP Server |

---

## Useful Commands

After connecting, use these prompts in Claude:

**Core operations:**

| Prompt | What it does |
|--------|--------------|
| "Preview this patch: ..." | Creates preview without applying |
| "Apply the preview" | Applies changes from current preview |
| "Validate this patch: ..." | Checks patch syntax only |

> **Note:** `preview` already includes validation. Use `validate` separately when you only need to check syntax without creating a preview.

**UI & status:**

| Prompt | What it does |
|--------|--------------|
| "Show current preview state" | Displays preview status |
| "List all backups" | Shows backup packages |

> **Note:** UI commands (`showPanel`, `hidePanel`, `openDiff`, `revealHunk`) require VS Code to be open and are available from Claude Code. In Claude Desktop Chat mode, use the lite-diff panel in VS Code directly.

**Selection:**

| Prompt | What it does |
|--------|--------------|
| "Select all hunks" | Selects all changes |
| "Deselect all hunks" | Deselects all changes |
| "Select only hunk h001 and h003" | Selects specific hunks |

---

## How It Works
```
You: "Add error handling to fetchData"
        ↓
Claude:
  1. Reads file content
  2. Generates lite-diff patch
  3. litediff_preview → creates preview (includes validation)
  4. litediff_apply → applies changes
        ↓
Done! File updated.
```

---

## Stopping the Server

To disconnect:

- **MCP Menu:** Click `MCP` button in lite-diff panel → **Stop Server**
- **Status Bar:** Click `● MCP` to toggle off
- **Command Palette:** `Diff Editor: Stop MCP Server`

This removes the litediff entry from both config files. Restart Claude Desktop to apply changes.

---

## Limitations

| Limitation | Description |
|------------|-------------|
| One workspace | Server is bound to the current VS Code workspace |
| Desktop requires restart | Config changes require Claude Desktop restart |
| UI requires VS Code | `showPanel`, `openDiff` only work if VS Code is open |
