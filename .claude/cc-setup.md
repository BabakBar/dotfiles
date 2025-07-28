# Claude Code Configuration Best Practices

This project supports advanced Claude Code environment variable management for optimal developer experience, security, and performance.

## 🏠 Global Configuration (recommended for all projects)

Set your personal defaults once on each machine:

- **Mac/Linux**:  
  Add to `~/.claude/settings.json`:
  ```json
  {
    "env": {
      "CLAUDE_CODE_DISABLE_FINE_GRAINED_TOOL_STREAMING": "1",
      "CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR": "1",
      "CLAUDE_CODE_ENABLE_UNIFIED_READ_TOOL": "1",
      "DISABLE_PROMPT_CACHING": "1",
      "ENABLE_BACKGROUND_TASKS": "1",
      "FORCE_AUTO_BACKGROUND_TASKS": "1",
      "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
    }
  }
  ```
- **Windows**:  
  Place the above `settings.json` file at `%USERPROFILE%\.claude\settings.json`.

- **Shell-level (optional):**  
  For CLI-only use, export vars in your `~/.bashrc`, `~/.zshrc`, or (Windows) set as user environment variables.

## 🗂️ Project-Level Overrides

This repo includes a `.claude/settings.json` file. Claude will merge it with your global config, using project settings when there are conflicts.

- To customize for this project, edit `.claude/settings.json` in the repo root.
- For most users, you don’t need to change this unless your workflow for this repo differs from your global defaults.

## 🔄 Keeping Configs in Sync

- Use a dotfiles manager (e.g. [chezmoi](https://www.chezmoi.io/)) to sync your global `~/.claude/settings.json` across devices.
- Store project-level overrides in your project repo for team consistency.

## 🛠️ Advanced: Per-Session Customization

You can launch Claude Code with custom settings for just one session:
```sh
env ENABLE_BACKGROUND_TASKS=1 FORCE_AUTO_BACKGROUND_TASKS=1 claude
```
Or use shell functions/aliases for convenience (see `bin/ccv.sh`).

## 📝 References

- [Claude Code Env Vars Reference](https://github.com/thevibeworks/claude-code-docs/blob/main/content/env-vars/claude-code-env-vars.md)
- [Dotfiles Best Practices](https://dotfiles.github.io/)

---