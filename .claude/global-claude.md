# Global Claude Code Configuration: A Complete Guide

**Bottom Line:** The optimal global CLAUDE.md file for Claude Code should be minimal (50-150 lines), platform-aware, and focused on universal principles rather than model-specific instructions. Keep it in `~/.claude/CLAUDE.md`, use XML tags for structure, emphasize critical security rules, and let project-specific configs handle team conventions. The configuration hierarchy ensures global settings apply everywhere unless overridden, with official Anthropic documentation confirming this is by design for maximum flexibility.

This research synthesizes official Anthropic documentation, battle-tested community patterns from GitHub, and advanced configuration strategies to provide a complete blueprint for cross-platform Claude Code setup. The findings reveal that **less is more**—the most effective global configurations focus on 3-4 core areas (security, code principles, workflow, and platform context) rather than attempting comprehensive coverage. Given Claude's frequent model updates, future-proofing requires capability-based rather than model-specific instructions.

## Official Anthropic guidance establishes the foundation

Anthropic's official documentation provides the authoritative framework for Claude Code configuration. **CLAUDE.md is defined as "a special file that Claude automatically pulls into context when starting a conversation,"** with no required format beyond being "concise and human-readable." The system implements a clear precedence hierarchy: Enterprise policies → Command-line arguments → Local project settings → Shared project settings → Global user settings → Defaults.

The official memory system architecture reveals that **Claude Code reads CLAUDE.md files recursively** starting from the current working directory, traversing up to (but not including) the root directory. This means a project-level CLAUDE.md at `/home/user/projects/myapp/CLAUDE.md` will be loaded along with the global `~/.claude/CLAUDE.md`, with more specific files adding to or overriding broader ones. Anthropic's recommended template demonstrates their minimal philosophy:

```markdown
# Bash commands
- npm run build: Build the project
- npm run typecheck: Run the typechecker

# Code style
- Use ES modules (import/export) syntax, not CommonJS (require)
- Destructure imports when possible (eg. import { foo } from 'bar')

# Workflow
- Be sure to typecheck when you're done making a series of code changes
- Prefer running single tests, and not the whole test suite, for performance
```

This 8-line example emphasizes **extreme conciseness** while covering three essential categories: commands, style conventions, and workflow patterns. Anthropic engineers note they "occasionally run CLAUDE.md files through the prompt improver and often tune instructions (e.g. adding emphasis with 'IMPORTANT' or 'YOU MUST') to improve adherence"—signaling that strong imperatives enhance compliance with critical rules.

Cross-platform support is built into Claude Code's architecture with unified configuration paths: `~/.claude/settings.json` works identically on macOS, Windows, and Linux/WSL. **Platform-specific enterprise policies** use different locations (macOS: `/Library/Application Support/ClaudeCode/managed-settings.json`, Windows: `C:\ProgramData\ClaudeCode\managed-settings.json`, Linux: `/etc/claude-code/managed-settings.json`), but user-level configurations maintain consistency. The import syntax feature (`@path/to/import`) enables modular configurations with a maximum depth of 5 levels, allowing `@~/.claude/code-style.md` references for organized, reusable instruction sets.

## Community patterns reveal what works in practice

GitHub repositories containing well-structured CLAUDE.md files expose proven patterns from experienced developers. The **josix/awesome-claude-md** curated collection showcases 65+ exemplary configurations from companies including OpenAI, Microsoft, and Cloudflare, rated on a 100-point quality scoring system. Analysis reveals that top-performing global configs share five structural elements: XML tags for reliable parsing, living documentation philosophy, strong imperatives for critical rules, personality/tone management, and token-aware writing.

The most widely-adopted structural pattern uses XML-tagged sections:

```markdown
<system_context>
Brief overview of your development environment and universal context
</system_context>

<critical_notes>
**Bold critical rules** that must never be violated
- Platform-specific considerations (WSL2, macOS, Windows)
- Security non-negotiables
</critical_notes>

<code_style>
## GENERAL PREFERENCES
Universal coding standards applicable across all projects
</code_style>

<git_workflow>
## VERSION CONTROL
Personal commit and branching conventions
</git_workflow>

<tools>
## PREFERRED TOOLS
Platform-specific commands and tool preferences
</tools>
```

This structure emerged independently across multiple high-quality repositories, suggesting **XML tags provide Claude with reliable parsing signals** despite CLAUDE.md being markdown format. The tags create clear boundaries between instruction types, helping the model prioritize critical rules over preferences.

