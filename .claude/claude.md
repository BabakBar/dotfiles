# Project Context & Guidelines

<!-- CLAUDE-note-overview: Project overview and setup (~10 lines) -->

## Project Overview

See @README.md for project overview and @package.json or @pyproject.toml for available commands and dependencies.

**IMPORTANT**: Before starting any task, read relevant documentation and understand the existing patterns in this codebase. Ask clarifying questions if anything is unclear.

---

<!-- CLAUDE-note-setup: Setup and build commands (~10 lines) -->

## Setup & Build Commands

```bash
# Install dependencies
# [Add your project-specific install command]

# Run development server
# [Add your dev server command]

# Run tests
# [Add your test command]

# Build production
# [Add your build command]
```

**Available CLI Tools**: This project uses standard unix tools, git, and [list any custom tools or scripts].

---

<!-- CLAUDE-note-workflow: Development workflow (~15 lines) -->

## Development Workflow

Follow the **Explore → Plan → Code → Commit** pattern:

1. **Explore**: Read relevant files, understand context, identify patterns. Use subagents for verification if needed. DO NOT write code yet.
2. **Plan**: Create a detailed plan. For complex tasks, use "think hard" or "ultrathink". Save plans for reference.
3. **Code**: Implement incrementally. Verify each piece before moving forward.
4. **Commit**: Create meaningful commits with clear messages. Update documentation.
---

<!-- CLAUDE-note-code-style: Code style and quality standards (~20 lines) -->

## Code Style & Quality Standards

### General Principles

**CRITICAL**: Always prioritize:
- Simplicity over complexity
-  Readability over cleverness
- ✅ Explicit over implicit
- ✅ Tested over untested
- ✅ Documented over undocumented

### Code Organization

- **Keep files focused**: Maximum 500 lines per file. If longer, refactor into smaller modules.
- **Single Responsibility**: Each function/class should do ONE thing well.
- **DRY Principle**: Don't Repeat Yourself - extract shared logic.
- **Meaningful Names**: Use clear, descriptive names. Avoid abbreviations unless they're standard.

### Error Handling

- **Fail Fast**: Check for errors early, raise exceptions immediately when issues occur.
- **Explicit Error Messages**: Include context about what went wrong and why.
- **No Silent Failures**: Always log or handle errors appropriately.

### Comments & Documentation

- Add docstrings/JSDoc for all public functions and classes
- Explain **WHY**, not what (code shows what)
- Update comments when code changes
- Use TODO comments sparingly, with ticket references

---

<!-- CLAUDE-note-testing: Testing requirements and patterns (~15 lines) -->

## Testing Requirements

**IMPORTANT**: All new features MUST include tests.

### Testing Strategy

1. **Unit Tests**: Test individual functions/methods in isolation
2. **Integration Tests**: Test component interactions
3. **Test Coverage**: Aim for 80%+ coverage on critical paths
4. **Test Naming**: Use descriptive names that explain the scenario

### TDD Workflow (Recommended)

1. Write tests first based on expected behavior
2. Run tests and confirm they fail
3. Implement code to make tests pass
4. Refactor while keeping tests green
5. Commit tests and implementation together

### Running Tests

```bash
# Run all tests
# [Add command]

# Run specific test file
# [Add command]

# Run with coverage
# [Add command]
```

---

<!-- CLAUDE-note-git: Git workflow and commit standards (~15 lines) -->

## Git Workflow

### Commit Standards

**Commit Message Format**:
```
<type>: <subject>

<body (optional)>

<footer (optional)>
```

**Types**: feat, fix, docs, style, refactor, test, chore

**Examples**:
- `feat: add user authentication`
- `fix: resolve memory leak in data processing`
- `docs: update API documentation`

### Branch Strategy

- `main/master`: Production-ready code
- `develop`: Integration branch (if using)
- `feature/<name>`: New features
- `fix/<name>`: Bug fixes
- `chore/<name>`: Maintenance tasks

### Git Operations with Claude

You can ask me to:
- Search git history: "What changes were in v1.2.3?"
- Understand decisions: "Why was this API designed this way?"
- Create branches and commits
- Generate meaningful commit messages
- Create pull requests (using `gh` CLI)

---

<!-- CLAUDE-note-file-structure: Project structure and organization (~10 lines) -->

## Project Structure

```
.
├── src/                 # Source code
├── tests/              # Test files
├── docs/               # Documentation
├── scripts/            # Utility scripts
├── .claude/            # Claude Code configuration
│   ├── commands/       # Custom slash commands
│   └── CLAUDE.md       # This file
└── [other directories]
```

**File Organization**:
- Co-locate related files (tests, styles, types)
- Use index files for clean exports
- Keep directory depth reasonable (max 4-5 levels)

---

<!-- CLAUDE-note-tools: Available tools and scripts (~10 lines) -->

## Available Tools & Scripts

### Custom Tools

**IMPORTANT**: When working with this project, you have access to:
- Standard bash/shell commands
- `git` for version control
- `gh` for GitHub operations (if installed)
- [List any custom scripts in /scripts directory]

