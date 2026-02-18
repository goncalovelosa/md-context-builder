# Pattern Extraction

This document provides guidance for extracting patterns and conventions from a codebase when generating CLAUDE.md files.

## What to Extract

Focus on patterns that are:

1. **Consistent** - Used throughout the codebase
2. **Important** - Violating them causes issues
3. **Non-obvious** - Not common knowledge or easily inferred
4. **Universal** - Apply to most tasks in the project

## Import/Export Conventions

### JavaScript/TypeScript

**Named exports vs default exports:**

```bash
# Check for patterns
grep -r "export " src/ | head -20
grep -r "export default" src/ | head -20
```

**Import alias patterns:**

```bash
# Check for consistent aliasing
grep -r "import.*as.*from" src/ | head -20
```

**Path aliases (from tsconfig.json):**

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@utils/*": ["./src/utils/*"]
    }
  }
}
```

**Barrel exports (index files):**

```bash
# Look for index files that re-export
find src/ -name "index.ts" -o -name "index.js"
```

### Python

**Import patterns:**

```bash
# Check relative vs absolute imports
grep -r "^from \." src/
grep -r "^from \.\." src/
```

**Import organization:**

- Standard library imports
- Third-party imports
- Local imports

## Naming Conventions

### File Naming

**Check patterns:**

```bash
# List source files to identify patterns
find src/ -type f -name "*.ts" -o -name "*.js" -o -name "*.py" | head -30
```

**Common patterns:**

- `kebab-case.ts` (React components, utilities)
- `PascalCase.ts` (React components, classes)
- `camelCase.ts` (hooks, services)
- `snake_case.py` (Python modules)

### Variable/Function Naming

**Check patterns:**

```bash
# Look for function declarations
grep -r "function " src/ | head -20
grep -r "const.*=.*function" src/ | head -20
grep -r "def " src/ | head -20
```

**Common patterns:**

- `camelCase` for variables and functions (JS/TS)
- `PascalCase` for classes and components (JS/TS)
- `snake_case` for variables and functions (Python)
- `SCREAMING_SNAKE_CASE` for constants

### Component/Class Naming

**React components:**

- Prefix with `use` for hooks
- Prefix with `Context` for contexts
- Component files match export names

**Classes:**

- PascalCase for class names
- Private methods prefixed with `_`

## File Organization Patterns

### Directory Structure

**Common patterns:**

```
# Feature-based organization
src/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── api/
│   └── dashboard/
│       ├── components/
│       ├── hooks/
│       └── api/

# Type-based organization
src/
├── components/
├── hooks/
├── utils/
├── services/
└── types/

# Layered organization
src/
├── controllers/
├── services/
├── repositories/
├── models/
└── routes/
```

**Detection:**

```bash
# Examine directory structure
tree -L 2 -I node_modules src/
```

### Co-location Patterns

**Look for:**

- Components with their styles in same directory
- Tests alongside source files (`*.test.ts`, `*.spec.ts`)
- Components with their hooks
- Feature files grouped together

## Common Patterns to Extract

### API Call Patterns

**Check for:**

```bash
# Look for fetch/axios/api calls
grep -r "fetch(" src/ | head -10
grep -r "axios\." src/ | head -10
grep -r "api\." src/ | head -10
```

**Document:**

- How API calls are made (fetch, axios, custom client)
- Base URL configuration
- Authentication handling
- Error handling patterns

### State Management Patterns

**Check for:**

```bash
# Look for state management
grep -r "useState" src/ | head -5
grep -r "useRedux" src/ | head -5
grep -r "createContext" src/ | head -5
grep -r "atom" src/ | head -5  # Jotai/Zustand
grep -r "signal" src/ | head -5  # Preact signals
```

**Document:**

- State management library (Redux, Zustand, Context, etc.)
- Store organization
- Selector patterns
- Action creator patterns

### Routing Patterns

**Check for:**

```bash
# Look for routing
grep -r "useRouter" src/ | head -5
grep -r "Link" src/ | head -5
grep -r "router\." src/ | head -5
```

**Document:**

- Router library (React Router, Next.js, etc.)
- Route definitions location
- Protected route patterns
- Navigation patterns

### Error Handling Patterns

**Check for:**

```bash
# Look for error handling
grep -r "try {" src/ | head -10
grep -r "catch" src/ | head -10
grep -r "throw new" src/ | head -10
```

**Document:**

- Error class hierarchy
- Error boundary usage (React)
- Error logging patterns
- User notification patterns

### Testing Patterns

**Check for:**

```bash
# Look for test patterns
grep -r "describe(" tests/ | head -10
grep -r "it(" tests/ | head -10
grep -r "test(" tests/ | head -10
```

**Document:**

- Test organization (unit, integration, e2e)
- Mock patterns
- Fixture patterns
- Test utilities

## "Do Not" Candidates

### Generated Files

**Always include:**

- `dist/`, `build/`, `out/` - Build output
- `node_modules/` - Dependencies
- `.next/`, `.nuxt/` - Framework build
- `*.generated.ts`, `*.generated.js` - Code generation
- `coverage/` - Test coverage

**Detection:**

```bash
# Check for common build dirs
ls -la | grep -E "(dist|build|out)"
cat .gitignore | grep -E "^(dist|build|out|node_modules)"
```

### Config Files (Usually Don't Edit Manually)

**Look for:**

- Lock files (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`)
- Generated config (often has comments saying "do not edit")
- Cache files

### Legacy/Deprecated Code

**Look for:**

- Files with `@deprecated` comments
- Files in `legacy/` or `deprecated/` directories
- Old versions of files (e.g., `component.old.ts`)

**Detection:**

```bash
# Find deprecated comments
grep -r "@deprecated" src/

# Find legacy directories
find . -type d -name "*legacy*" -o -name "*deprecated*"
```

### Framework-Managed Files

**Look for:**

- Framework auto-generated files
- Convention-based routes (don't add routes manually)
- Config that affects multiple files

**Examples:**

- Next.js: Don't manually add to `app/` without understanding conventions
- Rails: Don't modify auto-generated migrations
- Django: Don't edit auto-generated admin files

## Anti-Patterns to Document

**Common issues in the codebase:**

```bash
# Look for TODO comments about patterns
grep -r "TODO:" src/ | grep -i "pattern\|convention\|don't\|avoid"

# Look for FIXME comments
grep -r "FIXME:" src/

# Look for hacky patterns
grep -r "HACK\|XXX" src/
```

## Environment-Specific Patterns

### Environment Variables

**Check for:**

```bash
# Look for env usage
grep -r "process.env" src/ | head -10
grep -r "import\.meta\.env" src/ | head -10
grep -r "os\.getenv" src/ | head -10
```

**Document:**

- `.env.example` location
- Required vs optional variables
- Variable naming convention
- How to add new variables

### Configuration

**Check for:**

```bash
# Look for config files
find . -name "*.config.*" -o -name "config.*" -o -name "*.rc"
```

**Document:**

- Where configuration lives
- How to override for different environments
- Config validation patterns

## Code Style Patterns (Only If No Linter)

**Only document if:**

- No linter/formatter configured
- Highly unusual convention
- Critical to code correctness

**Otherwise:**

- Reference the linter config
- "Defer to [linter] for style"

## Pattern Extraction Workflow

### 1. Survey the Codebase

```bash
# Get overview
find src/ -type f | head -30
tree -L 2 src/

# Check for config files
ls -la | grep -E "config|rc"
```

### 2. Identify Consistent Patterns

Look for patterns repeated across:

- Multiple files
- Multiple developers
- Multiple features

### 3. Check for Deviations

If a pattern has many exceptions, it's not a pattern - don't document it.

### 4. Verify Importance

Ask: "What happens if someone doesn't follow this?"

- If nothing: Don't document
- If confusion: Consider documenting
- If breakage: Document as "Do Not"

### 5. Extract Examples

For each pattern:

- Find a clear example
- Use file:line references
- Explain the "why" if not obvious

## Documentation Templates

### Import Pattern Documentation

```
**Import Conventions:**
- Use absolute imports with `@/` alias for internal modules
- Use relative imports only for files in same directory
- Order: std lib → third-party → internal
- See `src/utils/example.ts:12` for example
```

### Naming Pattern Documentation

```
**Naming Conventions:**
- Components: PascalCase (`UserProfile.tsx`)
- Hooks: `use` prefix (`useUserData.ts`)
- Utilities: camelCase (`formatDate.ts`)
- Constants: SCREAMING_SNAKE_CASE (`API_BASE_URL`)
```

### File Organization Documentation

```
**File Organization:**
- Feature-based: `features/[feature]/components/`, `features/[feature]/hooks/`
- Tests co-located: `*.test.ts` next to source
- See `features/auth/` for reference structure
```

## What NOT to Extract

**Don't document:**

### 5. Extract Examples

For each pattern:

- Find a clear example
- Use file:line references
- Explain the "why" if not obvious

## Documentation Templates

### Import Pattern Documentation

```
**Import Conventions:**
- Use absolute imports with `@/` alias for internal modules
- Use relative imports only for files in same directory
- Order: std lib → third-party → internal
- See `src/utils/example.ts:12` for example
```

### Naming Pattern Documentation

```
**Naming Conventions:**
- Components: PascalCase (`UserProfile.tsx`)
- Hooks: `use` prefix (`useUserData.ts`)
- Utilities: camelCase (`formatDate.ts`)
- Constants: SCREAMING_SNAKE_CASE (`API_BASE_URL`)
```

### File Organization Documentation

```
**File Organization:**
- Feature-based: `features/[feature]/components/`, `features/[feature]/hooks/`
- Tests co-located: `*.test.ts` next to source
- See `features/auth/` for reference structure
```

## What NOT to Extract

**Don't document:**

- Personal preferences
- Inconsistent patterns
- One-off occurrences
- Obvious conventions (e.g., "functions do things")
- Framework defaults (stick to framework docs)
- Style covered by linters/formatters

**Remember:** CLAUDE.md is for context Claude doesn't already have. Focus on project-specific, non-obvious patterns.
