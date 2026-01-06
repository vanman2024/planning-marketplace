# {feature-name} - Tasks

**Feature**: F{NNN}
**Milestone**: {MVP|Beta|Post-MVP}
**Infrastructure Phase**: {N}
**Required Infrastructure**: {I001, I010}
**Estimated Days**: {N}

---

## CRITICAL: Check Before Creating/Updating

**BEFORE creating or modifying ANY file, you MUST check if it exists:**

```bash
# Check if file exists
ls -la {path/to/file} 2>/dev/null && echo "EXISTS" || echo "NOT FOUND"

# Check if spec exists
find specs -type d -name "*F{NNN}*" 2>/dev/null

# Check if service exists
ls -la backend/services/{name}*.py 2>/dev/null

# Check if component exists
ls -la frontend/components/{name}/ 2>/dev/null

# Check if test exists
ls -la backend/tests/unit/*{name}*.py 2>/dev/null
```

**If file EXISTS:**
- Read it first to understand current state
- UPDATE existing file (don't create duplicate)
- Preserve existing functionality

**If file NOT FOUND:**
- Safe to CREATE new file

**NEVER overwrite without reading first.**

---

## Task Completion Format

**Mark tasks complete by changing `[ ]` to `[x]`:**
```
BEFORE: - [ ] Create: `path/file.py` - description
AFTER:  - [x] Create: `path/file.py` - description
```

---

## L0: Prerequisites

- [ ] Verify: Infrastructure dependencies complete
  - [ ] {I001} - {name}
  - [ ] {I010} - {name}
- [ ] Verify: Feature dependencies complete
  - [ ] {F001} - {name}
- [ ] Check: No duplicate spec exists for this feature

---

## L1: Tests (MANDATORY - Write First)

> **TDD**: Write tests FIRST. They should FAIL until L2-L4 are implemented.
> **CHECK**: Verify test file doesn't already exist before creating.

### Contract Tests
- [ ] Check: `ls backend/tests/contract/test_{name}.py` - exists?
- [ ] Create/Update: `backend/tests/contract/test_{name}.py` - API contract tests
  - [ ] Test: GET returns expected when valid input
  - [ ] Test: GET returns error when invalid input
  - [ ] Test: POST creates resource correctly
  - [ ] Test: POST validates input

### Integration Tests
- [ ] Check: `ls backend/tests/integration/test_{name}.py` - exists?
- [ ] Create/Update: `backend/tests/integration/test_{name}.py` - user journey tests
- [ ] Create/Update: `backend/tests/e2e/test_{name}.py` - end-to-end flow

### Unit Tests
- [ ] Check: `ls backend/tests/unit/services/test_{service}.py` - exists?
- [ ] Create/Update: `backend/tests/unit/services/test_{service}.py` - service tests
- [ ] Create/Update: `backend/tests/unit/test_{validation}.py` - validation tests

### Frontend Tests
- [ ] Check: `ls frontend/tests/unit/components/{Component}.test.tsx` - exists?
- [ ] Create/Update: `frontend/tests/unit/components/{Component}.test.tsx` - component tests
- [ ] Create/Update: `frontend/tests/e2e/{feature}.spec.ts` - Playwright tests

---

## L2: Database

- [ ] Check: `ls supabase/migrations/*{name}*.sql` - migration exists?
- [ ] Create/Update: `supabase/migrations/{timestamp}_{name}.sql` - migration
  - [ ] Define {table} with columns and constraints
  - [ ] Add indexes for {frequently-queried-columns}
- [ ] Add: RLS policies
  - [ ] SELECT: users read own data
  - [ ] INSERT/UPDATE/DELETE: users modify own data
- [ ] Validate: Run migration locally

---

## L3: Backend

- [ ] Check: `ls backend/services/{name}_service.py` - service exists?
- [ ] Create/Update: `backend/services/{name}_service.py` - business logic
  - [ ] Implement {operation-1}
  - [ ] Implement {operation-2}
  - [ ] Add error handling
- [ ] Check: `ls backend/api/routes/{name}.py` - routes exist?
- [ ] Create/Update: `backend/api/routes/{name}.py` - API routes
  - [ ] GET endpoint
  - [ ] POST endpoint
  - [ ] PUT/PATCH endpoint
  - [ ] DELETE endpoint
- [ ] Check: `ls backend/models/{name}.py` - models exist?
- [ ] Create/Update: `backend/models/{name}.py` - Pydantic models
  - [ ] Request model with validation
  - [ ] Response model
- [ ] Update: `backend/main.py` - register router (if new)

---

## L4: Frontend

- [ ] Check: `ls frontend/app/{route}/page.tsx` - page exists?
- [ ] Create/Update: `frontend/app/{route}/page.tsx` - page component
  - [ ] Layout and structure
  - [ ] Server/client component separation
- [ ] Check: `ls frontend/components/{name}/` - component dir exists?
- [ ] Create/Update: `frontend/components/{name}/{Component}.tsx` - UI components
- [ ] Check: `ls frontend/hooks/use{Name}.ts` - hook exists?
- [ ] Create/Update: `frontend/hooks/use{Name}.ts` - data fetching hook
- [ ] Create/Update: `frontend/lib/api/{name}.ts` - API client
- [ ] Add: Loading states
- [ ] Add: Error handling

---

## L5: Integration

- [ ] Integrate: Connect frontend to backend API
- [ ] Integrate: Connect with {F001, F002} dependencies
- [ ] Validate: `backend/tests/integration/` - all pass
- [ ] Validate: `frontend/tests/e2e/` - all pass

---

## L6: AI Observability (REQUIRED if AI feature)

- [ ] Check: `ls backend/lib/telemetry/{name}.py` - exists?
- [ ] Create/Update: `backend/lib/telemetry/{name}.py` - OpenTelemetry spans
- [ ] Add: LangSmith/LangFuse tracing
- [ ] Check: `ls evals/datasets/{name}.json` - exists?
- [ ] Create/Update: `evals/datasets/{name}.json` - eval dataset
- [ ] Create/Update: `evals/{name}/test_accuracy.py` - accuracy eval
- [ ] Add: Guardrails (input/output validation)
- [ ] Add: Rate limiting

---

## L7: Production Ready

- [ ] Validate: All tests pass (L1)
- [ ] Validate: Performance (<500ms response)
- [ ] Validate: Security review (auth, RLS, input validation)
- [ ] Check: `ls docs/guides/{name}.md` - docs exist?
- [ ] Create/Update: `docs/guides/{name}.md` - documentation
- [ ] Update: `roadmap/features.json` - status to "completed"

---

## Checkpoint Summary

| Layer | Purpose | Done When |
|-------|---------|-----------|
| L0 | Prerequisites | All dependencies verified |
| L1 | Tests | Tests written and failing |
| L2 | Database | Schema migrated, RLS applied |
| L3 | Backend | API endpoints functional |
| L4 | Frontend | UI complete and connected |
| L5 | Integration | All layers working together |
| L6 | AI Observability | Telemetry + evals (if AI) |
| L7 | Production | All tests pass, docs updated |
