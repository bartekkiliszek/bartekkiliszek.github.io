---
layout: page
title: Claude Code Mastery Roadmap
permalink: /roadmaps/claude-code/
---

A comprehensive course from fundamentals to advanced techniques.

**Progress Legend:** `- [ ]` = Not started | `- [x]` = Completed

---

## Table of Contents

1. [Module 1: Fundamentals](#module-1-fundamentals)
2. [Module 2: Configuration and CLAUDE.md](#module-2-configuration-and-claudemd)
3. [Module 3: Effective Prompts and Workflows](#module-3-effective-prompts-and-workflows)
4. [Module 4: Custom Commands and Automation](#module-4-custom-commands-and-automation)
5. [Module 5: MCP (Model Context Protocol)](#module-5-mcp-model-context-protocol)
6. [Module 6: Hooks - Deterministic Control](#module-6-hooks---deterministic-control)
7. [Module 7: Sub-agents and Parallelization](#module-7-sub-agents-and-parallelization)
8. [Module 8: Working with Legacy Codebases](#module-8-working-with-legacy-codebases)
9. [Module 9: CI/CD and Pipeline Automation](#module-9-cicd-and-pipeline-automation)
10. [Module 10: Advanced Patterns and Optimization](#module-10-advanced-patterns-and-optimization)
11. [Additional Resources](#additional-resources)

---

## Module 1: Fundamentals
**Level: Beginner**

### 1.1 What is Claude Code?

Claude Code is an agentic coding tool that runs in your terminal. Unlike traditional AI assistants (copy-paste workflow), Claude Code:
- Directly edits files
- Executes shell commands
- Understands full project context
- Manages Git workflow

**Key philosophy:** Claude Code is intentionally low-level and unopinionated - it provides raw access to the model without forcing specific workflows.

### 1.2 Installation and Setup

**Official documentation (START HERE):**
- [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Setup Guide](https://docs.anthropic.com/en/docs/claude-code/setup)

**Installation steps:**
```bash
# Recommended installation (native binary)
curl -fsSL https://claude.ai/install.sh | bash

# Or via npm (alternative)
npm install -g @anthropic-ai/claude-code

# Verify installation
claude doctor

# Run in a project
cd your-project
claude
```

**Authentication options:**
- Claude Console (API billing)
- Claude Pro/Max subscription
- Enterprise: Amazon Bedrock, Google Vertex AI, Microsoft Foundry

### 1.3 Basic Commands and Navigation

**Slash commands to master:**

| Command | Description |
|---------|-------------|
| `/help` | Full command list |
| `/init` | Initialize project (creates CLAUDE.md) |
| `/clear` | Clear context (use FREQUENTLY!) |
| `/compact` | Compress conversation |
| `/cost` | Check token usage |
| `/model` | Change model |
| `/permissions` | Manage permissions |
| `/resume` | Resume previous session |
| `/rewind` | Rewind to previous point |

**Keyboard shortcuts:**
- `Escape` - interrupt Claude while working
- `Double Escape` - prompt history, navigate back
- `Shift+Tab` - toggle auto-accept mode
- `Control+V` - paste images (not Command+V!)
- `Up Arrow` - previous prompts

### Progress Checklist

- [ ] Read [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [ ] Read [Quickstart Guide](https://docs.anthropic.com/en/docs/claude-code/quickstart)
- [ ] Install Claude Code
- [ ] Run `claude doctor` successfully
- [ ] Practice basic slash commands
- [ ] Read [Cooking with Claude Code - Sid Bharath](https://www.siddharthbharath.com/claude-code-the-complete-guide/)
- [ ] Read [Getting Started - Quick Guide](https://fuszti.com/claude-code-setup-guide-2025/)
- [ ] Read [Builder.io - Best Tips](https://www.builder.io/blog/claude-code)
- [ ] Watch [Anthropic YouTube videos](https://www.youtube.com/@anthropic-ai)

---

## Module 2: Configuration and CLAUDE.md
**Level: Beginner-Intermediate**

### 2.1 Configuration File System

Claude Code uses hierarchical configuration:

```
~/.claude/
├── settings.json          # Global user settings
├── CLAUDE.md              # Global memory/instructions
└── commands/              # Global custom commands

project/
├── .claude/
│   ├── settings.json      # Project settings (git)
│   ├── settings.local.json # Local settings (gitignore)
│   └── commands/          # Project custom commands
├── CLAUDE.md              # Main project memory
└── subdirectory/
    └── CLAUDE.md          # Override for subfolder
```

**Priority:** Enterprise > CLI args > local project > shared project > user

### 2.2 CLAUDE.md - Your Project "Memory"

CLAUDE.md is the most important file - it's automatically included in the context of every conversation.

**What to include:**
```markdown
# Project: [Name]

## Tech Stack
- Frontend: React 18 + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL + Prisma
- Testing: Jest, React Testing Library

## Commands
- `npm run dev` - development server
- `npm run test` - tests
- `npm run lint` - linting
- `npm run typecheck` - type checking

## Code Conventions
- ES Modules (import/export), not CommonJS
- Destructured imports
- Functional components with hooks
- Tests alongside source files (.test.ts)

## Architecture
- Components: `src/components/`
- Utils: `src/utils/`
- API: `src/api/`

## Git Workflow
- Branch naming: `feature/`, `fix/`, `chore/`
- Conventional commits
- Squash merge to main

## Important Warnings
- DO NOT modify files in `/legacy/` without consultation
- API keys in .env (never commit!)
- Always run `npm run typecheck` before push
```

**Pro tips:**
- Run `/init` to have Claude generate a base CLAUDE.md
- Treat it as living documentation
- Update when you discover new patterns
- Use hierarchy - CLAUDE.md in subfolders for specific context

### 2.3 settings.json - Permissions and Configuration

```json
{
  "model": "claude-sonnet-4-20250514",
  "permissions": {
    "allowedTools": ["Read", "Write", "Bash(git *)"],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Write(./production.config.*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "python -m black $file"
          }
        ]
      }
    ]
  }
}
```

### Progress Checklist

- [ ] Read [Using CLAUDE.md Files](https://www.claude.com/blog/using-claude-md-files)
- [ ] Read [Configuration Guide](https://docs.anthropic.com/en/docs/claude-code/settings)
- [ ] Create CLAUDE.md for a project using `/init`
- [ ] Customize CLAUDE.md with project-specific info
- [ ] Set up settings.json with permissions
- [ ] Read [ClaudeLog - Configuration](https://claudelog.com/configuration/)
- [ ] Read [Shipyard Cheatsheet](https://shipyard.build/blog/claude-code-cheat-sheet/)
- [ ] Review [centminmod/my-claude-code-setup](https://github.com/centminmod/my-claude-code-setup) template

---

## Module 3: Effective Prompts and Workflows
**Level: Intermediate**

### 3.1 Magic Words - Extended Thinking

Claude Code maps special phrases to thinking budget:

| Phrase | Token Budget | Use Case |
|--------|-------------|----------|
| `"think"` | ~4,000 | Simple tasks |
| `"think hard"` | ~10,000 | Medium complexity |
| `"think harder"` | More | Complex problems |
| `"ultrathink"` | ~32,000 | Architecture, refactoring |

**Example:**
```
think hard about how to refactor the authentication module
to support OAuth2 alongside existing JWT auth
```

### 3.2 Plan → Code → Test Workflow

**Recommended Anthropic workflow:**

```
1. RESEARCH
   "Explore the codebase to understand how [X] works"

2. PLAN
   "Create a detailed plan for implementing [feature].
    Think hard. Don't write any code yet."

3. REVIEW PLAN
   Review the plan, give feedback

4. IMPLEMENT
   "Implement the plan. Verify each step as you go."

5. TEST
   "Write tests for the new functionality and run them"

6. COMMIT
   "Commit the changes and create a PR"
```

### 3.3 TDD with Claude Code

```
1. "Write failing tests for [feature] based on these
    input/output examples: [examples].
    Don't implement anything yet."

2. "Run the tests and confirm they fail."

3. "Commit the tests."

4. "Implement the minimum code to make tests pass."

5. "Iterate until all tests pass, then commit."
```

### 3.4 Context Management

**Rules:**
- Use `/clear` between tasks - this is CRITICAL
- Scope one session to one feature
- For large tasks: split into plan.md and checklists
- Avoid context overflow - compression loses information

**Context commands:**
```bash
# Compress context with focus
/compact "Focus on auth module implementation"

# Add directory to context
/add-dir ../shared-library

# Clear and start fresh
/clear
```

### 3.5 Work Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **Plan Mode** | Claude plans, doesn't code | Architecture, design |
| **Auto-accept** | No confirmations | Boilerplate, lint fixes |
| **Step-by-step** | Confirms each action | Default, safe |
| **Headless** | `-p` flag, non-interactive | CI/CD, scripting |

### Progress Checklist

- [ ] Read [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices) - **MUST READ!**
- [ ] Read [Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
- [ ] Practice extended thinking with "think hard" prompts
- [ ] Complete a Plan → Code → Test cycle
- [ ] Try TDD workflow with Claude
- [ ] Master `/clear` and `/compact` usage
- [ ] Read [Ultimate AI Coding Guide - Sabrina](https://www.sabrina.dev/p/ultimate-ai-coding-guide-claude-code)
- [ ] Read [ClaudeLog Best Practices](https://claudelog.com/)
- [ ] Read [NikiforovAll - Usage Best Practices](https://nikiforovall.blog/productivity/2025/06/13/claude-code-rules.html)

---

## Module 4: Custom Commands and Automation
**Level: Intermediate**

### 4.1 Creating Custom Commands

Custom commands are `.md` files in `.claude/commands/`:

```
.claude/commands/
├── fix-github-issue.md    # /project:fix-github-issue
├── review.md              # /project:review
├── create-prd.md          # /project:create-prd
└── security-audit.md      # /project:security-audit
```

**Example `fix-github-issue.md`:**
```markdown
Please analyze and fix the GitHub issue: $ARGUMENTS

Follow these steps:
1. Use `gh issue view` to get issue details
2. Understand the problem
3. Search codebase for relevant files
4. Implement the fix
5. Write and run tests
6. Ensure linting and type checking passes
7. Create descriptive commit message
8. Push and create a PR

Use GitHub CLI (`gh`) for all GitHub tasks.
```

**Usage:** `/project:fix-github-issue 1234`

### 4.2 Useful Ready-Made Commands

**review.md - Code Review:**
```markdown
Perform a comprehensive code review of recent changes:
1. Check code follows our conventions
2. Verify proper error handling
3. Ensure accessibility standards
4. Review test coverage
5. Check for security vulnerabilities
6. Validate performance implications
7. Confirm documentation updates

Use our code quality checklist and update CLAUDE.md
with any new patterns discovered.
```

**qplan / qcode / qcheck - Workflow commands:**
```markdown
# qplan.md
Analyze similar parts of the codebase and determine whether your plan:
- is consistent with rest of codebase
- introduces minimal changes
- reuses existing code

# qcode.md
Implement your plan and make sure your new tests pass.
Always run tests to verify you didn't break anything.
Run `prettier` on new files.
Run `turbo typecheck lint` for type checking.

# qcheck.md
You are a SKEPTICAL senior software engineer.
For every MAJOR code change, verify:
1. Writing Functions Best Practices from CLAUDE.md
2. Writing Tests Best Practices from CLAUDE.md
```

### 4.3 Global vs Project Commands

```
~/.claude/commands/        # Available in all projects
.claude/commands/          # Only in this project (git)
```

### Progress Checklist

- [ ] Read [Custom Commands Tutorial](https://docs.anthropic.com/en/docs/claude-code/tutorials#create-custom-slash-commands)
- [ ] Create first custom command
- [ ] Set up `fix-github-issue.md` command
- [ ] Set up `review.md` command
- [ ] Create project-specific workflow commands
- [ ] Review [zebbern/claude-code-guide](https://github.com/zebbern/claude-code-guide) cheatsheet
- [ ] Read [htdocs.dev - Best Practices](https://htdocs.dev/posts/claude-code-best-practices-and-pro-tips/)

---

## Module 5: MCP (Model Context Protocol)
**Level: Intermediate-Advanced**

### 5.1 What is MCP?

MCP is an open protocol standardizing AI connections to external data sources and tools. Think of it as "USB-C for AI".

**MCP offers:**
- **Resources** - file-like data (API responses, files)
- **Tools** - functions called by LLM
- **Prompts** - ready prompt templates

### 5.2 Configuring MCP in Claude Code

**`.mcp.json` file in project:**
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      }
    },
    "context7": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    },
    "microsoft.docs": {
      "type": "http",
      "url": "https://learn.microsoft.com/api/mcp"
    }
  }
}
```

**CLI commands:**
```bash
# Add MCP server
claude mcp add github --scope user

# List servers
claude mcp list

# Remove server
claude mcp remove github

# Debug
claude --mcp-debug
```

### 5.3 Popular MCP Servers

| Server | Use Case |
|--------|----------|
| **GitHub** | Issues, PRs, repo operations |
| **Slack** | Messages, channels |
| **Google Drive** | Documents, files |
| **PostgreSQL/MySQL** | Databases |
| **Puppeteer** | Web scraping, screenshots |
| **Sentry** | Error tracking |
| **Context7** | Real-time documentation |
| **Cloudflare** | Docs, Workers |

### 5.4 Creating Your Own MCP Server

**Minimal implementation (Python + FastMCP):**
```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def get_secret_number() -> int:
    """Returns the secret number."""
    return 42

if __name__ == "__main__":
    mcp.run()
```

### Progress Checklist

- [ ] Read [Model Context Protocol](https://modelcontextprotocol.io/)
- [ ] Read [MCP Quickstart](https://modelcontextprotocol.io/quickstart)
- [ ] Browse [MCP Servers Gallery](https://modelcontextprotocol.io/servers)
- [ ] Configure first MCP server (GitHub recommended)
- [ ] Test MCP server integration
- [ ] Read [Codecademy - MCP Tutorial](https://www.codecademy.com/article/how-to-use-model-context-protocol-mcp-with-claude-step-by-step-guide-with-examples)
- [ ] Read [DataCamp - MCP Demo Project](https://www.datacamp.com/tutorial/mcp-model-context-protocol)
- [ ] Read [MCPcat Guide](https://mcpcat.io/guides/adding-an-mcp-server-to-claude-code/)
- [ ] Explore [claube.ai - MCP Directory](https://www.claube.ai/)
- [ ] Try creating a custom MCP server

---

## Module 6: Hooks - Deterministic Control
**Level: Advanced**

### 6.1 What are Hooks?

Hooks are a mechanism for executing scripts on specific Claude Code events. They provide **deterministic** control in a probabilistic LLM environment.

**Event lifecycle:**
```
UserPromptSubmit → PreToolUse → [Tool execution] → PostToolUse → Stop
                      ↓
              PermissionRequest
```

### 6.2 Hook Types

| Event | When | Capabilities |
|-------|------|--------------|
| `UserPromptSubmit` | Before processing prompt | Validation, context injection |
| `PreToolUse` | Before tool execution | Blocking, input modification |
| `PostToolUse` | After tool execution | Formatting, tests, cleanup |
| `PermissionRequest` | On permission request | Auto-approve/deny |
| `Stop` | When Claude ends response | Cleanup, notifications |
| `Notification` | On notifications | Custom alerts |

### 6.3 Configuring Hooks

**In `settings.json`:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_FILE_PATHS\""
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/validate-bash-command.sh"
          }
        ]
      }
    ]
  }
}
```

### 6.4 Practical Examples

**Auto-formatting Python:**
```json
{
  "matcher": "Write(*.py)",
  "hooks": [{
    "type": "command",
    "command": "ruff check --fix $file && black $file"
  }]
}
```

**Auto-run tests after edit:**
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "npm run test -- --related $file"
  }]
}
```

**Blocking dangerous commands:**
```python
# validate-bash.py
import json
import sys

data = json.load(sys.stdin)
command = data.get("tool_input", {}).get("command", "")

dangerous = ["rm -rf /", "DROP TABLE", "> /dev/sda"]
if any(d in command for d in dangerous):
    print("BLOCKED: Dangerous command", file=sys.stderr)
    sys.exit(2)  # Exit code 2 = block
```

### 6.5 Hook Response Format

```json
{
  "continue": true,
  "stopReason": "Message when continue=false",
  "suppressOutput": false,
  "systemMessage": "Warning shown to user",
  "decision": "approve|block",
  "reason": "Explanation"
}
```

### Progress Checklist

- [ ] Read [Hooks Reference](https://docs.claude.com/en/docs/claude-code/hooks)
- [ ] Read [Hooks Getting Started](https://code.claude.com/docs/en/hooks-guide)
- [ ] Set up auto-formatting hook
- [ ] Set up auto-test hook
- [ ] Create a command validation hook
- [ ] Read [ClaudeLog - Hooks](https://claudelog.com/mechanics/hooks/)
- [ ] Read [GitButler - Claude Code Hooks](https://blog.gitbutler.com/automate-your-ai-workflows-with-claude-code-hooks)
- [ ] Review [disler/claude-code-hooks-mastery](https://github.com/disler/claude-code-hooks-mastery)
- [ ] Read [Apidog - Hooks Guide](https://apidog.com/blog/claude-code-hooks/)

---

## Module 7: Sub-agents and Parallelization
**Level: Advanced**

### 7.1 What are Sub-agents?

Sub-agents are specialized AI assistants with their own context, to which Claude can delegate tasks.

**Characteristics:**
- Own context window (isolation)
- Custom system prompt
- Limited/different tool permissions
- Parallel execution (max ~10 simultaneously)

### 7.2 Creating Sub-agents

**Via `/agents` command:**
```
/agents
→ Create new agent
→ Name: "code-reviewer"
→ Description: "Expert in code review..."
→ System prompt: [custom instructions]
→ Tools: Read only
→ Save
```

**Manually in `~/.claude/agents/` or `.claude/agents/`:**
```yaml
# code-reviewer.md
---
name: code-reviewer
description: Expert code reviewer focusing on quality
allowedTools: [Read, Grep, Glob]
model: sonnet
---

You are a senior code reviewer specializing in:
- Code quality and maintainability
- Security vulnerabilities
- Performance optimization
- Best practices

When reviewing:
1. Focus on critical issues first
2. Provide actionable feedback
3. Suggest specific improvements
4. Reference project conventions from CLAUDE.md
```

### 7.3 Using Sub-agents

**Automatic:** Claude decides when to delegate

**Explicit:**
```
Use the code-reviewer agent to review the changes in src/auth/
```

**Parallel exploration:**
```
Explore the codebase using 4 tasks in parallel.
Each agent should explore different directories:
- Agent 1: src/api/
- Agent 2: src/components/
- Agent 3: src/utils/
- Agent 4: tests/
```

### 7.4 Plan Agent (Built-in)

In Plan Mode, Claude automatically uses Plan subagent for research:
```
[Plan mode] Help me refactor the authentication module

→ Claude uses Plan agent for codebase exploration
→ Returns findings to main Claude
→ Claude presents plan
```

### 7.5 Parallelization with Git Worktrees

**Workflow for parallel development:**
```
project/
├── .claude/commands/
│   ├── init-parallel.md
│   └── exe-parallel.md
├── specs/
│   └── feature-spec.md
└── trees/
    ├── feature-impl-1/
    ├── feature-impl-2/
    └── feature-impl-3/
```

Each agent works in an isolated worktree, producing different implementations of the same specification.

### Progress Checklist

- [ ] Read [Subagents Documentation](https://code.claude.com/docs/en/sub-agents)
- [ ] Read [Building Agents with Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
- [ ] Create first sub-agent
- [ ] Test parallel exploration with multiple agents
- [ ] Set up code-reviewer sub-agent
- [ ] Try Git worktrees parallelization
- [ ] Read [How to Use Subagents - Zach Wills](https://zachwills.net/how-to-use-claude-code-subagents-to-parallelize-development/)
- [ ] Read [Subagent Deep Dive - Code Centre](https://cuong.io/blog/2025/06/24-claude-code-subagent-deep-dive)
- [ ] Read [Parallel AI Coding](https://docs.agentinterviews.com/blog/parallel-ai-coding-with-gitworktrees/)
- [ ] Read [AI Native Dev - Parallelizing Agents](https://ainativedev.io/news/how-to-parallelize-ai-coding-agents)
- [ ] Read [Multi-Agent Orchestration](https://dev.to/bredmond1019/multi-agent-orchestration-running-10-claude-instances-in-parallel-part-3-29da)
- [ ] Read [ClaudeLog - Task Agent Tools](https://claudelog.com/mechanics/task-agent-tools/)

---

## Module 8: Working with Legacy Codebases
**Level: Advanced**

### 8.1 Onboarding to New/Legacy Codebase

**Strategy:**
```
1. Run `/init` - Claude analyzes structure

2. Ask exploratory questions:
   "Explain the overall architecture of this project"
   "How does the authentication flow work?"
   "What are the main dependencies and why?"

3. Build CLAUDE.md iteratively:
   - Add discovered conventions
   - Document nuances and edge cases
   - Note warnings about problematic areas
```

### 8.2 Refactoring Strategies

**Incremental approach (RECOMMENDED):**
```
1. "Analyze the complexity hotspots in src/legacy/"
   → Claude identifies problematic files

2. "Create a refactoring plan for [file]. Think hard.
    Don't make any changes yet."
   → Review the plan

3. "Extract the first 50 lines of [function] into
    a separate utility. Run tests after."
   → Small, safe changes

4. Repeat, test, commit after each step
```

**Rules:**
- Small diffs, frequent commits
- Tests before refactoring
- Maintain backward compatibility
- Use Plan Mode for architectural decisions

### 8.3 Code Modernization

**Example tasks:**
```
"Update all class components to functional components with hooks"
"Migrate from CommonJS to ES Modules"
"Replace deprecated API calls with new equivalents"
"Add TypeScript types to the utils folder"
```

**Pro tip:** Use `--enable-architect` for complex modernizations.

### 8.4 Documenting Undocumented Code

```
"Read [file] and generate comprehensive documentation.
 Include:
 - Purpose of each function
 - Parameters and return types
 - Side effects and dependencies
 - Usage examples"
```

### Progress Checklist

- [ ] Read [Code Modernization](https://www.claude.com/solutions/code-modernization)
- [ ] Practice `/init` on an unfamiliar codebase
- [ ] Build iterative CLAUDE.md from exploration
- [ ] Complete incremental refactoring exercise
- [ ] Try code modernization task
- [ ] Read [Refactoring Large Projects](https://codenotary.com/blog/using-claude-code-and-aider-to-refactor-large-projects-enhancing-maintainability-and-scalability)
- [ ] Read [How to Use Claude Code for Refactoring](https://www.arsturn.com/blog/how-to-use-claude-code-to-refactor-and-clean-up-your-codebase)
- [ ] Read [Refactoring with Claude - Case Study](https://medium.com/@jbelis/refactoring-with-claude-b690a364d2f0)

---

## Module 9: CI/CD and Pipeline Automation
**Level: Advanced**

### 9.1 Headless Mode

```bash
# Non-interactive mode
claude -p "Summarize the changes in the last 5 commits"

# With output format
claude -p "Fix linting errors" --output-format stream-json

# With turn limit
claude --max-turns 3 -p "Add types to utils/"

# Pipe input
cat error.log | claude -p "Diagnose this error"
git diff HEAD~1 | claude -p "Review these changes"
```

### 9.2 GitHub Integration

**PR Reviews (auto):**
```bash
# Install GitHub app
/install-github-app

# Configuration in claude-code-review.yml:
direct_prompt: |
  Review this PR for bugs and security issues only.
  Be concise. Skip style nitpicks.
```

**Issue labeling:**
```bash
# In GitHub Actions
- name: Label new issue
  run: |
    claude -p "Analyze issue #${{ github.event.issue.number }} \
      and suggest appropriate labels" --output-format json
```

### 9.3 Pre-commit Hooks

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: claude-review
        name: Claude Code Review
        entry: claude -p "Review staged changes for critical issues"
        language: system
        pass_filenames: false
```

### 9.4 CI Pipeline Integration

```yaml
# GitHub Actions example
- name: Generate translations
  run: |
    claude -p "Find new text strings and translate to French. \
      Create PR for @lang-team to review." \
      --dangerously-skip-permissions

- name: Auto-fix issues
  run: |
    claude -p "Fix the failing test in ${{ github.event.issue.body }}"
```

### Progress Checklist

- [ ] Read [CLI Reference](https://docs.anthropic.com/en/docs/claude-code/cli-reference)
- [ ] Read [CI/CD Integration](https://docs.anthropic.com/en/docs/claude-code/ci-cd)
- [ ] Read [GitHub Integration](https://docs.anthropic.com/en/docs/claude-code/github)
- [ ] Try headless mode with `-p` flag
- [ ] Set up Claude in a GitHub Action
- [ ] Configure pre-commit hook with Claude
- [ ] Set up automated PR reviews
- [ ] Review [Claude Code Best Practices - CI section](https://www.anthropic.com/engineering/claude-code-best-practices)

---

## Module 10: Advanced Patterns and Optimization
**Level: Expert**

### 10.1 Cost and Token Optimization

**Strategies:**
- `/clear` aggressively between tasks
- Use hierarchical CLAUDE.md (smaller context)
- Sub-agents for research (isolated context)
- `/compact` with sensible summary
- Choose appropriate model (Haiku for simple tasks)

### 10.2 Security Modes

```bash
# Full automation (RISKY)
claude --dangerously-skip-permissions

# Safer: in Docker container
docker run -it --rm \
  -v $(pwd):/workspace \
  claude-sandbox \
  claude --dangerously-skip-permissions "Fix all linting errors"
```

### 10.3 Multi-model Workflow

```bash
# Opus for architecture
ANTHROPIC_MODEL="claude-opus-4-5-20251101" claude "Design the auth system"

# Sonnet for implementation
ANTHROPIC_MODEL="claude-sonnet-4-5-20250929" claude "Implement the plan"

# Haiku for quick fixes
ANTHROPIC_MODEL="claude-haiku-4-5-20251001" claude "Fix typos in comments"
```

### 10.4 VS Code Extension

The new VS Code extension (beta) offers:
- Graphical interface without terminal
- Inline diffs
- Sidebar integration
- Multi-pane parallel instances

### 10.5 Claude Code on the Web

```
https://claude.com/code
- Cloud-based sessions
- Parallel tasks across repos
- Auto PR creation
- No terminal needed
```

### Progress Checklist

- [ ] Read [Claude Code on the Web](https://www.anthropic.com/news/claude-code-on-the-web)
- [ ] Read [VS Code Extension docs](https://marketplace.visualstudio.com/items?itemName=anthropics.claude-code)
- [ ] Read [Security Documentation](https://docs.anthropic.com/en/docs/claude-code/security)
- [ ] Optimize token usage with aggressive `/clear`
- [ ] Try multi-model workflow
- [ ] Set up Docker-based sandbox
- [ ] Try VS Code extension
- [ ] Read [Simon Willison's Notes](https://simonwillison.net/2025/Apr/19/claude-code-best-practices/)
- [ ] Read [Vibe Coding Hub - Claude Code Review](https://vibecodinghub.org/tools/claude-code)

---

## Additional Resources

### Official Documentation

| Resource | Link |
|----------|------|
| Claude Code Docs | [docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview) |
| Claude Code Blog | [anthropic.com/engineering](https://www.anthropic.com/engineering) |
| GitHub Repo | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| MCP Protocol | [modelcontextprotocol.io](https://modelcontextprotocol.io/) |
| Release Notes | [docs.anthropic.com/en/docs/claude-code/release-notes](https://docs.anthropic.com/en/docs/claude-code/release-notes) |

### Community Resources

| Resource | Link | Description |
|----------|------|-------------|
| ClaudeLog | [claudelog.com](https://claudelog.com/) | Best practices, mechanics, guides |
| r/ClaudeAI | [reddit.com/r/ClaudeAI](https://reddit.com/r/ClaudeAI) | Reddit community |
| zebbern/claude-code-guide | [GitHub](https://github.com/zebbern/claude-code-guide) | Comprehensive cheatsheet |
| centminmod/my-claude-code-setup | [GitHub](https://github.com/centminmod/my-claude-code-setup) | Starter template |

### Video Resources

- Anthropic YouTube Channel
- AI Code King (YouTube) - parallel workflows
- @disler - hooks, worktrees tutorials

### Tools and Extensions

| Tool | Description |
|------|-------------|
| GitButler | Git worktrees integration |
| Verdent AI | Parallel agent orchestration |
| Claude Code VS Code Extension | IDE integration |
| context7 MCP | Real-time documentation |

---

## Mastery Checklist

**Overall Progress:**

- [ ] Configured CLAUDE.md in main projects
- [ ] 5+ custom commands in daily use
- [ ] At least 2 MCP servers configured
- [ ] Hooks for auto-formatting and tests
- [ ] Sub-agents for specialized tasks
- [ ] Plan → Code → Test workflow mastered
- [ ] CI/CD integration in at least one project
- [ ] Effective context management (/clear, /compact)
- [ ] Familiar with all work modes
- [ ] Token cost optimization

---

*Last updated: December 2025*
*Version: 1.0*

> "Claude Code is intentionally low-level and unopinionated, providing close to raw model access without forcing specific workflows. This design philosophy creates a flexible, customizable, scriptable, and safe power tool."
> — Anthropic Engineering
