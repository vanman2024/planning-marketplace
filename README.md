# Planning Marketplace

Planning tools for feature specifications, architecture design, ADRs, and project roadmaps.

## Features

- **Feature Specifications**: Create and manage feature specs with templates
- **Architecture Design**: System architecture, component diagrams, data flows
- **Decision Documentation**: Architecture Decision Records (ADRs)
- **Roadmap Planning**: Project roadmaps and milestone tracking
- **Multi-Agent Coordination**: Entity ownership tracking for parallel development

## Installation

```bash
# Via Claude Code plugin manager
claude plugins install vanman2024/planning-marketplace
```

## Available Commands

### Feature Management
- `/planning:add-feature` - Add a new feature with spec and ADR
- `/planning:update-feature` - Update an existing feature
- `/planning:spec` - Create feature specification

### Architecture
- `/planning:architecture` - Design system architecture
- `/planning:decide` - Document architectural decisions

### Project Planning
- `/planning:roadmap` - Create project roadmap
- `/planning:init-project` - Initialize all project specs

## Agents

- `feature-analyzer` - Analyzes projects and decomposes into features
- `feature-spec-writer` - Writes feature specifications
- `architecture-designer` - Designs system architecture
- `decision-documenter` - Documents architectural decisions
- `roadmap-planner` - Plans project roadmaps
- And 12+ more specialized agents

## Skills

- `spec-management` - Feature specification templates
- `architecture-patterns` - Architecture and diagram templates
- `decision-tracking` - ADR templates
- `build-manifest` - Build ordering and layer definitions
- `feature-workflow-generation` - Workflow automation
- `doc-sync` - Documentation synchronization

## License

MIT
