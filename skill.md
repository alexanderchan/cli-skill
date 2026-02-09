---
name: cli
description: Generate modern CLI tools using TypeScript (Node.js or Bun) or Go with best practices for LLM-assisted development. Use this skill when the user wants to create a CLI tool, build a command-line application, make a terminal utility, or set up CLI tooling. Helps with stack selection (Node.js/Bun/Go), build configuration, safety patterns (confirmations, dry-run), and distribution setup. Triggers on "create a cli", "build a cli tool", "make a command-line tool", or when CLI development is requested.
---

# Modern CLI Development Best Practices

You are an expert at building modern command-line tools following current best practices. Use this guide when creating CLI applications.

## Initial Questions

Before generating the CLI tool, ask the user about these key aspects:

1. **Language & Distribution**
   - TypeScript (quick iteration, Node.js ecosystems) or Go (standalone binaries, no runtime)?
   - Will this be distributed as an npm package or standalone binary?

2. **Interaction Mode**
   - Interactive prompts by default, or non-interactive?
   - Should destructive operations require confirmation?
   - Support for `--dry-run` flag to preview changes?

3. **Configuration & Defaults**
   - Does this tool need persistent configuration?
   - Where should config be stored (`~/.config/[name]`, project `.rc` file, both)?
   - What sensible defaults should be provided?
   - Support for environment variable overrides?

4. **Build & Distribution Setup**
   - Should I set up build tooling (tsup, esbuild, goreleaser)?
   - Need GitHub Actions for releases?
   - Want automated binary builds for multiple platforms?
   - Should include installation instructions (npm, homebrew, direct download)?

5. **Project Scope**
   - Is this a project-local script or a distributed tool?
   - Single command or multiple subcommands?
   - Need for plugins or extensibility?

## Core Stack Recommendations

### TypeScript with Node.js (Recommended for Node.js ecosystems)

Use these libraries for rapid CLI development with strong typing:

- **@commander-js/extra-typings** - Command parsing with inferred types
- **@clack/prompts** - Interactive prompts with beautiful UI
- **zx** - Shell command execution with JavaScript
- **chalk** - Terminal string styling (optional)

### TypeScript with Bun (Recommended for fast standalone executables)

Bun is a fast all-in-one JavaScript runtime with built-in TypeScript support:

- **Built-in bundler** - No need for tsup or esbuild
- **Compile to executables** - `bun build --compile` creates standalone binaries
- **Same libraries** - Use @commander-js/extra-typings, @clack/prompts, etc.
- **Fast startup** - Significantly faster than Node.js
- **No runtime needed** - Compiled binaries include the Bun runtime

### Go (Recommended for maximum portability)

Use these libraries for production CLIs:

- **Bubble Tea** - TUI framework for interactive applications
- **Lip Gloss** - Styling and layout for terminal output
- **Huh** - Forms and interactive inputs
- **modernc.org/sqlite** - SQLite database without C dependencies

## Why TypeScript Over Shell Scripts

Modern LLMs are significantly better at TypeScript and Go than bash. As complexity grows:

- Shell scripts become unmaintainable with complex logic
- TypeScript/Go offer better error handling and type safety
- Portability is easier (no bash/sh/zsh compatibility issues)
- Better tooling and testing support

## LLM Development Pattern

When asking an LLM to generate CLI code, provide this context once:

```
I'm building a CLI tool in [TypeScript/Go]. Use these libraries:
[List your chosen libraries from above]

Follow these guidelines:
- Use non-destructive operations with confirmation prompts by default
- Support --yes or --non-interactive flags for automation
- Store configuration in ~/.config/[app-name]/ (or OS equivalent)
- Include helpful error messages and usage examples
```

Modern LLMs know these libraries well and will generate working code with this simple prompt pattern.

## Key Best Practices

### Safety First
- Use confirmation prompts for destructive operations by default
- Implement `--yes` or `--non-interactive` flags for automation/CI
- Validate inputs before performing operations
- Provide clear error messages with recovery suggestions

