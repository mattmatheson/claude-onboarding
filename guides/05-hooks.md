# 05 — Hooks & Automation

> **Goal:** Set up automatic actions that fire when Claude does things — formatting code, notifications, voice feedback.

## What Are Hooks?

Hooks are shell commands that run automatically at specific moments in Claude's lifecycle. Skills are manual (you type `/something`). Hooks are automatic (they fire when conditions are met).

Think of hooks like GitHub Actions but for Claude Code. "When Claude finishes editing a file, auto-format it." "When Claude stops responding, play a sound."

## Why Hooks Matter

Without hooks, you're the automation. You manually run prettier after Claude edits code. You manually check if Claude is done. Hooks eliminate that friction — they make Claude's workflow self-correcting and self-notifying.

## Hook Lifecycle Events

| Event | When It Fires |
|-------|--------------|
| `SessionStart` | Claude Code launches |
| `PreToolUse` | Before Claude runs a tool (can block it) |
| `PostToolUse` | After a tool succeeds |
| `PostToolUseFailure` | After a tool fails |
| `Stop` | When Claude finishes responding |
| `WorktreeCreate` | When creating a git worktree |
| `WorktreeRemove` | When removing a git worktree |

## Where Hooks Are Configured

In `~/.claude/settings.json` under the `hooks` key:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "echo 'Claude is done' | say"
      }
    ]
  }
}
```

## Practical Hook Examples

### Auto-Format After File Edits

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null || true"
      }
    ]
  }
}
```

**What it does:** After Claude edits or creates a file, Prettier auto-formats it. No more style inconsistencies.

### Notification When Claude Finishes

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "osascript -e 'display notification \"Claude is done\" with title \"Claude Code\"'"
      }
    ]
  }
}
```

**What it does:** macOS notification when Claude finishes a response. Great for long-running tasks — kick it off, switch to another app, get notified when it's done.

### Text-to-Speech (macOS)

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "python3 ~/.claude/hooks/tts-stop.py"
      }
    ]
  }
}
```

**What it does:** Reads Claude's response aloud using macOS `say` command. Toggle on/off with voice commands.

> **How to build this:** Create a script that checks for a flag file (`~/.claude-voice-on`). If it exists, pipe Claude's output to `say`. If not, do nothing. Toggle with `touch ~/.claude-voice-on` and `rm ~/.claude-voice-on`.

### Block Dangerous Operations

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "echo \"$CLAUDE_TOOL_INPUT\" | grep -qE 'rm -rf|drop table|--force' && echo 'BLOCKED: Dangerous command detected' && exit 1 || exit 0"
      }
    ]
  }
}
```

**What it does:** Catches potentially destructive commands before they run. A safety net even in auto-approve mode.

## Building Your Own Hooks

1. **Identify the trigger.** What event should fire this? (Stop, PostToolUse, etc.)
2. **Write the command.** Any shell command or script works.
3. **Use matchers.** Filter which tools trigger the hook (e.g., only `Edit` and `Write`, not every tool).
4. **Test carefully.** A broken hook can block Claude from working. Start simple.

## Environment Variables Available in Hooks

Claude passes context to hook scripts via environment variables:
- `$CLAUDE_FILE_PATH` — The file being operated on
- `$CLAUDE_TOOL_INPUT` — The tool's input parameters
- `$CLAUDE_TOOL_NAME` — Which tool is running

## What's Next

Now let's set up Obsidian as Claude's persistent brain → [06 — Obsidian as Your Brain](06-obsidian.md)
