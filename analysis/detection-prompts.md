# Detection Prompts

This document provides prompts and guidance for detecting project characteristics when analyzing a codebase for CLAUDE.md generation.

## Detection Checklist

Use these checks to systematically identify project characteristics.

## Git Repository Detection

### Repository Verification

**Check if git repository:**

```bash
# Check for .git directory
git rev-parse --git-dir 2>/dev/null
```

**Get commit count:**

```bash
# Count total commits
git rev-list --count HEAD 2>/dev/null
```

**Health indicators:**

| Metric    | Healthy        | Caution     | Skip Analysis |
| --------- | -------------- | ----------- | ------------- |
| Commits   | > 100          | 20-100      | < 10          |
| Activity  | Last 30 days   | Last 90 days| > 90 days stale|
| Depth     | Full clone     | Shallow     | N/A           |

**Confirmation prompts:**

- "Is `.git` directory present?"
- "What is the total commit count?"
- "When was the most recent commit?"
- "Is this a full clone or shallow clone?"

**When to skip git analysis:**

- Not a git repository (no `.git` directory)
- Empty/new repository (< 10 commits)
- User explicitly opts out
- Shallow clone (note limited history in output)

**State tracking for incremental updates:**

Check for existing CLAUDE.md timestamp:

```bash
# Extract last update timestamp for incremental updates
grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //'
```

This enables showing only NEW commits since the last CLAUDE.md generation.

### Git History Commands

**Recent activity (first generation - no timestamp exists):**

```bash
# Files changed in last 90 days
git log --name-only --pretty=format: --since="90 days ago" | sort | uniq -c | sort -rn | head -20
```

**Recent activity (subsequent updates - using timestamp):**

```bash
# Get only NEW commits since last update
LAST_UPDATE=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')
git log --name-only --pretty=format:"%h|%ad|%s" --date=format:"%Y-%m-%d %H:%M" --since="$LAST_UPDATE" | head -50
```

**Historical hotspots:**

```bash
# Most frequently changed files (all time)
git log --name-only --pretty=format: | sort | uniq -c | sort -rn | head -30
```

**File coupling (for large projects):**

```bash
# Files that change together within commits
git log --name-only --pretty=format:COMMIT_SEPARATOR --since="90 days ago" | \
  awk '/COMMIT_SEPARATOR/ {if (count > 1) print prev; count=0; prev=""} \
  NF {if ($0 != "COMMIT_SEPARATOR") {files[count++]=$0; prev=prev$0" "}}' | \
  grep -v '^$' | sort | uniq -c | sort -rn | head -20
```

**See also:** `git-history-analysis.md` for comprehensive git analysis guidance.

## Package Manager Detection

### JavaScript/TypeScript Projects

**Check order:**

1. Look for lock files in root directory
2. Check `package.json` for `packageManager` field
3. Examine any CI/CD configuration files

| Lock File           | Package Manager |
| ------------------- | --------------- |
| `yarn.lock`         | Yarn            |
| `package-lock.json` | npm             |
| `pnpm-lock.yaml`    | pnpm            |
| `bun.lockb`         | Bun             |

**Confirmation prompts:**

- "Which lock file exists in the root directory?"
- "What does `package.json` specify in the `packageManager` field?"
- "Which package manager commands are used in CI/CD files?"

### Python Projects

| File                             | Tool       |
| -------------------------------- | ---------- |
| `poetry.lock` + `pyproject.toml` | Poetry     |
| `Pipfile` + `Pipfile.lock`       | Pipenv     |
| `requirements.txt`               | pip        |
| `setup.py`                       | setuptools |

### Go Projects

| File                | Tool       |
| ------------------- | ---------- |
| `go.mod` + `go.sum` | Go modules |

### Rust Projects

| File                        | Tool  |
| --------------------------- | ----- |
| `Cargo.toml` + `Cargo.lock` | Cargo |

### Ruby Projects

| File                       | Tool    |
| -------------------------- | ------- |
| `Gemfile` + `Gemfile.lock` | Bundler |

### Java Projects

| File                                | Tool   |
| ----------------------------------- | ------ |
| `pom.xml`                           | Maven  |
| `build.gradle` / `build.gradle.kts` | Gradle |

## Framework Detection

### JavaScript/TypeScript Frameworks

**React:**

- `react`, `react-dom` in dependencies
- JSX/TSX files
- `src/App.jsx` or `src/App.tsx`

**Next.js:**

- `next` in dependencies
- `pages/` or `app/` directory
- `next.config.js` or `next.config.mjs`

**Vue:**

- `vue` in dependencies
- `.vue` files
- `src/main.js` or `src/main.ts`

**Svelte:**

- `svelte` in dependencies
- `.svelte` files
- `svelte.config.js`

