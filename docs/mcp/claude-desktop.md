# Claude Desktop

Connect lite-diff to Claude Desktop for AI-assisted code patching.

## Quick Start

### 1. Start MCP Server in VS Code

**Option A: MCP Menu**

1. Click `MCP` button in lite-diff panel
2. Select **Claude Desktop**

**Option B: Status Bar**

Click `MCP` in Status Bar (bottom of VS Code) to toggle server on/off.

> **Note:** Uses last selected client. To change client, use MCP Menu or Command Palette.

**Option C: Command Palette**

1. Open Command Palette (`Ctrl+Shift+P`)
2. Run: `Diff Editor: Start MCP Server`
3. Select: **Claude Desktop**

Status Bar will show: `● MCP`

The extension automatically updates `claude_desktop_config.json` with the litediff server configuration.

### 2. Restart Claude Desktop

1. Fully close Claude Desktop
2. Start it again
3. Claude will automatically connect to MCP Server

> **Important:** Clicking "X" minimizes the app to system tray. To fully close: right-click the Claude icon in the tray (near the clock) → **Quit**.

### Try It

In Claude Desktop, ask:
```
What litediff tools are available?
```

Claude should list 18 tools including `litediff_preview`, `litediff_apply`, `litediff_validate`.

---

## Configuration

### Config File Location

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| Linux | `~/.config/Claude/claude_desktop_config.json` |

The extension automatically manages this file. Manual editing is not required.

---

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| "MCP server litediff not found" | Node.js not installed | Install [Node.js](https://nodejs.org/) |
| Tools don't work | VS Code not running | Open VS Code with the same workspace |
| Tools unavailable after restart | Config not updated | Restart MCP Server, then restart Claude Desktop |

---

## Useful Commands

After connecting, use these prompts in Claude Desktop:

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

> **Note:** UI commands (`showPanel`, `hidePanel`, `openDiff`, `revealHunk`) are not available in Claude Desktop. Use the lite-diff panel in VS Code directly.

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

This removes the litediff entry from `claude_desktop_config.json`. Restart Claude Desktop to apply changes.

---

## Limitations

| Limitation | Description |
|------------|-------------|
| One workspace | Server is bound to the current VS Code workspace |
| Requires restart | Config changes require Claude Desktop restart |
| UI requires VS Code | `showPanel`, `openDiff` only work if VS Code is open |
