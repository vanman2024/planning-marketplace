---
id: F{NNN}
name: {feature-name}
phase: {MVP|Beta|Post-MVP}
priority: {P0|P1|P2}
status: planned
infrastructure_phase: {0-5}
infrastructure_dependencies: [{I001}, {I010}]
feature_dependencies: [{F001}, {F002}]
has_ai_components: {true|false}
estimated_days: {N}
created: {YYYY-MM-DD}
---

# {feature-name}

## Overview
{Brief 2-3 sentence description of what this feature does}

## User Stories

### US1: {Title} (P1)
**As a** {user-type}
**I want** {capability}
**So that** {benefit}

**Acceptance Criteria:**
- [ ] {criterion-1}
- [ ] {criterion-2}
- [ ] {criterion-3}

### US2: {Title} (P2)
**As a** {user-type}
**I want** {capability}
**So that** {benefit}

**Acceptance Criteria:**
- [ ] {criterion-1}
- [ ] {criterion-2}

## Infrastructure Dependencies

| ID | Name | Phase | Status |
|----|------|-------|--------|
| {I001} | {name} | {N} | {required|optional} |
| {I010} | {name} | {N} | {required|optional} |

**Blocked until**: Infrastructure phase {N} complete

## Feature Dependencies
- **Requires**: {F001, F002} - must be built first
- **Blocks**: {F007, F008} - waiting on this feature

## Testing Requirements (MANDATORY)

### Contract Tests
- **CT-001**: {endpoint} returns {expected} when {valid-input}
- **CT-002**: {endpoint} returns {error} when {invalid-input}

### Integration Tests
- **IT-001**: {user-journey-1} end-to-end flow
- **IT-002**: {user-journey-2} end-to-end flow

### Unit Tests
- **UT-001**: {service/function} behaves correctly when {condition}
- **UT-002**: {validation} handles {edge-cases}

## AI Observability (REQUIRED if has_ai_components: true)

### Telemetry
- All AI calls traced (latency, tokens, model)
- Responses logged with PII redaction

### Evaluation
- Accuracy target: {percentage}
- Quality metrics: {coherence, relevance}

### Guardrails
- Input validation: {rules}
- Output validation: {rules}
- Rate limits: {limits}

## Scope

**Included:**
- {what-is-included-1}
- {what-is-included-2}

**Out of Scope:**
- {what-is-NOT-included}

## References
- Architecture: `docs/architecture/{section}.md`
- ADR: `docs/adr/{number}-{decision}.md`
