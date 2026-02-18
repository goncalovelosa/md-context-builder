# Architecture

This document describes the architecture of [project name].

## System Overview

[Brief paragraph describing the overall system architecture and major components]

## Architecture Diagram

[Text-based diagram or description of major components and their relationships]

```
┌─────────────────┐      ┌─────────────────┐
│                 │      │                 │
│   [Service 1]   │──────│   [Service 2]   │
│                 │      │                 │
└─────────────────┘      └─────────────────┘
         │                        │
         └────────────┬───────────┘
                      │
              ┌───────▼────────┐
              │                │
              │  [Shared Lib]  │
              │                │
              └────────────────┘
```

## Core Services

### [Service 1 Name]

- **Location:** `packages/[service-name]/` or `apps/[service-name]/`
- **Purpose:** [What it does]
- **Tech stack:** [Framework/language]
- **Key dependencies:** [Critical deps]
- **Documentation:** `.claude/docs/services/[service-name].md`

### [Service 2 Name]

- **Location:** `packages/[service-name]/` or `apps/[service-name]/`
- **Purpose:** [What it does]
- **Tech stack:** [Framework/language]
- **Key dependencies:** [Critical deps]
- **Documentation:** `.claude/docs/services/[service-name].md`

## Shared Libraries

### [Library 1]

- **Location:** `packages/[lib-name]/`
- **Purpose:** [What it provides]
- **Key exports:**
  - `[export]` - [purpose]
  - `[export]` - [purpose]
- **Consumers:** [Which services use it]

### [Library 2]

- **Location:** `packages/[lib-name]/`
- **Purpose:** [What it provides]
- **Key exports:**
  - `[export]` - [purpose]
  - `[export]` - [purpose]
- **Consumers:** [Which services use it]

## Data Flow

[Describe how data flows through the system]

1. [Step 1 - where data originates]
2. [Step 2 - processing/handling]
3. [Step 3 - storage/output]

## Integration Points

### External Systems

- **[System 1]:** [Description, API location]
- **[System 2]:** [Description, API location]

### Internal Communication

- **[Protocol/Method]:** [How services communicate]
- **[Protocol/Method]:** [How services communicate]

## Infrastructure

- **Hosting:** [Where it's deployed]
- **Database:** [Type and location]
- **Caching:** [If applicable]
- **Message Queue:** [If applicable]

## Deployment Architecture

[Describe how different services are deployed]

```
[Environment 1]
├── [Service 1] → [deployment info]
├── [Service 2] → [deployment info]
└── [Service 3] → [deployment info]

[Environment 2]
├── [Service 1] → [deployment info]
└── [Service 2] → [deployment info]
```

## Key Patterns

- **[Pattern 1]:** [Where and why it's used]
- **[Pattern 2]:** [Where and why it's used]
- **[Pattern 3]:** [Where and why it's used]

## Technology Decisions

| Technology | Why it was chosen | Where it's used |
| ---------- | ----------------- | --------------- |
| [Tech 1]   | [Reason]          | [Location]      |
| [Tech 2]   | [Reason]          | [Location]      |