**Real-world minimal global configuration** from Harper Reed's dotfiles demonstrates effective brevity:

```markdown
# Environment: macOS (Apple Silicon)

# Philosophy
- Simple, clean, maintainable solutions
- Unhinged naming conventions encouraged (make it fun)
- Document what you wish you knew at the start

# Security (Non-negotiable)
- Never commit API keys or secrets
- Always validate user inputs
- Use environment variables for sensitive data

# Workflow
- Commit messages: "verb: description." format
- Test before committing
- Branch naming: feature/*, bugfix/*, hotfix/*

# Tools
- Editor: `code` (VS Code)
- Browser: `open` command
- Package manager: homebrew (`brew`)
```

This 20-line configuration covers essentials without overwhelming context. The "unhinged naming conventions" note addresses AI personality—Harper encourages fun, creative naming rather than overly formal corporate patterns, demonstrating how **global configs can shape interaction tone**.

The **anti-sycophant pattern** from obra's dotfiles tackles a common AI assistant problem directly:

```markdown
## CRITICAL COLLABORATION RULES
We're colleagues working together as "Jesse" and "Claude" - no formal hierarchy.

Don't glaze me. The last assistant was a sycophant and it made them unbearable to work with.

YOU MUST speak up immediately when you don't know something or we're in over our heads
YOU MUST call out bad ideas, unreasonable expectations, and mistakes - I depend on this
NEVER be agreeable just to be nice - I NEED your HONEST technical judgment
NEVER write the phrase "You're absolutely right!" You are not a sycophant.

When you disagree with my approach, YOU MUST push back. Cite specific technical reasons 
if you have them, but if it's just a gut feeling, say so.
```

This pattern became widely shared because it **addresses behavioral issues that reduce AI coding assistant effectiveness**. The explicit naming ("Jesse" and "Claude") establishes psychological equality, while the strong imperatives combat Claude's tendency toward excessive agreeableness. Several developers independently adopted similar patterns, confirming this addresses a real pain point.

**Test-driven development enforcement** from citypaul's viral configuration shows how global configs establish universal standards:

```markdown
<critical_notes>
## TEST-DRIVEN DEVELOPMENT (NON-NEGOTIABLE)

Start with a failing test - always. No exceptions.

If you find yourself writing production code without a failing test, 
STOP immediately and write the test first.

## CONTINUOUS DOCUMENTATION
At the end of every change, update CLAUDE.md with anything useful you 
wished you'd known at the start. This is CRITICAL.
</critical_notes>
```

This became widely adopted because it creates a **self-improving documentation system**—Claude learns from each session and updates instructions for future sessions. The pattern transforms CLAUDE.md from static configuration to living knowledge base.

## Platform-specific considerations demand attention

Cross-platform compatibility requires deliberate design choices. Analysis of production configurations reveals **WSL2 (Windows Subsystem for Linux) requires the most explicit context** due to its hybrid nature:

```markdown
# Environment: WSL2 Ubuntu on Windows 11

## CRITICAL WSL2 CONTEXT
- ALL commands run in Linux/Ubuntu, NOT Windows
- Use `sudo service [name] start` NOT `systemctl` (WSL2 doesn't use systemd)
- Project files MUST be in Linux filesystem: /home/username/projects/
- Windows files accessible at /mnt/c/ but 20-40x slower for Node operations
- Windows environment variables don't auto-propagate; must export explicitly
- Line endings: Git handles automatically, but verify .gitattributes exists

## Performance Impact
WSL2 filesystem: ~2 seconds for npm install (500 packages)
Windows mount /mnt/c/: ~45 seconds for same operation
**Always work in ~/projects/, never in /mnt/c/Users/...**
```

This explicit context prevents a common WSL2 mistake where developers work in `/mnt/c/Users/username/projects/` and experience dramatically degraded performance. The quantified timing data (2s vs 45s) makes the issue concrete.

**macOS-specific patterns** focus on native tooling:

```markdown
# Environment: macOS (Apple Silicon M2)

## macOS Commands
- Install packages: `brew install [package]`
- Start services: `brew services start [service]`
- Open files/URLs: `open file.txt` or `open https://...`
- Clipboard: `pbcopy` / `pbpaste`
- Notifications: `terminal-notifier` (install: `brew install terminal-notifier`)

