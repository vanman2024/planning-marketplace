---
id: F{NNN}
name: {feature-name}
phase: {MVP|Beta Launch|Post-MVP}
infrastructure_phase: {0-5}
infrastructure_dependencies: [{I001}, {I010}]
priority: {P0|P1|P2}
status: planned
has_ai_components: {true|false}
---

# {feature-name}

## Overview
{brief-description}

## User Stories
**As a** {user-type}
**I want** {capability}
**So that** {benefit}

## Acceptance Criteria
- [ ] {criterion-1}
- [ ] {criterion-2}
- [ ] {criterion-3}

## Infrastructure Dependencies
**Required infrastructure (must be built first):**
- {I001} - {name} (phase {N})
- {I010} - {name} (phase {N})

**Infrastructure phase**: {N} - Feature cannot be built until phase {N} infrastructure exists

## Feature Dependencies
- **Requires**: {F001, F002} - features that must be built first
- **Blocks**: {F007, F008} - features waiting on this

## Testing Requirements (MANDATORY)

> **CRITICAL**: Every feature MUST define testing requirements. Tests are written BEFORE implementation (TDD).

### Contract Tests (API/Interface)
- **CT-001**: {endpoint} MUST return {expected-response} when {valid-input}
- **CT-002**: {endpoint} MUST return {error-response} when {invalid-input}

### Integration Tests (User Journeys)
- **IT-001**: Complete flow for {user-story-1}: {describe end-to-end scenario}
- **IT-002**: Complete flow for {user-story-2}: {describe end-to-end scenario}

### Unit Tests (Core Logic)
- **UT-001**: {service/function} MUST {expected-behavior} when {input-condition}
- **UT-002**: {validation-logic} MUST {expected-behavior} for {edge-cases}

## AI Observability Requirements (MANDATORY if AI feature)

> **CRITICAL**: If `has_ai_components: true`, this section is REQUIRED.
> Skip this section ONLY if the feature has NO AI components.

### Telemetry & Tracing
- **TEL-001**: All AI calls MUST be traced with latency, tokens, model, prompt hash
- **TEL-002**: AI responses MUST be logged for debugging (with PII redaction)
- **TEL-003**: Error rates MUST be tracked per AI operation type

### Evaluation Criteria
- **EVAL-001**: AI output accuracy: {how to measure - e.g., "90% correct classification"}
- **EVAL-002**: Response quality: {coherence, relevance, helpfulness metrics}
- **EVAL-003**: Baseline comparison: {what baseline to compare against}

### Monitoring & Alerts
- **MON-001**: Alert when AI error rate exceeds {threshold, e.g., 5%}
- **MON-002**: Alert when AI latency exceeds {threshold, e.g., 3 seconds}
- **MON-003**: Track AI costs and alert when {budget threshold} is exceeded

### Guardrails
- **GR-001**: Input validation: {what inputs to validate, e.g., prompt length}
- **GR-002**: Output validation: {what outputs to validate, e.g., no harmful content}
- **GR-003**: Rate limiting: {limits per user/endpoint}

## References
- **Architecture**: `docs/architecture/{section}.md#{anchor}`
- **ADR**: `docs/adr/{number}-{decision}.md`
- **Infrastructure Specs**: `specs/infrastructure/phase-{N}/{number}-{name}/`

## Scope
**Included:**
- {what-is-included-1}
- {what-is-included-2}

**Out of Scope:**
- {what-is-NOT-included}

## Technical Notes
{architecture-notes-specific-to-this-feature}
