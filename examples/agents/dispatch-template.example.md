# Agent Dispatch Template (Sanitized Example)

# Every agent prompt follows this structure to ensure
# clear objectives, proper context, and verifiable output.

## Template

```
## Objective
{1-2 sentences: what to deliver + done criteria}

## Files / Context
- {path/to/relevant/file}
- {path/to/another/file}

## Skills Available
- /skill-name — {why this skill helps}

## Constraints
- {What NOT to do}
- {Quality bar}

## Output
Write to: {specific path}
Commit with: git add {files} && git commit -m "{message}"
```

## Dispatch Decision Table

| Scale | Method |
|-------|--------|
| 1 simple task | Execute directly (no agent) |
| 1 heavy task | Single background agent |
| 2-3 independent tasks | Parallel agents in one message |
| 4+ coordinated tasks | Create team + task list + agents |

## Model Routing

| Task Type | Model | Rationale |
|-----------|-------|-----------|
| Code writing, architecture | Opus | Quality matters |
| Complex reasoning | Opus | Multi-step strength |
| Research, file operations | Sonnet | Faster, cheaper |
| Bulk audits | Sonnet | Parallel speed |
| Simple transforms | Sonnet | Opus is overkill |

## Self-Healing Rule
Failure → read output → classify error → re-dispatch with fix.
Same failure twice → escalate to human. Never retry blindly.