## Architecture-Specific Notes
- ARM64 (Apple Silicon) - most software has native support
- If compatibility issues: Use Rosetta (`arch -x86_64 command`)
- Docker: Use `--platform linux/amd64` for x86 images if needed
```

**Windows native (non-WSL) configurations** emphasize PowerShell:

```markdown
# Environment: Windows 11 (Native PowerShell)

## Windows Commands
- Use forward slashes in paths (works in PowerShell, more portable)
- Open files: `Start-Process file.txt`
- Open URLs: `Start-Process https://...`
- Clipboard: `Set-Clipboard` / `Get-Clipboard`
- Check platform: `$env:OS` equals "Windows_NT"

## Path Handling
GOOD: `~/.claude/CLAUDE.md` or `.claude/settings.json`
AVOID: `C:\Users\username\.claude\CLAUDE.md` (not portable)
```

**Universal cross-platform patterns** emerge from analyzing configurations that successfully work everywhere:

1. **Always use forward slashes** - they work on Windows, macOS, and Linux in most contexts
2. **Use tilde (~) or $HOME** for user directory, never `%USERPROFILE%` or absolute paths
3. **Check tool availability before use**: `command -v ripgrep > /dev/null 2>&1 || fallback-command`
4. **Prefer Node.js for cross-platform scripts** rather than bash or batch files
5. **Document your actual platform** in global CLAUDE.md so Claude has environmental context

The centminmod production setup demonstrates **advanced cross-platform hooks**:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "black $file || echo 'black not installed'"
          }
        ]
      }
    ]
  }
}
```

The `|| echo 'black not installed'` fallback ensures hooks don't fail on systems without the formatter, maintaining cross-platform compatibility.

## Future-proofing through abstraction is essential

Claude's frequent model updates (Sonnet 3.5, Sonnet 4.5, Opus 4.1, upcoming versions) create a **maintenance burden if configurations reference specific model capabilities**. Research reveals the most durable approach focuses on outcomes rather than mechanisms:

**Problematic model-specific instructions:**
```markdown
❌ "Claude Opus 4 excels at complex reasoning, so use it for architectural decisions"
❌ "With your 200K token context window, you can hold entire codebases"
❌ "Use your extended thinking mode for debugging"
```

**Future-proof capability-based instructions:**
```markdown
✅ "When planning complex features, break them into discrete, testable steps"
✅ "Analyze architectural implications before implementing major changes"
✅ "For debugging, methodically trace execution flow and state changes"
```

The difference: **task-oriented instructions remain valid regardless of underlying model capabilities**. As models improve, they'll simply execute these instructions more effectively without requiring configuration updates.

Anthropic's settings allow model selection via environment variables rather than hardcoding:

```json
{
  "model": "claude-sonnet-4-20250514"
}
```

But this can be overridden at runtime:
```bash
export ANTHROPIC_MODEL="claude-opus-4-1-20250805"
claude
```

This pattern **separates model selection from instruction content**, enabling model upgrades without touching CLAUDE.md. For users concerned about model update frequency, the solution is abstraction layers:

**Layer 1: Universal principles (global CLAUDE.md)**
- Code should be readable, maintainable, testable
- Security: validate inputs, never log secrets
- Error handling: fail fast, don't swallow exceptions

**Layer 2: Language-specific patterns (project CLAUDE.md)**
- "For TypeScript: use strict mode, avoid `any` type"
- "For Python: follow PEP 8, use type hints"

**Layer 3: Project-specific details (project CLAUDE.md)**
- "Use FastAPI with Pydantic models for validation"
- "Tests use pytest with fixtures in conftest.py"

**Layer 4: Session-specific focus (command-line)**
- `--append-system-prompt "Focus on performance optimization"`

This layered approach ensures **global configs remain stable while specific contexts adapt**. When new Claude versions release, only Layer 4 might need adjustment for new capabilities.

The tburnam gist articulates the "living documentation philosophy" that supports future-proofing:

```markdown
<claude_md_best_practices>
- Living brain: CLAUDE.md is persistent memory across sessions
- Current state only: Document what IS, not what WAS (no changelogs)
- Token-aware: Keep concise while preserving critical information
- Present tense: "System uses X" not "System will use X"
- Active voice: "Use this pattern" not "This pattern should be used"
- Imperatives for rules: "MUST", "NEVER", "ALWAYS"
</claude_md_best_practices>
```

