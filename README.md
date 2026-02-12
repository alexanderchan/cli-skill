# Modern CLI Builder Skill

A Claude Code skill for generating modern command-line tools using TypeScript (Node.js or Bun) or Go, based on current best practices for LLM-assisted development.

## What This Skill Does

This skill helps you build production-ready CLI tools with:
- ✨ Modern tech stacks (TypeScript with Node.js/Bun or Go + Bubble Tea)
- 🛡️ Safety-first patterns (confirmations, --yes flags, validation)
- 🎨 Beautiful interactive prompts
- ⚙️ Proper configuration management
- 📦 Distribution best practices (npm packages or standalone binaries)

## Installation

```bash
npx skills add alexanderchan/cli-skill
```

## Usage

The skill is triggered automatically when you:
- Say "create a cli tool"
- Say "build a command-line tool"
- Use the `/cli` command

### Example Prompts

```
/cli
I need a CLI tool to manage my todo list with add, list, and complete commands.
```

```
Create a CLI tool in TypeScript that:
- Fetches data from an API
- Caches results locally
- Supports both interactive and non-interactive modes
```

```
Build a Go CLI that processes files in a directory with progress indicators
```

## What You'll Get

The skill will guide Claude to:
1. Choose the appropriate tech stack (TypeScript or Go)
2. Set up the project structure
3. Implement commands with proper argument parsing
4. Add safety checks and confirmation prompts
5. Support both interactive and automation modes
6. Follow configuration best practices
7. Include helpful error messages and examples

## Tech Stacks Supported

### TypeScript with Node.js
- @commander-js/extra-typings (command parsing)
- @clack/prompts (interactive prompts)
- zx (shell commands)
- Build tools: esbuild, tsdown, or bun build

### TypeScript with Bun
- Same libraries as Node.js (@commander-js/extra-typings, @clack/prompts, zx)
- Built-in bundler (no separate tool needed)
- Standalone executables with `--compile`
- 10-20x faster builds than esbuild

### Go
- Bubble Tea (TUI framework)
- Lip Gloss (styling)
- Huh (forms)
- modernc.org/sqlite (database)
- GoReleaser (cross-platform releases)

## Based On

This skill is based on the blog post: [Modern CLI Tips for LLMs](https://alexmchan.com/blog/2026-01-25-updated-cli)

## Contributing

Suggestions and improvements welcome! Open an issue or PR at [https://github.com/alexanderchan/cli-skill](https://github.com/alexanderchan/cli-skill).

## License

MIT