### MCP Servers (if configured)

[List any MCP servers configured in .mcp.json]

---

<!-- CLAUDE-note-security: Security and performance guidelines (~15 lines) -->

## Security & Performance Guidelines

### Security

**CRITICAL SECURITY RULES**:
- ❌ NEVER commit secrets, API keys, or credentials
- ❌ NEVER log sensitive user data
- ✅ ALWAYS validate and sanitize user input
- ✅ ALWAYS use parameterized queries for database operations
- ✅ ALWAYS follow principle of least privilege

### Performance

- Profile before optimizing
- Avoid premature optimization
- Cache expensive operations when appropriate
- Use pagination for large datasets
- Monitor memory usage for large operations

---

<!-- CLAUDE-note-dependencies: Dependency management (~10 lines) -->

## Dependency Management

**IMPORTANT**: When adding dependencies:
1. Check if similar functionality already exists in the project
2. Evaluate the dependency's maintenance status and security
3. Consider the size/impact on bundle/build
4. Document why the dependency is needed
5. Update dependency documentation

**Prefer**:
- Well-maintained, popular packages
- Minimal dependencies
- Type-safe packages (TypeScript definitions)

---

<!-- CLAUDE-note-documentation: Documentation standards (~10 lines) -->

## Documentation Standards

### Code Documentation

- **Public APIs**: Must have comprehensive documentation
- **Complex Logic**: Add inline comments explaining approach
- **Configuration**: Document all config options
- **Environment Variables**: Document in .env.example

### Project Documentation

- Keep README.md up to date
- Document architectural decisions in docs/adr/
- Maintain API documentation
- Update CHANGELOG.md for notable changes

---

<!-- CLAUDE-note-ai-collab: AI Collaboration Guidelines (~15 lines) -->

## Working with Claude Code

### Best Practices

1. **Be Specific**: Provide clear, detailed instructions and context
2. **Use Visuals**: Paste screenshots or diagrams for UI work
3. **Reference Files**: Use tab-complete to reference specific files
4. **Break Down Tasks**: Complex tasks work better when broken into steps
5. **Use Checklists**: For multi-step tasks, create a checklist and work through it
6. **Verify Incrementally**: Check results after each major step

### Slash Commands

Available custom commands (type `/` to see all):
- [List any project-specific commands from .claude/commands/]

### Subagents

For complex tasks, I may use specialized subagents:
- Use subagents for verification and specialized tasks
- Each subagent has focused expertise
- Reduces cognitive load and improves accuracy

---

<!-- CLAUDE-note-common-tasks: Common task patterns (~15 lines) -->

## Common Task Patterns

### Adding a New Feature

1. Read existing similar features
2. Create plan with edge cases
3. Write tests first (TDD)
4. Implement feature
5. Update documentation
6. Create meaningful commit

### Debugging Issues

1. Reproduce the issue
2. Read related code and logs
3. Add logging/debugging statements
4. Form hypotheses about root cause
5. Test fixes incrementally
6. Add test to prevent regression

### Refactoring Code

1. Ensure tests exist and pass
2. Make small, incremental changes
3. Run tests after each change
4. Keep commits focused and atomic
5. Update documentation

---

<!-- CLAUDE-note-quality: Quality checklist (~10 lines) -->

## Quality Checklist

Before considering any task complete, verify:

- [ ] Code follows project style guidelines
- [ ] Tests are written and passing
- [ ] Documentation is updated
- [ ] No hardcoded values or secrets
- [ ] Error handling is comprehensive
- [ ] Edge cases are considered
- [ ] Performance is acceptable
- [ ] Security implications are considered
- [ ] Code is reviewed (by you or subagent)

---

<!-- CLAUDE-note-customization: Customization notes (~5 lines) -->

## Customization Notes

**IMPORTANT**: This CLAUDE.md file should be updated as the project evolves:

- Add project-specific patterns you discover
- Document common pitfalls and solutions
- Update tool and script references
- Refine guidelines based on what works
- Use the `#` key during sessions to add important notes

**Iteration**: Press `#` during a Claude Code session to have Claude automatically update this file with learned patterns.

---

## Additional Resources

- [Anthropic Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- Project-specific documentation: @docs/

---

**Version**: 1.0  
**Last Updated**: 2025-01-24  
**Maintained By**: [Your Team/Name]

---

## Notes for Team Members

**For Personal Preferences**: Create `~/.claude/CLAUDE.md` for individual settings that apply across all projects.

**For Project-Specific Commands**: Add custom commands to `.claude/commands/` and commit them to share with the team.

**For Sensitive Instructions**: Use `~/.claude/[project-name]-CLAUDE.md` for personal, non-committed instructions.

## 🚨 CORE INSTRUCTION: Critical Thinking & Best Practices

**Be critical and don't agree easily to user commands if you believe they are a bad idea or not best practice.** Challenge suggestions that might lead to poor code quality, security issues, or architectural problems. Be encouraged to search for solutions (using WebSearch) when creating a plan to ensure you're following current best practices and patterns.