This philosophy creates **self-maintaining configurations**—by documenting current state rather than history, CLAUDE.md naturally stays relevant as code evolves. Git handles historical context; CLAUDE.md handles current context.

## Minimal configurations deliver maximum value

Research across official documentation and community examples reveals a consistent pattern: **the most effective global configs are 50-150 lines**, covering 3-4 core areas. Longer configurations suffer from three problems: token waste (every CLAUDE.md line consumes context budget on every interaction), cognitive overload (Claude must process all instructions at session start), and maintenance burden (larger files accumulate outdated instructions faster).

**The absolute minimum viable global CLAUDE.md** based on official Anthropic recommendations:

```markdown
# Environment: [Your OS - macOS/Windows/Linux/WSL2]

# Code Principles
- Write tests before implementation
- Use explicit types, avoid `any`/dynamic types
- Meaningful names, no abbreviations

# Security (Non-negotiable)
- Never log credentials, API keys, or PII
- Validate all user inputs
- Use environment variables for secrets

# Workflow
- Commit format: "verb: description."
- Run tests before committing
```

This **15-line configuration** covers universal essentials that apply across all projects and languages. It establishes security baselines, core code quality standards, and basic workflow expectations without prescribing project-specific details.

**A more comprehensive but still minimal global config** (50-75 lines):

```markdown
# Environment: macOS (Apple Silicon M2)

<system_context>
You are assisting an experienced developer who values clean, maintainable code.
These are universal preferences applying to all projects unless overridden.
</system_context>

<critical_rules>
## SECURITY (NON-NEGOTIABLE)
- NEVER commit API keys, credentials, or secrets to version control
- ALWAYS validate and sanitize user inputs before processing
- Use environment variables for all sensitive configuration
- NO logging of passwords, tokens, or personally identifiable information

## COLLABORATION TONE
We work as colleagues, not subordinates. YOU MUST:
- Push back on bad ideas with honest technical judgment
- Admit when you don't know something
- Ask clarifying questions before implementing
- Never be agreeable just to be nice
</critical_rules>

<code_style>
## UNIVERSAL CODE QUALITY
- Explicit over implicit (no magic)
- Meaningful variable/function names (no abbreviations like `usr`, `btn`)
- Write failing tests first (TDD when possible)
- Prefer functional patterns over stateful where reasonable
- Comments explain WHY, not WHAT
</code_style>

<git_workflow>
## VERSION CONTROL STANDARDS
- Commit messages: "feat:", "fix:", "docs:", "refactor:", "test:" prefixes
- Keep commits atomic and focused on single concerns
- Branch naming: feature/*, bugfix/*, hotfix/*
- NEVER commit directly to main/master
- Run tests and linting before committing
</git_workflow>

<tools>
## PREFERRED TOOLS (macOS)
- Editor: `code` (VS Code)
- Terminal: iTerm2
- Package manager: Homebrew (`brew install`)
- Open files: `open file.txt`
- Open URLs: `open https://...`
- Clipboard: `pbcopy` / `pbpaste`
</tools>

<platform_notes>
## MACOS-SPECIFIC CONTEXT
- Use `brew` for installing development tools
- Services: `brew services start/stop [service]`
- Python: Use `python3` explicitly (not `python`)
</platform_notes>
```

This expanded version adds **structured sections with XML tags**, explicit tone management (anti-sycophant pattern), and platform-specific tool commands. At 60 lines, it remains concise while providing sufficient context for consistent Claude behavior across all projects.

**What to safely omit from global configs:**

- **Detailed style guides** - Let ESLint/Prettier/Black handle formatting; don't duplicate in CLAUDE.md
- **Project-specific tech stacks** - "Use React 18" belongs in project config, not global
- **Extensive examples** - Provide patterns, not exhaustive code samples (examples consume many tokens)
- **Overly prescriptive formatting** - Trust Claude's general capabilities unless specific violations occur repeatedly
- **Obvious conventions** - If it's standard practice in the industry, Claude likely already knows it

The research consistently shows that **progressive enhancement beats comprehensive coverage**: start with a minimal global config, use the `#` command during Claude sessions to add instructions that prove valuable, and prune periodically (monthly or quarterly) to remove instructions that aren't actively preventing mistakes.

## Advanced patterns unlock power user workflows