**Angular:**

- `@angular/core` in dependencies
- `angular.json`
- `.ts` files with decorators

**Nuxt:**

- `nuxt` in dependencies
- `nuxt.config.ts`
- `pages/` directory

**Astro:**

- `astro` in dependencies
- `astro.config.mjs`
- `.astro` files

**Express:**

- `express` in dependencies
- Routing patterns like `app.get()`, `app.post()`

**Fastify:**

- `fastify` in dependencies
- `fastify.get()`, `fastify.post()`

**NestJS:**

- `@nestjs/core` in dependencies
- `@Controller()` decorators
- `app.module.ts`

### Python Frameworks

**Django:**

- `django` in dependencies
- `settings.py`, `urls.py`, `wsgi.py`
- `manage.py`
- App directories with `models.py`, `views.py`

**FastAPI:**

- `fastapi` in dependencies
- `@app.get()`, `@app.post()` decorators
- `pydantic` models

**Flask:**

- `flask` in dependencies
- `@app.route()` decorators

### Ruby Frameworks

**Rails:**

- `rails` in dependencies
- `app/`, `config/`, `db/` directories
- `config/routes.rb`

### Go Frameworks

**Gin:**

- `github.com/gin-gonic/gin` in dependencies
- `router.GET()`, `router.POST()`

**Echo:**

- `github.com/labstack/echo/v4` in dependencies
- `e.GET()`, `e.POST()`

## Build Tool Detection

### JavaScript/TypeScript Build Tools

| Tool      | Indicator Files                                     | Config Files     |
| --------- | --------------------------------------------------- | ---------------- |
| Webpack   | `webpack.config.js`, `webpack.config.ts`            | `.webpackrc.js`  |
| Vite      | `vite.config.js`, `vite.config.ts`                  | `vite.config.ts` |
| Turbopack | `turbo/` directory, `next.config.js` with turbopack | `turbo.json`     |
| esbuild   | `esbuild.config.js`, `esbuild` in scripts           | `esbuild.js`     |
| Rollup    | `rollup.config.js`, `rollup.config.ts`              | `.rolluprc.js`   |
| Parcel    | No config required (zero-config)                    | `.parcelrc`      |
| SWC       | `.swcrc`                                            | `.swcrc`         |

### TypeScript Tooling

| Tool             | Indicator              |
| ---------------- | ---------------------- |
| `ts-loader`      | TypeScript via webpack |
| `ts-node`        | TypeScript execution   |
| `tsx`            | TypeScript execution   |
| `esbuild-loader` | TypeScript via esbuild |

### Monorepo Build Tools

| Tool            | Config File                      | Key Indicators                                  |
| --------------- | -------------------------------- | ----------------------------------------------- |
| Nx              | `nx.json`                        | `workspace:` dependencies, `affected:` commands |
| Turborepo       | `turbo.json`                     | `pipeline` configuration, cache configuration   |
| Lerna           | `lerna.json`                     | Independent versioning, publish config          |
| Yarn Workspaces | `package.json` with `workspaces` | `workspaces:` field, `yarn.lock`                |
| pnpm Workspaces | `pnpm-workspace.yaml`            | `packages:` field, `pnpm-lock.yaml`             |

## Test Framework Detection

### JavaScript/TypeScript Testing

| Framework       | Indicator Files                    | Patterns                               |
| --------------- | ---------------------------------- | -------------------------------------- |
| Jest            | `jest.config.js`, `jest.config.ts` | `*.test.js`, `*.spec.js`, `__tests__/` |
| Vitest          | `vitest.config.ts`                 | `*.test.ts`, `*.spec.ts`               |
| Mocha           | `mocha.opts`, `.mocharc.*`         | `*.test.js`, `test/` directory         |
| Jasmine         | `jasmine.json`                     | `*.spec.js`                            |
| Jasmine + Karma | `karma.conf.js`                    | `*.spec.js`                            |
| Playwright      | `playwright.config.ts`             | `*.spec.ts`, e2e tests                 |
| Cypress         | `cypress.config.ts`                | `cypress/` directory                   |
| Testing Library | `@testing-library/*` in deps       | `render()`, `screen()` in tests        |

### Python Testing

| Framework | Indicator Files                              | Patterns                         |
| --------- | -------------------------------------------- | -------------------------------- |
| pytest    | `pytest.ini`, `pyproject.toml` [tool.pytest] | `test_*.py`, `*_test.py`         |
| unittest  | No specific config                           | `test_*.py`, `unittest.TestCase` |
| nose2     | `nose2.cfg`                                  | `test_*.py`                      |
| Robot     | `*.robot` files                              | Robot Framework syntax           |

## Monorepo Detection

### Nx Monorepo

