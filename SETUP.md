# Quick Setup Guide

## Installation

```bash
cd ~/Sites/github/sauliuspr/iterm2-claude-status
chmod +x scripts/claude_status.sh
```

## Test It

```bash
# Compact format
./scripts/claude_status.sh

# Detailed format  
./scripts/claude_status.sh --format=detailed

# Without emoji (for tmux/plain terminals)
./scripts/claude_status.sh --no-emoji
```

## tmux Setup ✅

Already added to your `~/.tmux.conf`. To apply:

```bash
# Reload tmux config (if already running)
tmux source-file ~/.tmux.conf

# Or restart tmux
tmux kill-server && tmux new-session
```

Your status bar will show:
```
[0] editor  ⚙  haiku | 📊 64% | 💾 16k | 🔧 0 tools  14:32
```

## iTerm2 Setup

1. Open **iTerm2 → Preferences → Profiles → [Your Profile] → Status Bar**
2. Enable "Status bar is visible"
3. Click "Configure Status Bar"
4. Drag **Custom** component to the right side
5. Configure:
   - **Command:** `~/Sites/github/sauliuspr/iterm2-claude-status/scripts/claude_status.sh --no-emoji`
   - **Refresh:** `2` seconds
6. Click Done

Or import the example config:
```bash
cp examples/iterm2-statusbar.json ~/Library/Application\ Support/iTerm2/DynamicProfiles/
```

## Global Installation (Optional)

```bash
sudo ln -s ~/Sites/github/sauliuspr/iterm2-claude-status/scripts/claude_status.sh /usr/local/bin/claude-status
```

Then use:
```bash
claude-status  # Works from anywhere
```

## Troubleshooting

**Script not found in iTerm2:**
- Use full path: `/Users/YOUR_USER/Sites/github/sauliuspr/iterm2-claude-status/scripts/claude_status.sh`
- Or install globally (see above)

**No output or "inactive":**
- Ensure Claude Code is running
- Check `~/.claude/history.jsonl` exists: `ls -la ~/.claude/history.jsonl`

**Metrics not updating:**
- Refresh rate should be 2-5 seconds
- Restart iTerm2 or run `source ~/.tmux.conf` in tmux