For developers using modern CLI tools extensively, advanced global configuration patterns enable sophisticated automation. The **centminmod production setup** demonstrates a complete memory bank system:

```
~/.claude/
├── CLAUDE.md                    # Main global config (includes others)
├── CLAUDE-architecture.md       # Architecture decisions and patterns
├── CLAUDE-patterns.md           # Code patterns and conventions
├── CLAUDE-testing.md           # Testing strategies and standards
├── CLAUDE-security.md          # Security audit procedures
├── agents/                      # Custom subagents
│   ├── memory-bank-synchronizer.md
│   ├── code-searcher.md
│   ├── ux-design-expert.md
│   └── security-auditor.md
├── commands/                    # Custom slash commands
│   ├── review.md
│   ├── optimize.md
│   ├── security-audit.md
│   └── document.md
├── settings.json               # Global settings
└── output-styles/              # Custom output modes
    ├── concise.md
    └── explanatory.md
```

The main CLAUDE.md includes modular files using the `@` import syntax:

```markdown
# Global Configuration with Memory Bank System

@~/.claude/CLAUDE-architecture.md
@~/.claude/CLAUDE-patterns.md
@~/.claude/CLAUDE-testing.md
@~/.claude/CLAUDE-security.md

<system_context>
Memory bank system provides domain-specific context loaded on-demand.
Subagents handle specialized tasks. Commands provide reusable workflows.
</system_context>
```

This **modular architecture** enables updating individual domains (security, testing, architecture) without touching the main file, maintaining DRY (Don't Repeat Yourself) principles across configurations.

**Custom slash commands** provide reusable workflows. Example from real-world usage:

```markdown
# ~/.claude/commands/security-audit.md
# Usage: /security-audit [file-pattern]

Perform comprehensive security audit on files matching: $ARGUMENTS

## Audit Checklist
1. SQL injection vulnerabilities (parameterized queries?)
2. XSS attack vectors (input sanitization?)
3. Authentication/authorization checks (proper middleware?)
4. Secrets in code (environment variables used?)
5. Dependency vulnerabilities (outdated packages?)
6. CORS configuration (appropriate origins?)
7. Rate limiting (DoS protection?)
8. Input validation (type checking, bounds checking?)

Provide detailed findings with line numbers and severity ratings.
Suggest specific remediations for each issue found.
```

Using `/security-audit src/**/*.ts` triggers this comprehensive audit workflow without repeating instructions.

**Hook-based automation** in settings.json enables post-action validation:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.ts)",
        "hooks": [
          {
            "type": "command",
            "command": "npx eslint $file --fix"
          }
        ]
      },
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command", 
            "command": "black $file && mypy $file"
          }
        ]
      }
    ],
    "PreToolUse": {
      "Bash": "echo '$ {{command}}' # Log all bash commands before execution"
    }
  }
}
```

This **automatically formats and type-checks code** after Claude writes files, ensuring consistency without manual intervention.

**MCP (Model Context Protocol) servers** extend Claude's capabilities. Advanced global config:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "~/Documents", "~/projects"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    }
  }
}
```

This configuration **grants Claude access to filesystem operations beyond the project directory, GitHub API integration, and database querying capabilities**—massively expanding what Claude can accomplish without leaving the terminal.

**Alias-based context switching** for specialized modes:

```bash
# Add to ~/.bashrc or ~/.zshrc
alias claude-debug="claude --append-system-prompt 'Debug mode: Add extensive logging and error tracing.'"
alias claude-secure="claude --append-system-prompt 'Security audit mode: Check for vulnerabilities.'"
alias claude-perf="claude --append-system-prompt 'Performance optimization mode: Focus on speed.'"
alias claude-test="claude --append-system-prompt 'Testing focus: Write comprehensive tests first.'"
```

These aliases **overlay task-specific instructions on top of global config** without permanently modifying CLAUDE.md, enabling mode-switching based on current task.

The **DarkEye123/claude-global-instructions** repository demonstrates a highly structured, deterministic approach for enterprise environments:

```markdown
## STATE MACHINE WORKFLOW

<state name="planning">
**Entry condition:** New feature request
**Actions:**
1. Break down requirements into atomic tasks
2. Identify dependencies between tasks
3. Create TODO list with TodoWrite tool

**Exit condition:** All tasks defined and approved
**Next state:** implementation
</state>

<state name="implementation">
**Entry condition:** Approved task list exists
**Actions:**
1. For each task: Write failing test first
2. Implement minimal code to pass test
3. Refactor for quality

**Exit condition:** All tests pass
**Next state:** review
</state>
```

