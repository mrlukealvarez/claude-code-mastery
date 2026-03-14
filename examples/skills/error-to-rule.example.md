# Error-to-Rule Skill (Sanitized Example)

# This skill converts failures into persistent rules that prevent recurrence.

## When to Trigger
- After sprint failures or agent crashes
- After repeated mistakes
- When user says "learn from this" or "don't do that again"

## Workflow

1. **Review the failure**: Read the error output, identify root cause
2. **Classify the error type**: Configuration, logic, tool misuse, hallucination, scope creep
3. **Check existing rules**: Search rules/ directory for duplicate or related rules
4. **Generate the rule**: Write a clear, actionable rule with:
   - What went wrong
   - Why it went wrong
   - What to do instead
   - When this rule applies
5. **Save to rules directory**: Write as a markdown file in ~/.claude/rules/
6. **Verify loading**: Confirm the rule loads in the next session

## Rule Template

```markdown
# Rule: {descriptive-name}

## Problem
{What went wrong, specifically}

## Root Cause
{Why it happened}

## Rule
{What to do instead, stated as a clear directive}

## When This Applies
{Conditions under which this rule activates}
```

## Example

A tool was used for the wrong purpose repeatedly:
- Problem: WebFetch used on x.com URLs, which always fail
- Root cause: No routing rule for Twitter/X content
- Rule: "Always use bird CLI for X/Twitter content. WebFetch on x.com always fails."
- Saved to: rules/scraping-routing.md
- Result: Never happened again across 300+ subsequent sprints