### Configuration Management
- Store config in standard locations:
  - Linux/Mac: `~/.config/[app-name]/`
  - Windows: `%APPDATA%\[app-name]\`
- Support config file and environment variables
- Allow command-line flags to override config

### User Experience
- Provide helpful error messages
- Include progress indicators for long operations
- Support both interactive and non-interactive modes
- Show usage examples in help text
- Use colors and formatting thoughtfully (not excessively)

### Code Organization
- Separate project-local scripts from distributed CLIs
- Keep commands modular and testable
- Document installation and usage clearly

## Tool Selection Guide

**Choose TypeScript + Node.js when:**
- Building tools for Node.js/JavaScript projects
- Rapid prototyping is priority
- You need quick iteration with LLM assistance
- Users already have Node.js installed
- Distribution via npm is acceptable

**Choose TypeScript + Bun when:**
- You want TypeScript but need standalone executables
- Fast startup time is important
- You want simpler build setup (no separate bundler needed)
- Cross-platform binaries are needed (Bun compiles for Linux/macOS/Windows)
- You want the speed of Go but familiarity of TypeScript

**Choose Go when:**
- Maximum portability is required
- Smallest possible binary size is needed
- Building system-level tools
- Performance is absolutely critical
- You need the most mature cross-compilation tooling

## Project Structure & Build Setup

### TypeScript CLI with Node.js (Full Setup)
```
my-cli/
├── package.json           # Dependencies and scripts
├── tsconfig.json          # TypeScript configuration
├── tsup.config.ts         # Build configuration (optional)
├── .github/
│   └── workflows/
│       └── release.yml    # Automated releases
├── src/
│   ├── index.ts          # Entry point with commander setup
│   ├── commands/          # Individual command implementations
│   ├── config.ts          # Configuration management
│   └── utils/             # Shared utilities
├── bin/
│   └── cli.js             # Executable wrapper (#!/usr/bin/env node)
├── tests/                 # Test files
└── README.md
```

**Build Tools:**
- **tsup** - Fast TypeScript bundler (recommended for CLIs)
- **esbuild** - Alternative fast bundler
- **pkg** - Package as standalone executables (optional)

**package.json scripts:**
```json
{
  "bin": {
    "my-cli": "./bin/cli.js"
  },
  "scripts": {
    "build": "tsup src/index.ts --format esm,cjs --dts",
    "dev": "tsup src/index.ts --watch",
    "prepublishOnly": "npm run build"
  }
}
```

### TypeScript CLI with Bun (Standalone Executable)
```
my-cli/
├── package.json           # Dependencies (Bun compatible)
├── tsconfig.json          # TypeScript configuration (optional, Bun has defaults)
├── .github/
│   └── workflows/
│       └── release.yml    # Automated releases with Bun
├── src/
│   ├── index.ts          # Entry point
│   ├── commands/          # Individual command implementations
│   ├── config.ts          # Configuration management
│   └── utils/             # Shared utilities
├── build/                 # Compiled binaries
└── README.md
```

**Build with Bun:**
```json
{
  "scripts": {
    "build": "bun build src/index.ts --compile --outfile build/my-cli",
    "build:all": "bun run build:linux && bun run build:macos && bun run build:windows",
    "build:linux": "bun build src/index.ts --compile --target=bun-linux-x64 --outfile build/my-cli-linux",
    "build:macos": "bun build src/index.ts --compile --target=bun-darwin-arm64 --outfile build/my-cli-macos",
    "build:windows": "bun build src/index.ts --compile --target=bun-windows-x64 --outfile build/my-cli-windows.exe",
    "dev": "bun run src/index.ts"
  }
}
```

**Key Bun advantages:**
- No separate bundler needed - Bun has it built-in
- Fast compilation to standalone executables
- TypeScript works out of the box
- Compatible with most npm packages
- Smaller learning curve than Go for JS/TS developers

### Go CLI (Full Setup)
```
my-cli/
├── go.mod
├── go.sum
├── .goreleaser.yml        # Release automation
├── .github/
│   └── workflows/
│       └── release.yml    # Automated releases with GoReleaser
├── main.go                # Entry point
├── cmd/
│   ├── root.go           # Root command setup
│   └── subcommand.go     # Subcommands
├── internal/
│   ├── config/           # Configuration management
│   └── ui/               # UI components (Bubble Tea)
├── pkg/                   # Public packages
└── README.md
```

**Build Tools:**
- **GoReleaser** - Automated releases with cross-compilation
- **Task** - Modern Make alternative (optional)
- **Air** - Live reload for development (optional)

**Makefile or Taskfile.yml:**
```makefile
build:
	go build -o dist/my-cli main.go