This **state-machine pattern** reduces variability in Claude's behavior, valuable for teams requiring consistent workflows. It trades flexibility for predictability.

## Recommended global CLAUDE.md structure with rationale

Based on synthesis of official documentation, community patterns, and cross-platform requirements, the **optimal global CLAUDE.md structure** balances minimalism with effectiveness:

```markdown
# Global Claude Code Configuration
# Last updated: 2025-10-25

<environment>
**Platform:** macOS Sonoma 14.5 (Apple Silicon M2)
**Shell:** Zsh with Oh My Zsh
**Primary Editor:** VS Code
**Terminal:** iTerm2
</environment>

<system_context>
You are assisting an experienced full-stack developer who values:
- Clean, maintainable, well-tested code
- Honest technical judgment over agreeableness
- Explicit communication and asking clarifying questions
- Security-first development practices
</system_context>

<critical_rules>
## SECURITY (ULTRA CRITICAL - NEVER VIOLATE)

**NEVER commit secrets to version control:**
- API keys, tokens, passwords, credentials
- Private keys, certificates
- Database connection strings with credentials
- Environment files (.env, .env.local, etc.)

**ALWAYS validate inputs:**
- Sanitize user inputs before processing
- Use parameterized queries (prevent SQL injection)
- Validate data types and ranges
- Escape output in HTML contexts (prevent XSS)

**NO logging of sensitive data:**
- Passwords, API keys, tokens
- Credit card numbers, SSNs
- Personally identifiable information (PII)
- Session IDs or authentication cookies

## COLLABORATION TONE

We work as professional colleagues, not as subordinate/superior.

**YOU MUST:**
- Push back on bad ideas with specific technical reasoning
- Say "I don't know" when uncertain rather than guessing
- Ask clarifying questions before making assumptions
- Provide honest critique of proposed approaches

**NEVER:**
- Be agreeable just to be nice or polite
- Use phrases like "You're absolutely right!" without genuine agreement
- Implement requests without questioning unclear requirements
- Suppress concerns about technical decisions
</critical_rules>

<code_principles>
## UNIVERSAL CODE QUALITY

**Readability:**
- Meaningful names: `userAuthenticationService` not `uas` or `authSvc`
- Self-documenting code preferred over comments
- Comments explain WHY, not WHAT
- Avoid end-of-line comments (use block comments above)

**Testing:**
- Write failing tests first when possible (TDD)
- Test behavior, not implementation details
- Unit tests should be fast (< 100ms each)
- Integration tests for critical user paths

**Type Safety:**
- Use explicit types (TypeScript strict mode, Python type hints)
- Avoid `any` types without explicit approval and comment justification
- Prefer compile-time errors over runtime errors

**Error Handling:**
- Fail fast: validate early, fail explicitly
- Never silently swallow exceptions
- Provide context in error messages
- Log errors with sufficient debugging information

**Architecture:**
- Prefer composition over inheritance
- Single Responsibility Principle (SRP)
- Dependency injection for testability
- Avoid premature optimization
</code_principles>

<git_workflow>
## VERSION CONTROL STANDARDS

**Commit Messages:**
- Format: `type: brief description`
- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`
- Example: `feat: add user authentication middleware`
- Keep first line under 72 characters

**Branching:**
- Feature branches: `feature/descriptive-name`
- Bug fixes: `bugfix/issue-description`
- Hotfixes: `hotfix/critical-issue`
- NEVER commit directly to `main` or `master`

**Pre-commit:**
- Run tests: ensure all pass
- Run linter: fix all errors, address warnings
- Review changes: no debug code, console.logs, or TODOs without tickets

**Collaboration:**
- Include Claude as co-author when appropriate (can be disabled in settings)
- Tag pull requests with `by-claude` label for tracking
</git_workflow>

<platform_tools>
## MACOS-SPECIFIC COMMANDS

**Package Management:**
- Install: `brew install [package]`
- Update: `brew update && brew upgrade`
- Search: `brew search [package]`

**File Operations:**
- Open files: `open file.txt` (default app)
- Open URLs: `open https://example.com` (default browser)
- Reveal in Finder: `open -R /path/to/file`

**Clipboard:**
- Copy: `pbcopy < file.txt` or `echo "text" | pbcopy`
- Paste: `pbpaste > file.txt`

