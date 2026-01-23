# agen

A Unix-native AI agent CLI that composes in pipelines.

## Philosophy

**Vision B: Agent in the Shell** — The agent is one tool among many. The shell orchestrates. Agents compose in pipelines.

```bash
cat error.log | agen "diagnose" | agen "suggest fix" > recommendations.md
```

## Installation

1. Clone this repository
2. Ensure dependencies are installed: `bash 4+`, `curl`, `jq`
3. Set your API key: `export ANTHROPIC_API_KEY="your-key"`
4. Run: `./agen --help`

## Usage

```
agen [OPTIONS] [PROMPT]
command | agen [OPTIONS] [PROMPT]
```

### Options

| Option | Description |
|--------|-------------|
| `--help` | Show help message |
| `--version` | Show version |
| `--batch` | No interactive prompts; exit 2 if input needed |
| `--json` | Output JSON instead of plain text |
| `--state=FILE` | State file for persistence/resume |
| `--resume` | Continue from state file |
| `--checkpoint` | Save state after completion |
| `--max-turns=N` | Maximum agentic loop iterations (default: 1) |
| `--tools=LIST` | Tools to enable (default: none). Available: bash |
| `--model=MODEL` | Model to use (default: claude-sonnet-4-20250514) |
| `--verbose` | Debug output on stderr |

### Exit Codes

| Code | Meaning | Script Usage |
|------|---------|--------------|
| 0 | Success | `agen && echo "done"` |
| 1 | Failure | `agen \|\| echo "failed"` |
| 2 | Needs input | Retry with more context |
| 3 | Hit limit | Increase --max-turns |

## Examples

### Simple Query

```bash
agen "Explain Unix pipes"
```

### Pipeline Usage

```bash
# Diagnose an error
cat error.log | agen "diagnose this error"

# Chain agents
cat data.csv | agen "summarize" | agen "format as markdown" > summary.md
```

### Stateful Conversation

```bash
# Start a conversation
agen --state=chat.json "Help me design an API"

# Continue it later
agen --state=chat.json --resume "Add authentication"
```

### Agentic Mode

```bash
# Let the agent execute commands
agen --tools=bash --max-turns=5 "Create a Python hello world and run it"
```

### Scripting

```bash
# Use in scripts with proper error handling
if cat report.txt | agen --batch "summarize"; then
  echo "Summary complete"
else
  case $? in
    1) echo "Error occurred" ;;
    2) echo "More context needed" ;;
    3) echo "Task too complex" ;;
  esac
fi
```

## Dependencies

- **bash 4+** — for modern bash features
- **curl** — for API requests
- **jq** — for JSON handling
- **ANTHROPIC_API_KEY** — environment variable

## License

MIT