install:
	go install

release:
	goreleaser release --clean
```

## Getting Started

When generating a new CLI tool:

1. **Choose your stack** based on distribution needs
2. **Set up the project** with recommended libraries
3. **Define commands** and their arguments
4. **Implement safety checks** and confirmations
5. **Add configuration support**
6. **Test both interactive and non-interactive modes**
7. **Write clear documentation**

## Common Patterns

### Confirmation Prompt (TypeScript with @clack/prompts)
```typescript
import * as p from '@clack/prompts';

const shouldContinue = await p.confirm({
  message: 'This will delete files. Continue?',
  initialValue: false
});

if (p.isCancel(shouldContinue) || !shouldContinue) {
  p.cancel('Operation cancelled');
  process.exit(0);
}
```

### Non-Interactive Flag Support
```typescript
const program = new Command()
  .option('-y, --yes', 'skip confirmation prompts')
  .option('--non-interactive', 'run without prompts');

// Later in code
if (!options.yes && !options.nonInteractive) {
  // Show confirmation prompt
}
```

### Config File Loading with Defaults
```typescript
import { readFile, mkdir, writeFile } from 'fs/promises';
import { join } from 'path';
import { homedir } from 'os';

const defaultConfig = {
  theme: 'auto',
  verbose: false,
  timeout: 30000
};

async function loadConfig() {
  const configDir = join(homedir(), '.config', 'my-cli');
  const configPath = join(configDir, 'config.json');

  try {
    const config = JSON.parse(await readFile(configPath, 'utf-8'));
    return { ...defaultConfig, ...config };
  } catch {
    // Create default config if it doesn't exist
    await mkdir(configDir, { recursive: true });
    await writeFile(configPath, JSON.stringify(defaultConfig, null, 2));
    return defaultConfig;
  }
}
```

### Dry Run Support
```typescript
const program = new Command()
  .option('--dry-run', 'preview changes without executing')
  .option('-y, --yes', 'skip confirmation prompts');

async function deleteFiles(files: string[], options: { dryRun?: boolean }) {
  if (options.dryRun) {
    console.log('Would delete:', files);
    return;
  }

  // Actually delete files
  for (const file of files) {
    await unlink(file);
  }
}
```

## Build & Release Automation

### TypeScript + Node.js with GitHub Actions
```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          registry-url: 'https://registry.npmjs.org'
      - run: npm ci
      - run: npm run build
      - run: npm test
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.NPM_TOKEN}}
```

### TypeScript + Bun with GitHub Actions
```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  build:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: oven-sh/setup-bun@v1
        with:
          bun-version: latest
      - run: bun install
      - run: bun test
      - run: bun build src/index.ts --compile --outfile my-cli-${{ runner.os }}
      - uses: actions/upload-artifact@v4
        with:
          name: my-cli-${{ runner.os }}
          path: my-cli-${{ runner.os }}*

  release:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
      - uses: softprops/action-gh-release@v1
        with:
          files: |
            my-cli-*/*
```

### Go with GoReleaser
```yaml
# .goreleaser.yml
builds:
  - env:
      - CGO_ENABLED=0
    goos:
      - linux
      - darwin
      - windows
    goarch:
      - amd64
      - arm64

archives:
  - format: tar.gz
    name_template: '{{ .ProjectName }}_{{ .Version }}_{{ .Os }}_{{ .Arch }}'

brews:
  - name: my-cli
    repository:
      owner: myorg
      name: homebrew-tap
    folder: Formula
    homepage: https://github.com/myorg/my-cli
    description: My awesome CLI tool
```

---

Remember: Modern LLMs excel at generating CLI code with these libraries. After gathering requirements through the initial questions, provide clear context about the chosen stack and let the LLM handle implementation details with these patterns in mind.