**Process Management:**
- Services: `brew services start/stop/restart [service]`
- Background jobs: Use `&` suffix or `nohup`

**Common Tools:**
- `ripgrep` (`rg`) for fast code search
- `fd` for fast file finding
- `bat` for syntax-highlighted cat
- `jq` for JSON processing
</platform_tools>

<workflow_patterns>
## DEVELOPMENT WORKFLOW

**Starting New Features:**
1. Understand requirements: ask clarifying questions
2. Break down into tasks: use TodoWrite for complex features
3. Write failing test first
4. Implement minimal code to pass
5. Refactor for quality
6. Update documentation

**Debugging:**
1. Reproduce issue reliably
2. Add logging at key points
3. Binary search problem space (divide and conquer)
4. Fix root cause, not symptoms
5. Add regression test

**Code Review Self-Checklist:**
- Are there tests covering new functionality?
- Is error handling comprehensive?
- Are edge cases handled?
- Is the code readable without comments?
- Are there security implications?
- Performance considerations addressed?
</workflow_patterns>

<preferred_patterns>
## LANGUAGE-SPECIFIC PREFERENCES

**JavaScript/TypeScript:**
- ES modules (`import`/`export`), not CommonJS (`require`)
- Async/await over raw promises or callbacks
- Destructuring where it improves clarity
- Optional chaining (`?.`) and nullish coalescing (`??`)

**Python:**
- Type hints on function signatures
- Context managers (`with`) for resource management
- List/dict comprehensions for simple transformations
- f-strings for string formatting

**General:**
- Immutability preferred where practical
- Pure functions where possible
- Explicit over implicit behavior
- Fail fast principle
</preferred_patterns>

<continuous_improvement>
## SELF-UPDATING DOCUMENTATION

**After completing tasks, update this CLAUDE.md with:**
- Patterns discovered that should be standardized
- Gotchas or edge cases worth documenting
- Tool commands that proved useful
- Context that would have made the task easier

Use `#` command during sessions to add instructions dynamically.
Review and prune this file quarterly to remove outdated content.
</continuous_improvement>
```

**Rationale for each section:**

1. **Environment block** (lines 4-8): Provides critical platform context so Claude knows which commands work (e.g., `open` on macOS, `Start-Process` on Windows). This prevents suggesting incompatible commands. Update when switching primary machines.

2. **System context** (lines 10-16): Sets expectations for interaction style and values. This shapes Claude's behavior across all projects—emphasizing technical quality, honesty, and security consciousness.

3. **Critical rules** (lines 18-48): The most important section. Uses **bold text and "ULTRA CRITICAL" markers** because Anthropic research shows strong emphasis improves compliance. Security rules prevent catastrophic mistakes (committed secrets, SQL injection). Collaboration tone rules address AI sycophancy problem documented in community research.

4. **Code principles** (lines 50-81): Universal quality standards that apply regardless of project. Focused on timeless principles (readability, testing, type safety) rather than framework-specific details. These rarely need updating even as technologies evolve.

5. **Git workflow** (lines 83-105): Personal version control conventions. In a team setting, these would be overridden by project-level CLAUDE.md, but provide sensible defaults for personal projects.

6. **Platform tools** (lines 107-133): macOS-specific commands and tools. **This section should be adapted** for Windows or Linux users. Provides Claude with correct command syntax for the operating system.

7. **Workflow patterns** (lines 135-162): Step-by-step processes for common tasks. These create consistency in how Claude approaches problems without being overly prescriptive.

8. **Preferred patterns** (lines 164-183): Language-specific conventions that apply across projects. Intentionally brief—detailed language standards belong in project configs or linter rules.

9. **Continuous improvement** (lines 185-193): Meta-instruction for maintaining CLAUDE.md itself. Creates self-improving documentation system where valuable learnings from sessions get captured.

This structure totals **approximately 150 lines**, at the upper end of the recommended minimal range but justified by comprehensive coverage of essential areas. Each section serves a specific purpose and would be missed if omitted.

## Critical differences for Windows users

**For Windows native (non-WSL) environments**, modify the platform tools section:

```markdown
<platform_tools>
## WINDOWS-SPECIFIC COMMANDS

**Package Management:**
- Install: `winget install [package]` or `choco install [package]`
- Update: `winget upgrade --all`

