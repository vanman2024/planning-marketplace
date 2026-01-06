---
id: I{NNN}
name: {infrastructure-name}
phase: {0-5}
priority: {P0|P1|P2}
status: planned
category: {auth|cache|database|ai|monitoring|security}
blocks_features: [{F001}, {F010}]
created: {YYYY-MM-DD}
---

# {infrastructure-name}

## CRITICAL: Check Before Creating/Updating

**BEFORE creating this infrastructure spec:**
```bash
# Check if infrastructure spec already exists
find specs/infrastructure -type d -name "*I{NNN}*" 2>/dev/null
find specs/infrastructure -type d -name "*{name}*" 2>/dev/null

# Check features.json for existing entry
grep -E "I{NNN}" roadmap/features.json 2>/dev/null
```

**If EXISTS:** Update existing spec, don't create duplicate.
**If NOT FOUND:** Safe to create new spec.

---

## Overview
{Brief description of this infrastructure component}

## Purpose
- {Why this infrastructure is needed}
- {What features depend on it}

## Features Blocked
| Feature | Name | Waiting For |
|---------|------|-------------|
| {F001} | {name} | This infrastructure |
| {F010} | {name} | This infrastructure |

## Technical Requirements

### Components
- {component-1}: {description}
- {component-2}: {description}

### Configuration
- {config-1}: {value/description}
- {config-2}: {value/description}

### Environment Variables
```bash
{SERVICE}_URL=
{SERVICE}_API_KEY=your_key_here
{SERVICE}_SECRET=your_secret_here
```

## Implementation Tasks

- [ ] Check: `ls backend/services/{name}.py` - exists?
- [ ] Setup: {service/component}
- [ ] Configure: Environment variables
- [ ] Create/Update: `backend/services/{name}.py` - service wrapper
- [ ] Add: Health check endpoint
- [ ] Test: Integration works
- [ ] Check: `ls docs/infrastructure/{name}.md` - docs exist?
- [ ] Create/Update: `docs/infrastructure/{name}.md` - documentation

## Validation Criteria

- [ ] Service responds to health checks
- [ ] Authentication works
- [ ] Error handling in place
- [ ] Monitoring configured
- [ ] Documentation complete

## References
- Docs: {external-documentation-url}
- Architecture: `docs/architecture/{section}.md`
