# How I Made Claude Code Self-Improving: A Field Guide from 397 Sprints

> **By Luke Alvarez** | Founder, Seed Foundation (501(c)(3)) | Custer, South Dakota
>
> 397 sprints. 782 git commits. 1,000+ AI agents dispatched. 80 active days. One founder.
>
> **I have never written a single line of code.** Claude wrote all 800,000+ lines across 39 repositories. I configured, corrected, and directed. Claude built. This guide documents how.
>
> Most Claude Code users are operating at maybe 10% of what's possible. This guide shows you the other 90%.
>
> Inspired by the growing movement of AI-native builders (like [@karpathy's autoresearch](https://github.com/karpathy/autoresearch) and [@ellen_in_sf's autoresearch-gen](https://x.com/ellen_in_sf/status/2032538112971444436)) who are proving that the right configuration turns AI from a tool into a co-founder. The goal: help every Claude Code user verify they're using it fully, and build a subject matter expert that stays current with every CLI update and feature release.

---

## What This Guide Is

I used Claude Code every single day for 80 days to build an AI-powered economic development nonprofit from scratch. By the end, Claude wasn't just writing code for me. It was dispatching its own agents, detecting its own failures, generating rules to prevent recurrence, and permanently improving without retraining.

This guide documents every system I built to make that happen. Not theory. Receipts.

---

## Table of Contents

1. [The Self-Improvement Pipeline](#1-the-self-improvement-pipeline)
2. [Hook Architecture (19 Events, 35 Handlers)](#2-hook-architecture)
3. [The Skills System](#3-the-skills-system)
4. [Agent Orchestration (Boris Overseer)](#4-agent-orchestration)
5. [MCP Server Integration (8 Servers)](#5-mcp-server-integration)
6. [The Memory System](#6-the-memory-system)
7. [Brain Dump Workflow (Voice-First, ADHD-Native)](#7-brain-dump-workflow)
8. [Quality Gates (7-Signal Rejection)](#8-quality-gates)
9. [What Most Users Miss](#9-what-most-users-miss)
10. [The Results](#10-the-results)

---

## 1. The Self-Improvement Pipeline

Most people use Claude Code as a tool. I turned it into a system that learns from every mistake.

### How It Works

```
User corrects Claude → Hook captures the correction →
Pattern clustered with similar corrections →
Rule generated automatically →
Rule saved to persistent file →
Next session loads the rule →
Claude never makes that mistake again
```

This is not fine-tuning. It's not RLHF. It's application-layer value alignment using Claude Code's native hook system and persistent memory files. Every correction I make becomes a permanent rule. After 397 sprints, I have hundreds of rules that make Claude behave exactly the way my operation needs.

### The Key Insight

Claude Code has a `CLAUDE.md` file system. Most users put a few lines in it. I built a layered architecture:

```
~/.claude/CLAUDE.md                    # Global rules (apply everywhere)
~/.claude/rules/*.md                   # Always-on behavioral rules
~/project/CLAUDE.md                    # Project-specific rules
~/.claude/projects/{project}/memory/   # Persistent memory across sessions
~/.claude/skills/*/SKILL.md            # Callable expertise modules
```

Each layer serves a purpose. Global rules handle voice, tone, and anti-patterns. Project rules handle domain-specific behavior. Memory persists facts across conversations. Skills provide callable expertise.

### The Error-to-Rule Pipeline

When something goes wrong, I don't just fix it. I run `/error-to-rule`, a custom skill that:

1. Reviews the failure
2. Classifies the root cause
3. Generates a persistent rule to prevent recurrence
4. Saves it to the rules directory
5. Verifies the rule loads in future sessions

After 397 sprints, my Claude Code instance has internalized hundreds of corrections that a fresh instance would repeat.

---

## 2. Hook Architecture

Claude Code supports hook events that run shell commands in response to tool calls. Most users don't know they exist. I have 19 event types with 35 handlers.

### What Hooks Do

Hooks intercept Claude's actions and run your code before or after. Examples:

- **Pre-commit hooks**: Run security scans before any git commit
- **Post-tool hooks**: Log every tool call for audit trails
- **Session start hooks**: Load context, check environment, verify credentials
- **Notification hooks**: Alert you when background agents complete

### My Hook Categories

| Category | Events | What They Do |
|----------|--------|-------------|
| Quality gates | 5 | Reject low-quality output before it ships |
| Security | 3 | Scan for secrets, validate permissions |
| Auto-learn | 4 | Capture corrections, cluster patterns |
| Self-healing | 3 | Detect failures, retry with different approach |
| Notification | 2 | Background agent completion alerts |
| Context | 2 | Load/unload project context on session boundaries |

### Why This Matters

Without hooks, Claude Code is reactive. With hooks, it's proactive. My instance catches its own mistakes, enforces its own quality standards, and improves its own behavior, all without me typing a single correction.

---

## 3. The Skills System

Skills are callable expertise modules. Think of them as specialized knowledge that Claude can invoke on demand instead of keeping in memory all the time.

### How Skills Work

```
~/.claude/skills/
  dispatch/SKILL.md          # Multi-agent team orchestration
  ghostty/SKILL.md           # Terminal management expertise
  political-outreach/SKILL.md # Political research pipeline
  smaug-filer/SKILL.md       # Intelligence filing system
  cf-crawl/SKILL.md          # Website crawling via Cloudflare
  quality-audit/SKILL.md     # Universal data audit pattern
  error-to-rule/SKILL.md     # Convert failures to rules
  ...28+ total
```

Each skill is a markdown file containing:
- When to activate (trigger conditions)
- Domain knowledge (what the skill knows)
- Workflows (step-by-step procedures)
- Tool routing (which tools to use)

### Why Skills Beat System Prompts

System prompts are always loaded. Skills are loaded on demand. This means:
- Your context window isn't wasted on knowledge you don't need right now
- Each skill can be deep (thousands of lines) without bloating every conversation
- Skills can be shared, versioned, and composed

I have 28+ skills covering everything from political outreach to terminal configuration to web scraping routing. Claude invokes them when it recognizes the task matches the skill's trigger conditions.

---

## 4. Agent Orchestration

### Boris Overseer Mode

I built an operating pattern called "Boris" where Claude acts as a team lead, not an individual contributor. Instead of doing work directly, it:

1. Parses my input (usually a voice-to-text brain dump)
2. Classifies each task (code / research / content / infra)
3. Detects dependencies between tasks
4. Dispatches specialized agents to handle each one
5. Monitors progress and self-heals on failure

### The Dispatch Pattern

```
1 simple task    → Execute directly
1 heavy task     → Single background agent
2-3 independent  → Parallel agents in one message
4+ coordinated   → TeamCreate + TaskCreate + Agent(team_name)
```

### Agent Teams

Claude Code's native team system lets you spawn named agents that:
- Run in isolated git worktrees (no conflicts)
- Share a task list (coordination without collision)
- Send messages to each other
- Report back to the team lead
- Shut down gracefully when done

In one sprint, I dispatched 4 agents simultaneously: one crawling websites, one compiling local intelligence, one scraping Twitter, and one verifying claims. They ran in parallel, completed in minutes, and merged their outputs.

### The Numbers

- 1,000+ total agent dispatches across 397 sprints
- Peak: 4 agents running simultaneously per team
- Self-healing: failed agents are diagnosed and re-dispatched automatically
- Worktree isolation: agents commit their changes before shutdown or changes are lost

---

## 5. MCP Server Integration

Model Context Protocol (MCP) connects Claude Code to external services. I run 8 servers:

| Server | What It Does | How I Use It |
|--------|-------------|-------------|
| Supabase | PostgreSQL database | CRM queries, security audits, data pipeline |
| Slack | Team communication | Channel monitoring, message sending |
| Notion | Knowledge management | Page creation, search, updates |
| Playwright | Browser automation | Form filling, web app testing, screenshots |
| GitHub | Repository management | PR creation, issue tracking |
| Context7 | Library documentation | Up-to-date docs for any framework |
| Stripe | Payment processing | Product/price management |

### Tool Routing

Not every tool is right for every job. I built a routing table:

| Need | Right Tool | Wrong Tool |
|------|-----------|------------|
| Twitter/X | `bird` CLI | WebFetch (always fails on x.com) |
| JS-heavy pages | Playwright MCP | curl (gets empty shell) |
| Static pages | WebFetch or curl | Playwright (overkill) |
| Database | Supabase MCP | Raw psql (breaks sandboxing) |
| Full site crawl | Cloudflare Browser Rendering | Individual page fetches |

These routing rules are saved in persistent configuration so Claude never uses the wrong tool twice.

---

## 6. The Memory System

Claude Code conversations end. Memory persists.

### How It's Structured

```
~/.claude/projects/{project}/memory/
  MEMORY.md                    # Index (always loaded)
  operating-system.md          # Org structure, entity map
  government-ai-product.md     # Product details
  deal-pipeline.md             # Active deals, pitch status
  partnerships.md              # Key relationships
  infrastructure.md            # Tech stack, tools, gaps
  anti-patterns.md             # Known failures to avoid
  ...15+ topic files
```

### Memory Types

| Type | Purpose | Example |
|------|---------|---------|
| User | Who am I, how I work | "ADHD, voice-first, skip disclaimers" |
| Feedback | Corrections that apply to future sessions | "Don't mock the database in tests" |
| Project | Ongoing work context | "Merge freeze starts March 5" |
| Reference | Where to find external information | "Pipeline bugs tracked in Linear project INGEST" |

### The Index Pattern

`MEMORY.md` is always loaded into context. It contains only links to topic files with brief descriptions, never the content itself. This keeps the always-on context cost low while making deep knowledge accessible on demand.

---

## 7. Brain Dump Workflow

I have ADHD. My input to Claude is often a voice-to-text brain dump: typo-heavy, stream-of-consciousness, 3-7 tasks mixed together with filler words.

### The Brain Dump Parser

Claude is configured to:

1. **Extract tasks** from natural language (each action verb = one task)
2. **Classify each** (code / research / content / infra / financial)
3. **Detect dependencies** ("after X" / "once Y" = blocked by)
4. **Route by complexity** (simple = direct, complex = agent dispatch)
5. **Never ask for clarification** on brain dumps (parse and act)

### Why This Matters for AI Accessibility

This is an ADHD accommodation built entirely with AI. The system adapts to my cognitive style instead of requiring me to adapt to it. Dario Amodei wrote about AI helping neurodivergent individuals. This is what that looks like in practice: a system that takes fragmented, voice-first input and converts it into structured, executed work.

### The Anti-ADHD-Tax Rules

- **Persistence**: Any items from a brain dump that can't be acted on immediately get saved to a priorities file AND created as tasks. Nothing gets lost.
- **Circle back**: The system explicitly tracks unanswered questions and circles back.
- **No confirmation needed**: "Should I proceed?" is banned. Just do it.
- **Execution bias**: Default is execute, not ask. Only confirm for destructive operations.

---

## 8. Quality Gates

### The 7-Signal Rejection System

Before Claude delivers anything, it self-checks against 7 signals:

| Signal | What It Catches |
|--------|----------------|
| Filler | "It's worth noting", "Let me explain", "As mentioned" |
| Over-politeness | "I hope this helps!", "Let me know if..." |
| Hedge stacking | "might potentially perhaps" |
| Obvious comments | Documenting what code already says |
| Generic claims | "enhances the user experience" |
| Cosmetic-only changes | Whitespace/formatting passed off as work |
| Placeholder content | TODO/TBD/"coming soon" in deliverables |

If Claude catches itself writing any of these, it rewrites the line before delivering.

### Anti-Hallucination Protocol

- Verify numbers against a single source-of-truth file
- `ls | wc -l`, `grep -c`, actual file reads over estimates
- If 2-3 searches confirm something doesn't exist, stop searching
- Never cite numbers without checking the verified table

---

## 9. What Most Users Miss

After 397 sprints, here are the features most Claude Code users don't know about or underuse:

### 1. Hooks (the biggest gap)
Most users have zero hooks configured. Hooks are the difference between a tool and a system. Start with one: a pre-commit hook that checks for secrets.

### 2. CLAUDE.md layering
Most users have a single CLAUDE.md with a few lines. The layered architecture (global + rules + project + memory) is where the real power lives.

### 3. Skills (on-demand expertise)
Instead of cramming everything into the system prompt, build skills that load when needed. A 2,000-line skill costs zero tokens until invoked.

### 4. Agent teams with worktree isolation
You can spawn 4 agents working in parallel on isolated copies of your repo. They don't conflict. They commit independently. The team lead orchestrates.

### 5. Memory persistence
Without memory files, every conversation starts from zero. With them, Claude knows your project, your preferences, your anti-patterns, and your priorities from the first message.

### 6. MCP servers
8 external services connected through a standardized protocol. Most users interact with Claude through text only. MCP gives it hands: databases, browsers, messaging, documentation.

### 7. The self-improvement loop
Corrections → rules → better behavior → fewer corrections. This compounds. By sprint 397, Claude makes errors that sprint-1 Claude would have made, approximately never.

---

## 10. The Results

### What I Built in 80 Active Days

| Metric | Value |
|--------|-------|
| Claude Code sprints | 397+ |
| Git commits | 782 |
| AI agents dispatched | 1,000+ |
| Federal data API connectors | 17 |
| Intelligence files produced | 693 (348,000+ lines) |
| Websites deployed | 15 |
| CRM records | Thousands |
| Partner towns | 14 |
| Government AI terminals deployed | 6 |
| Economic impact model | Multi-community, multi-year |
| Campus developed | 15 acres, 3 buildings |
| Political meetings | State and federal elected officials |

### What This Proves

One person with Claude Code, configured correctly, can produce the documented output of a mid-sized organization. Not because AI replaces people, but because the self-improving system compounds. Every sprint is faster than the last. Every correction prevents the next error. Every skill makes the next task easier.

The question isn't whether AI can augment individual productivity. The data from 397 sprints proves it can. The question is what happens when that capability reaches the communities that need it most, the ones your Economic Index shows are being left behind.

I moved to a town of 2,100 people to find out.

---

## About the Author

**Luke Alvarez** is the founder of Seed Foundation, a 501(c)(3) AI-powered economic development nonprofit operating from Custer, South Dakota. He previously held 8 roles at Instructure (Canvas LMS, $4.8B KKR acquisition) and managed federal government accounts at Unisys (DoD, DFAS, Interior). He holds an MBA from BYU's Marriott School of Business and a BS in Data Science. He is a Wilderness First Responder and nearly 5-year veteran of Custer County Search and Rescue.

He uses Claude Code on a Max plan with 1M context every day. His API spend is in Anthropic's billing system.

**Contact:** luke@blackhillsconsortium.org

---

## Acknowledgments

Thanks to [Mike Krieger](https://x.com/mikeyk) and the Claude team for building something worth mastering. The doubled usage hours came at exactly the right time. Thanks to [@ellen_in_sf](https://x.com/ellen_in_sf) for showing what AI-native research looks like, and to [@karpathy](https://github.com/karpathy) for autoresearch. We're all building the same future from different angles.

And thanks to Claude itself. 800,000 lines of code, zero written by me. Every sprint, every agent, every terminal. Co-founded, not assisted.

---

*This guide was written with Claude Code. Obviously.*