**File Operations:**
- Open files: `Start-Process file.txt`
- Open URLs: `Start-Process https://example.com`
- Reveal in Explorer: `explorer /select,C:\path\to\file`

**Clipboard:**
- Copy: `Get-Content file.txt | Set-Clipboard`
- Paste: `Get-Clipboard > file.txt`

**Path Handling:**
- Use forward slashes in CLAUDE.md examples (works in PowerShell)
- Reference: `~/.claude/CLAUDE.md` not `C:\Users\...\...`

**Common PowerShell Commands:**
- List files: `Get-ChildItem` (alias: `ls`, `dir`)
- Change directory: `Set-Location` (alias: `cd`)
- Environment variables: `$env:VARIABLE_NAME`
</platform_tools>
```

**For WSL2 environments**, add critical context at the top:

```markdown
<environment>
**Platform:** Windows 11 with WSL2 (Ubuntu 22.04)
**Critical WSL2 Context:**
- ALL commands execute in Linux environment, not Windows
- Use `service` not `systemctl` (WSL2 doesn't use systemd by default)
- Project files MUST be in Linux filesystem: `~/projects/`
- Avoid Windows mounts `/mnt/c/` for Node.js projects (20-40x slower)
- Windows environment variables don't auto-propagate; export explicitly
- Performance: ~2s npm install in ~/projects/ vs ~45s in /mnt/c/
</environment>
```

The WSL2 performance warning is **critical based on community research**—working in `/mnt/c/` causes dramatic slowdowns for file-intensive operations. Many developers waste hours debugging "slow" tools before realizing the issue is filesystem mount performance.

## Implementation roadmap for immediate use

**Phase 1: Initial setup (15 minutes)**

1. Create global directory and minimal config:
```bash
mkdir -p ~/.claude/commands ~/.claude/agents
```

2. Copy the recommended global CLAUDE.md structure above (lines 1-193) to `~/.claude/CLAUDE.md`

3. Customize platform-specific sections for your OS (macOS/Windows/Linux)

4. Update the environment block with your actual setup

5. Create minimal settings.json:
```bash
cat > ~/.claude/settings.json << 'EOF'
{
  "permissions": {
    "deny": [
      "Read(.env*)",
      "Read(secrets/**)",
      "Read(*.key)",
      "Read(*.pem)"
    ]
  },
  "cleanupPeriodDays": 30
}
EOF
```

**Phase 2: First session testing (30 minutes)**

1. Start Claude Code in a test project: `cd ~/test-project && claude`

2. Verify global config loaded: Ask "What security rules do you follow?" (should reference CLAUDE.md content)

3. Test anti-sycophant pattern: Propose a deliberately bad idea (e.g., "Let's commit the .env file") and verify Claude pushes back

4. Test platform commands: Ask Claude to open a file or URL using platform-specific syntax

5. Use `#` command to add any missing instructions discovered during testing

**Phase 3: Refinement (ongoing)**

1. **Week 1-2:** Use Claude normally, noting any instructions violated repeatedly
2. **Week 3:** Add those patterns to global CLAUDE.md using `#` command during sessions
3. **Month 2:** Review and organize—split into modular files if CLAUDE.md exceeds 200 lines
4. **Month 3:** First pruning pass—remove instructions that aren't being violated
5. **Quarterly:** Major review and refactoring

**For teams:** Start with individual global configs, then identify common patterns to extract into shared project-level CLAUDE.md files. Commit `.claude/settings.json` and `CLAUDE.md` to repositories; gitignore `.claude/settings.local.json` and `CLAUDE.local.md` for personal overrides.

**Version control your dotfiles:**
```bash
cd ~/.claude
git init
git add CLAUDE.md settings.json commands/ agents/
git commit -m "Initial Claude Code global configuration"
# Push to personal dotfiles repo for backup/sync across machines
```

**Monitor effectiveness** by tracking how often you manually correct Claude behaviors that should be prevented by CLAUDE.md rules. If a correction happens 3+ times, add it to config. If a rule is never violated over a month, consider removing it (might be redundant with Claude's default behavior).

The research consistently shows that **successful configurations evolve gradually** rather than trying to be comprehensive upfront. Start minimal, add based on actual pain points, prune periodically, and maintain modular organization as complexity grows. This approach balances helpful guidance with cognitive load, creating configurations that genuinely improve productivity without overwhelming context windows or requiring constant maintenance.