**Indicators:**

- `nx.json` in root
- `workspace:` protocol in dependencies
- `apps/` and `libs/` or `packages/` directories
- `project.json` files in package directories
- `affected:*` commands in package.json

**Confirmation:**

```bash
# Check for nx.json
cat nx.json

# Check for workspace dependencies
grep -r "workspace:" packages/*/package.json
```

### Turborepo

**Indicators:**

- `turbo.json` in root
- `package.json` with `workspaces`
- `tasks` configuration in turbo.json
- Cache configuration

**Confirmation:**

```bash
# Check for turbo.json
cat turbo.json

# Check for workspaces
grep -A 5 "workspaces" package.json
```

### Lerna

**Indicators:**

- `lerna.json` in root
- Independent or fixed versioning
- `lerna publish` commands

**Confirmation:**

```bash
# Check for lerna.json
cat lerna.json
```

### Yarn Workspaces

**Indicators:**

- `package.json` with `workspaces` field
- `yarn.lock`
- No additional monorepo tool config

**Confirmation:**

```bash
# Check for workspaces
grep -A 10 "workspaces" package.json
```

### pnpm Workspaces

**Indicators:**

- `pnpm-workspace.yaml`
- `pnpm-lock.yaml`
- `packages:` field in workspace file

**Confirmation:**

```bash
# Check for workspace file
cat pnpm-workspace.yaml
```

## Project Size Detection

### Small Project (< 50 files)

**Characteristics:**

- Flat directory structure or shallow nesting
- Single entry point
- No sub-packages or workspaces
- Total files < 50

**Detection:**

```bash
# Count source files (excluding node_modules, build dirs)
find . -type f -not -path "*/node_modules/*" -not -path "*/dist/*" -not -path "*/.git/*" | wc -l
```

### Medium Project (50-500 files)

**Characteristics:**

- Multiple subdirectories
- Clear separation of concerns (src/, tests/, config/)
- May have a few utility libraries
- Total files 50-500

**Detection:**

```bash
# Check directory depth
find . -type d -not -path "*/node_modules/*" -not -path "*/.git/*" | wc -l
```

### Large Project/Monorepo (500+ files)

**Characteristics:**

- Multiple packages or workspaces
- Complex directory structure
- Multiple services or applications
- Total files > 500
- Monorepo tool present

**Detection:**

```bash
# Count files across all packages
find . -type f -not -path "*/node_modules/*" -not -path "*/dist/*" -not -path "*/.git/*" -not -path "*/.next/*" | wc -l

# Check for multiple package.json files
find . -name "package.json" -not -path "*/node_modules/*" | wc -l
```

## Linter/Formatter Detection

### ESLint

**Config files (in order of precedence):**

- `eslint.config.js` (Flat config)
- `eslint.config.mjs` (Flat config)
- `.eslintrc.js`
- `.eslintrc.cjs`
- `.eslintrc.json`
- `.eslintrc.yaml`
- `.eslintrc.yml`
- `package.json` with `eslintConfig` field

**Detection:**

```bash
# Look for eslint config
ls -la | grep eslint
cat package.json | grep -A 20 "eslintConfig"
```

### Prettier

**Config files (in order of precedence):**

- `.prettierrc`
- `.prettierrc.json`
- `.prettierrc.yaml`
- `.prettierrc.yml`
- `.prettierrc.json5`
- `.prettierrc.js`
- `.prettierrc.cjs`
- `.prettierrc.toml`
- `package.json` with `prettier` field
- `.prettier.config.js` (deprecated)

**Detection:**

```bash
# Look for prettier config
ls -la | grep prettier
cat package.json | grep -A 10 "prettier"
```

### Python Tools

| Tool   | Config File                                             |
| ------ | ------------------------------------------------------- |
| Pylint | `.pylintrc`, `pyproject.toml` [tool.pylint]             |
| Black  | `pyproject.toml` [tool.black]                           |
| Ruff   | `ruff.toml`, `.ruff.toml`, `pyproject.toml` [tool.ruff] |
| isort  | `.isort.cfg`, `pyproject.toml` [tool.isort]             |
| mypy   | `mypy.ini`, `.mypy.ini`, `pyproject.toml` [tool.mypy]   |

### Go Tools

| Tool          | Config File                       |
| ------------- | --------------------------------- |
| golangci-lint | `.golangci.yml`, `.golangci.yaml` |

### Rust Tools

| Tool    | Config File    |
| ------- | -------------- |
| clippy  | `clippy.toml`  |
| rustfmt | `rustfmt.toml` |

## CI/CD Detection

