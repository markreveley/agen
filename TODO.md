# TODO: Rename `agen` → `agent`

This repo is being renamed from `agen` to `agent` as part of a toolkit-wide rename.
The GitHub repo will move from `shellagentics/agen` to `shellagentics/agent`.

## Summary of changes

| What | Old | New |
|------|-----|-----|
| Repo name | `agen` | `agent` |
| Executable | `agen` | `agent` |
| Env: backend | `AGEN_BACKEND` | `AGENT_BACKEND` |
| Env: stub file | `AGEN_STUB_FILE` | `AGENT_STUB_FILE` |
| GitHub URL | `shellagentics/agen` | `shellagentics/agent` |

Related renames happening across the toolkit (update any cross-references):

| Old | New |
|-----|-----|
| `agen-memory` | `amem` |
| `agen-log` | `alog` |
| `agen-audit` | `aaud` |
| `agen-skills` | `ascr` |
| "skills" (concept) | "scripts" |
| `AGEN_MEMORY_DIR` | `AGENT_MEMORY_DIR` |
| `AGEN_LOG_DIR` | `AGENT_LOG_DIR` |
| `AGEN_PATH` | `AGENT_PATH` |

---

## File-by-file instructions

### 1. Rename the executable: `agen` → `agent`

```bash
mv agen agent
```

### 2. Edit `agent` (the renamed executable)

This is the main script (~376 lines). Make these changes:

- **Line 3 comment**: `# agen - Unix-native AI agent CLI` → `# agent - Unix-native AI agent CLI`
- **All comment references**: Replace `agen` with `agent` in all comments at the top of the file
- **Usage examples in comments**: Update `agen "Explain Unix pipes"` → `agent "Explain Unix pipes"` etc.
- **AGEN_BACKEND**: Replace all occurrences of `AGEN_BACKEND` with `AGENT_BACKEND` (in env var reads, help text, comments)
- **AGEN_STUB_FILE**: Replace all occurrences of `AGEN_STUB_FILE` with `AGENT_STUB_FILE`
- **Default stub path**: `/tmp/agen-stub-counter` → `/tmp/agent-stub-counter`
- **die() prefix**: `"agen: $*"` → `"agent: $*"`
- **verbose() prefix**: `"agen: $*"` → `"agent: $*"`
- **show_help()**: Update all text — the tool name, synopsis, examples, environment section
- **show_version()**: `"agen $VERSION"` → `"agent $VERSION"`
- **ENVIRONMENT section in help**: Update env var names
- **BACKENDS section in help**: Update references

**Exact string replacements to apply throughout the file:**
- `agen:` → `agent:` (in die/verbose/error messages)
- `agen "` → `agent "` (in examples/comments)
- `agen --` → `agent --` (in examples/comments)
- `AGEN_BACKEND` → `AGENT_BACKEND`
- `AGEN_STUB_FILE` → `AGENT_STUB_FILE`
- `/tmp/agen-stub-counter` → `/tmp/agent-stub-counter`
- `agen $VERSION` → `agent $VERSION`
- The word `agen` standing alone as tool name → `agent` (be careful not to change "agent" inside words like "agent-1")

### 3. Edit `test.sh`

- `AGEN=` variable assignment: update the path to `"$SCRIPT_DIR/agent"`
- All references to the `agen` command name in test descriptions
- `AGEN_BACKEND` → `AGENT_BACKEND` in env var usage
- `AGEN_STUB_FILE` → `AGENT_STUB_FILE` in env var usage
- `/tmp/agen-stub-counter` → `/tmp/agent-stub-counter` if referenced
- Update test output string checks (e.g., version check for "0.3.0")

### 4. Edit `README.md`

This is extensive. Replace throughout:
- All occurrences of `` `agen` `` (as a command name) → `` `agent` ``
- `agen "` → `agent "` in all code examples
- `agen --` → `agent --` in all code examples
- `AGEN_BACKEND` → `AGENT_BACKEND`
- `AGEN_STUB_FILE` → `AGENT_STUB_FILE`
- `/tmp/agen-stub-counter` → `/tmp/agent-stub-counter`
- `# agen` (heading) → `# agent`
- GitHub URLs: `shellagentics/agen` → `shellagentics/agent`
- Cross-references to other tools:
  - `agen-skills` → `ascr`
  - `agen-memory` → `amem`
  - `agen-log` → `alog`
  - `agen-audit` → `aaud`
  - "skill script" → "agent script" or just "script" (where referring to the concept)
- The sentence about "the skill script is the gatekeeper" → "the orchestrating script is the gatekeeper"
- Directory structure listing: `agen/` → `agent/` and filename `agen` → `agent`

### 5. Edit `DEVLOG.md`

- Update references to `agen` → `agent` (the tool name)
- The changelog entry at bottom mentions "Renamed CLI from `agent` to `agen`" — this should now be updated or removed since we're renaming back to `agent`

### 6. Edit `CLAUDE.md`

This file has extensive references (~333 lines). Replace throughout:
- All occurrences of `agen` as a tool name → `agent`
- `AGEN_BACKEND` → `AGENT_BACKEND`
- `AGEN_STUB_FILE` → `AGENT_STUB_FILE`
- All code examples using `agen`
- The project overview section
- The directory structure listing
- Commit message format examples
- The "Why Bash" section references

### 7. Edit `prompts/README.md`

- `` `agen` `` → `` `agent` `` throughout
- "build and guide the `agen` CLI" → "build and guide the `agent` CLI"
- `BUILD_PROMPT.md` description references

### 8. Edit `prompts/BUILD_PROMPT.md`

This is the longest file (~565 lines). Replace throughout:
- All occurrences of `agen` as tool name → `agent`
- `build-agen-cli` prompt ID → `build-agent-cli`
- `Building agen` → `Building agent`
- All code examples
- The interface contract sections
- Phase descriptions
- Quick reference card
- Changelog entry about renaming

---

## Verification

After all changes, run:

```bash
# Ensure no stale references remain
grep -r "agen" --include="*.md" --include="*.sh" .
grep -r "AGEN_" --include="*.md" --include="*.sh" .

# Run the test suite
./test.sh
```

The grep results should only show legitimate uses (like the word "agent" which contains "agen" as a substring). Filter carefully.

## Delete this file when done

Remove this TODO.md after completing all changes.
