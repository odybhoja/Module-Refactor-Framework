
# Module Refactor Framework

A powerful framework for automating code refactoring and module restructuring across programming languages and frameworks.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-success.svg)](#)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](#)

## Overview

The Module Refactor Framework provides automated tools and recipes for:
- **Framework Migrations**: Update dependencies and migrate between framework versions
- **Security Fixes**: Apply common security patches and best practices automatically
- **Code Modernization**: Transform legacy code patterns to modern equivalents
- **Style Consistency**: Enforce coding standards across large codebases
- **Language Translation**: Convert code between programming languages (experimental)

Inspired by projects like [OpenRewrite](https://github.com/openrewrite/rewrite), this framework focuses on making repeatable, auditable code transformations.

## Features

- 🚀 **Automated Refactoring**: Pre-packaged recipes for common refactoring tasks
- 🔧 **Multi-Language Support**: Works with Java, Kotlin, TypeScript, Python, and more
- 📦 **Package Manager Integration**: Understands `package.json`, `pom.xml`, `build.gradle`, etc.
- ✅ **Lossless Transformations**: Preserves original formatting and style
- 🔍 **AST-Based Analysis**: Uses Lossless Semantic Trees for precise code understanding
- 📊 **Audit Trail**: Track all changes made during refactoring sessions

## Installation

### Prerequisites

- Node.js 16+ or Python 3.8+
- Git
- [Optional] Docker for containerized execution

### Install via npm

```bash
npm install -g module-refactor-framework
```

### Install from source

```bash
git clone https://github.com/odybhoja/Module-Refactor-Framework.git
cd Module-Refactor-Framework
npm install
npm run build
npm link
```

## Quick Start

```bash
# Refactor a directory
mrf refactor ./my-project --target=nodejs-v18

# Apply security fixes
mrf apply security-fixes ./my-project

# Run custom recipes
mrf run recipes/custom.yaml ./my-project
```

## Configuration

Create a `mrf.config.json` in your project root:

```json
{
  "sourceDir": "./src",
  "targetDir": "./refactored",
  "recipes": ["framework-upgrade", "security-hardening"],
  "preserveFormatting": true,
  "dryRun": false
}
```

## Available Recipes

| Recipe | Description | Languages |
|--------|-------------|-----------|
| `framework-upgrade` | Migrate between framework versions | Node.js, React, Vue |
| `dependency-update` | Update package dependencies | npm, yarn, pip |
| `security-fixes` | Apply security patches | All |
| `modernize-syntax` | Convert legacy to modern syntax | JavaScript, TypeScript, Python |
| `test-generation` | Generate unit tests from code | Multiple |

## Known Issues

- ⚠️ Files exceeding 7000 tokens may have chunking issues
- ⚠️ Limited support for fundamental language concept differences (e.g., mutex locks in Rust vs JavaScript)
- ⚠️ Package manager equivalence lookups are in progress

See [ISSUES.md](ISSUES.md) for the complete list.

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspiration from [OpenRewrite](https://openrewrite.org/)
- Built with ❤️ by a builder

## Contact

- Project Maintainer: @odybhoja
- Issues: [GitHub Issues](https://github.com/odybhoja/Module-Refactor-Framework/issues)
- Discussions: [GitHub Discussions](https://github.com/odybhoja/Module-Refactor-Framework/discussions)
```