| Platform        | Config Files              |
| --------------- | ------------------------- |
| GitHub Actions  | `.github/workflows/*.yml` |
| GitLab CI       | `.gitlab-ci.yml`          |
| CircleCI        | `.circleci/config.yml`    |
| Travis CI       | `.travis.yml`             |
| Azure Pipelines | `azure-pipelines.yml`     |
| Jenkins         | `Jenkinsfile`             |

**Detection:**

```bash
# Check for CI/CD configs
ls -la .github/workflows/
ls -la .gitlab-ci.yml
ls -la .circleci/
```

## Language Detection

**Primary language indicators:**

| Language   | File Extensions       | Config Files                 |
| ---------- | --------------------- | ---------------------------- |
| TypeScript | `.ts`, `.tsx`         | `tsconfig.json`              |
| JavaScript | `.js`, `.jsx`, `.mjs` | `package.json`               |
| Python     | `.py`                 | `pyproject.toml`, `setup.py` |
| Go         | `.go`                 | `go.mod`                     |
| Rust       | `.rs`                 | `Cargo.toml`                 |
| Ruby       | `.rb`                 | `Gemfile`                    |
| Java       | `.java`               | `pom.xml`, `build.gradle`    |
| Kotlin     | `.kt`, `.kts`         | `build.gradle.kts`           |

**Detection:**

```bash
# Count file types by extension
find . -type f -name "*.ts" -not -path "*/node_modules/*" | wc -l
find . -type f -name "*.py" | wc -l
find . -type f -name "*.go" | wc -l
```

## Framework Version Detection

**From lock files:**

- `yarn.lock`: Search for package name
- `package-lock.json`: Check `packages` > `[package-name]` > `version`
- `pnpm-lock.yaml`: Check `packages` > `[package-name]`

**From package.json:**

```bash
# Check for CI/CD configs
ls -la .github/workflows/
ls -la .gitlab-ci.yml
ls -la .circleci/
```

## Language Detection

**Primary language indicators:**

| Language   | File Extensions       | Config Files                 |
| ---------- | --------------------- | ---------------------------- |
| TypeScript | `.ts`, `.tsx`         | `tsconfig.json`              |
| JavaScript | `.js`, `.jsx`, `.mjs` | `package.json`               |
| Python     | `.py`                 | `pyproject.toml`, `setup.py` |
| Go         | `.go`                 | `go.mod`                     |
| Rust       | `.rs`                 | `Cargo.toml`                 |
| Ruby       | `.rb`                 | `Gemfile`                    |
| Java       | `.java`               | `pom.xml`, `build.gradle`    |
| Kotlin     | `.kt`, `.kts`         | `build.gradle.kts`           |

**Detection:**

```bash
# Count file types by extension
find . -type f -name "*.ts" -not -path "*/node_modules/*" | wc -l
find . -type f -name "*.py" | wc -l
find . -type f -name "*.go" | wc -l
```

## Framework Version Detection

**From lock files:**

- `yarn.lock`: Search for package name
- `package-lock.json`: Check `packages` > `[package-name]` > `version`
- `pnpm-lock.yaml`: Check `packages` > `[package-name]`

**From package.json:**

```bash
# Check installed versions
grep -A 2 "react" package.json
grep -A 2 "next" package.json
grep -A 2 "typescript" package.json
```

## Detection Workflow

### Step 1: Identify Language

1. Check for `tsconfig.json` → TypeScript
2. Look at most common file extensions
3. Check `package.json` or equivalent

### Step 2: Identify Package Manager

1. Look for lock files
2. Check package manager field in package.json
3. Examine CI/CD files for commands used

### Step 3: Identify Framework

1. Check dependencies for framework packages
2. Look for framework-specific config files
3. Examine directory structure

### Step 4: Identify Build Tools

1. Look for build config files
2. Check scripts in package.json
3. Examine CI/CD build steps

### Step 5: Identify Monorepo

1. Look for monorepo config (nx.json, turbo.json, lerna.json)
2. Check for workspaces configuration
3. Look for multiple package.json files

### Step 6: Identify Project Size

1. Count total files (excluding node_modules, dist, build, .git)
2. Count directory depth
3. Count number of packages (if monorepo)

### Step 7: Identify Tooling

1. Look for linter configs
2. Look for formatter configs
3. Check for CI/CD configuration

### Step 8: Analyze Git History (if applicable)

1. Confirm git repository exists
2. Check commit count and health
3. Extract or generate last update timestamp
4. Run git analysis commands for insights
5. Determine if git-based sections should be included

**Conditional inclusion:**

| Condition            | Include Section                      |
| -------------------- | ------------------------------------ |
| > 20 commits         | Recent Activity (small/medium)       |
| > 100 commits        | Historical Hotspots (medium/large)   |
| > 200 commits        | File Coupling (large only)           |
| Recent activity < 90d| Active Development (all sizes)       |
