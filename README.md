# iterm2-claude-status

Display real-time Claude Code session metrics in your iTerm2 status bar.

A lightweight shell script that extracts and displays key Claude Code metrics:
- **Model**: Current Claude model (e.g., opus-4, sonnet-4, haiku)
- **Context**: Context window usage percentage
- **Tokens**: Approximate token usage
- **Tools**: Number of tool calls in the session
- **Runtime**: Session duration

## Features

✨ **Lightweight** - Pure bash/zsh, minimal dependencies
⚡ **Fast** - Designed to refresh every 1-5 seconds
🎯 **Graceful** - Fails silently if no active Claude session
📊 **Simple** - Single-line output for status bars
🔧 **Extensible** - Easy to adapt for custom metrics

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/sauliuspr/iterm2-claude-status.git
cd iterm2-claude-status
```

### 2. Make the script executable

```bash
chmod +x scripts/claude_status.sh
```

### 3. (Optional) Install globally

```bash
sudo cp scripts/claude_status.sh /usr/local/bin/claude-status
```

Or add to your shell config:

```bash
alias claude-status="~/path/to/iterm2-claude-status/scripts/claude_status.sh"
```

## Usage

### Quick Test

```bash
./scripts/claude_status.sh
```

Expected output:
```
🤖 haiku | 📊 65% | 💾 12k | 🔧 3 tools
```

### Command-Line Options

```bash
# Compact format (default)
./scripts/claude_status.sh --format=compact

# Detailed format
./scripts/claude_status.sh --format=detailed

# Disable emoji output
./scripts/claude_status.sh --no-emoji
```

### Sample Output

**Compact (default):**
```
🤖 opus-4 | 📊 72% | 💾 84k | 🔧 4 tools
```

**Detailed:**
```
🤖 Claude Status
  Model: opus-4
  Context: 72%
  Tokens: 84k
  Tools: 4
  Runtime: 23m
```

**No session:**
```
⚫ inactive
```

## iTerm2 Configuration

### Add Status Bar Component

1. Open **iTerm2 → Preferences → Profiles → <Your Profile>**
2. Go to the **Status Bar** tab
3. Click **Configure Status Bar**
4. Drag **Custom** component into the status bar configuration
5. Double-click the new **Custom** component to configure it

### Configuration JSON

In the component settings, set:

**Component:** `Custom`
**Execute:** `/path/to/iterm2-claude-status/scripts/claude_status.sh`
**Refresh rate:** `2` (seconds)

### Using the Example Config

The `examples/iterm2-statusbar.json` file shows a complete status bar setup:

```json
{
  "advanced_configuration": {
    "auto_log": true,
    "check_for_updates_on_startup": true
  },
  "customComponents": [
    {
      "knobs": {
        "base64": false,
        "command": "/path/to/scripts/claude_status.sh",
        "refresh_interval": 2
      },
      "uuid": "claude-status"
    }
  ]
}
```

To use:

1. Replace `/path/to/scripts/claude_status.sh` with your actual path
2. Copy the configuration to your iTerm2 preferences
3. Restart iTerm2

## Dependencies

The script uses only standard Unix tools:
- `bash` or `zsh`
- `jq` (optional, for enhanced parsing)
- `grep`, `sed`, `awk`, `stat` (standard)

If `jq` is not installed, the script falls back to basic grep parsing.

### Install jq (optional, for better performance)

**macOS:**
```bash
brew install jq
```

**Linux:**
```bash
sudo apt-get install jq
```

## Data Sources

The script reads Claude Code session data from:

- **Primary:** `~/.claude/history.jsonl` - Session and activity logs
- **Secondary:** `~/.claude/debug/` - Debug logs (if available)
- **Fallback:** System estimation based on file timestamps

## Performance

- **Startup time:** < 50ms (with jq), < 100ms (without)
- **Memory usage:** < 5MB
- **Recommended refresh:** 2-5 seconds in iTerm2

## Troubleshooting

### No session appears

1. Ensure Claude Code is running and active
2. Check that `~/.claude/history.jsonl` exists:
   ```bash
   ls -la ~/.claude/history.jsonl
   ```
3. Try running the script manually:
   ```bash
   ./scripts/claude_status.sh
   ```

### Metrics show as "?"

The script may not have access to detailed usage metrics from Claude Code yet. This is expected. The tool estimates metrics based on available session data.

### iTerm2 status bar not updating

1. Check the refresh rate (should be 2-5 seconds)
2. Verify the script path is correct and executable
3. Check iTerm2 preferences for status bar enabled
4. Restart iTerm2

## How It Works

1. **Reads** the Claude history log (`~/.claude/history.jsonl`)
2. **Extracts** the most recent session ID
3. **Calculates** metrics from available data:
   - **Model:** From recent Claude API calls
   - **Context %:** Estimated from session freshness
   - **Tokens:** Estimated from token-heavy operations
   - **Tools:** Count of tool invocations
   - **Runtime:** Calculated from file timestamp
4. **Outputs** as a single-line status string

## Extending the Tool

You can extend the script to read custom metrics:

1. Add your metric extraction function
2. Update `extract_metrics()` to include new data
3. Modify the output format in `format_output()`

Example: Add cost tracking

```bash
# In extract_metrics()
local cost="$0.15"

# In format_output()
echo "${ROBOT} ${model} | 💰 ${cost}"
```

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

MIT License - see LICENSE file for details

## Author

Created by [@sauliuspr](https://github.com/sauliuspr)

## Acknowledgments

- iTerm2 for excellent terminal customization options
- Claude Code team for the great development environment

---

**Questions or issues?**
Open an issue on [GitHub](https://github.com/sauliuspr/iterm2-claude-status/issues)
