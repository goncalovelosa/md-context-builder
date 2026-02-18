# Development Guide

This document provides comprehensive development guidance for [project name].

## Initial Setup

### Prerequisites

- [Prerequisite 1] with version [version]
- [Prerequisite 2] with version [version]
- [Prerequisite 3] - [optional/required]

### Installation

```bash
# Clone the repository
git clone [repo-url]
cd [project-name]

# Install dependencies
[install command]

# Setup environment variables
cp .env.example .env
# Edit .env with your values
```

## Monorepo Commands

### Root Level Commands

```bash
# Install all dependencies
[command]

# Run [task] across all packages
[command]

# Build all packages
[command]

# Test all packages
[command]

# Lint all packages
[command]
```

### Package-Specific Commands

```bash
# Run [task] in specific package
[command]

# Build specific package
[command]

# Test specific package
[command]
```

## Workspace Structure

### Root Configuration

- `package.json` - Root package.json with workspace configuration
- `[monorepo-config].json` - [Nx/Turbo/Lerna] configuration
- `tsconfig.base.json` - Base TypeScript configuration

### Package Organization

```
packages/
├── shared/          # Shared utilities and types
├── ui/              # Shared UI components
└── api-client/      # API client library

apps/
├── web/             # Web application
├── api/             # API server
└── worker/          # Background worker
```

## Development Workflow

### Starting Development

```bash
# Start all services in dev mode
[command]

# Start specific service
[command]

# Start multiple services
[command]
```

### Testing

```bash
# Run all tests
[command]

# Run tests for specific package
[command]

# Run tests in watch mode
[command]

# Run tests with coverage
[command]
```

### Building

```bash
# Build all packages
[command]

# Build specific package
[command]

# Build for production
[command]
```

## Code Quality

### Linting

- **Tool:** [ESLint/Pylint/Golangci-lint]
- **Config:** `[config path]`
- **Pre-commit hook:** [yes/no]

```bash
# Lint all files
[command]

# Lint specific package
[command]

# Auto-fix issues
[command]
```

### Formatting

- **Tool:** [Prettier/Black/gofmt]
- **Config:** `[config path]`

```bash
# Format all files
[command]

# Check formatting
[command]
```

### Type Checking

```bash
# Type check all packages
[command]

# Type check specific package
[command]
```

## Common Development Tasks

### Adding a New Dependency

```bash
# Add to root (shared by all)
[add command]

# Add to specific package
[add command]
```

### Adding a New Package

```bash
# Using [monorepo tool]
[command]

# Manual setup
[steps]
```

### Updating Dependencies

```bash
# Update all dependencies
[command]

# Update specific dependency
[command]

# Interactive update
[command]
```

## Service-Specific Development

### [Service 1]

See `.claude/docs/services/[service-name].md` for details on:

- Architecture
- Local development
- Testing
- Deployment

### [Service 2]

See `.claude/docs/services/[service-name].md` for details on:

See `.claude/docs/services/[service-name].md` for details on:

- Architecture
- Local development
- Testing
- Deployment

## Debugging

### Common Issues

1. **[Issue 1]**
   - Symptoms: [What you see]
   - Cause: [What causes it]
   - Solution: [How to fix]
   - Reference: `[path:line]`

2. **[Issue 2]**
   - Symptoms: [What you see]
   - Cause: [What causes it]
   - Solution: [How to fix]
   - Reference: `[path:line]`

### Debugging Tools

- **[Tool 1]:** [Purpose and usage]
- **[Tool 2]:** [Purpose and usage]

## CI/CD

### Pipeline Stages

1. **[Stage 1]:** [What it does]
2. **[Stage 2]:** [What it does]
3. **[Stage 3]:** [What it does]

### Running Locally

```bash
# Run CI pipeline locally
[command]
```

## Release Process

1. [Step 1]
2. [Step 2]
3. [Step 3]
