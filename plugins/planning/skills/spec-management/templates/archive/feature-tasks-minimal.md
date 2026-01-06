# {feature-name} - Tasks

**Milestone**: {MVP|Beta Launch|Post-MVP}
**Infrastructure Phase**: {N}
**Required Infrastructure**: {I001, I010, I012}
**Estimated Days**: {N}

---

## Task Format (REQUIRED for Auto-Sync)

**Every task MUST follow this format for automatic completion tracking:**

```
- [ ] {Verb}: `{exact/relative/path.ext}` - {description}
```

**Examples:**
```markdown
- [ ] Create: `backend/services/auth_service.py` - authentication logic
- [ ] Update: `backend/api/routes/users.py` - add validation
- [ ] Test: `backend/tests/unit/test_auth.py` - unit tests for auth
```

**When a file is edited, matching tasks auto-mark `[x]`**

---

## Task Naming Convention (REQUIRED)

| Verb | Usage | Example |
|------|-------|---------|
| Create: | New files | `Create: \`backend/services/foo.py\`` |
| Update: | Modify existing | `Update: \`backend/api/routes/bar.py\`` |
| Add: | Append to file | `Add: \`frontend/hooks/useFoo.ts\`` |
| Implement: | Add functions | `Implement: \`backend/services/foo.py\`` |
| Configure: | Config changes | `Configure: \`backend/config/settings.py\`` |
| Integrate: | Connect systems | `Integrate: \`backend/api/routes/webhooks.py\`` |
| Test: | Test files | `Test: \`backend/tests/unit/test_foo.py\`` |
| Validate: | Verify correct | `Validate: \`database/migrations/001.sql\`` |

---

## L0: Prerequisites

- [ ] Verify infrastructure dependencies are complete:
  - [ ] {I001} - {name} (phase {N})
  - [ ] {I010} - {name} (phase {N})
- [ ] Verify feature dependencies are complete:
  - [ ] {F001} - {name}

---

## L1: Tests (REQUIRED - Write First, TDD)

> **CRITICAL**: Tests MUST be written BEFORE implementation. They should FAIL initially.

### Contract Tests
- [ ] Create: `backend/tests/contract/test_{name}.py` - contract tests for {endpoint}
- [ ] Create: `backend/tests/contract/test_{name-2}.py` - contract tests for {endpoint-2}

### Integration Tests
- [ ] Create: `backend/tests/integration/test_{name}.py` - integration tests for {user-journey}
- [ ] Create: `backend/tests/e2e/test_{name}.py` - E2E tests for {flow}

### Unit Tests
- [ ] Create: `backend/tests/unit/test_{service}.py` - unit tests for {service}
- [ ] Create: `backend/tests/unit/test_{validation}.py` - validation tests

---

## L2: Database

- [ ] Create: `database/migrations/{timestamp}_{name}.sql` - migration file
  - [ ] Define {table-1} table with columns and constraints
  - [ ] Define {table-2} table with columns and constraints
  - [ ] Add indexes for {frequently-queried-columns}
- [ ] Update: `database/migrations/{timestamp}_{name}.sql` - add RLS policies
  - [ ] Policy for SELECT: users read own data
  - [ ] Policy for INSERT/UPDATE/DELETE: users modify own data
- [ ] Create: `database/seeds/{name}.sql` - seed data
- [ ] Validate: `database/migrations/{timestamp}_{name}.sql` - test migration locally

---

## L3: Backend

- [ ] Create: `backend/services/{name}.py` - business logic service
  - [ ] Implement {operation-1} business logic
  - [ ] Implement {operation-2} business logic
- [ ] Create: `backend/api/routes/{name}.py` - API routes
  - [ ] GET endpoint for {resource}
  - [ ] POST endpoint for {resource}
  - [ ] PUT/PATCH endpoint for {resource}
  - [ ] DELETE endpoint for {resource}
- [ ] Create: `backend/models/{name}.py` - Pydantic models
  - [ ] Request model with field validation
  - [ ] Response model with serialization
- [ ] Update: `backend/services/{name}.py` - add error handling
- [ ] Test: `backend/tests/unit/test_{name}.py` - validate unit tests pass

---

## L4: Frontend

- [ ] Create: `frontend/app/{route}/page.tsx` - page component
  - [ ] Implement page layout and structure
  - [ ] Add server/client component separation
- [ ] Create: `frontend/components/{name}/{Component}.tsx` - UI components
- [ ] Create: `frontend/hooks/use{Name}.ts` - data fetching hooks
- [ ] Create: `frontend/lib/api/{name}.ts` - API client functions
- [ ] Update: `frontend/app/{route}/page.tsx` - add loading states
- [ ] Update: `frontend/components/{name}/{Component}.tsx` - add error handling

---

## L5: Integration

- [ ] Integrate: `backend/api/routes/{name}.py` - connect with {F001, F002}
- [ ] Validate: `backend/tests/integration/test_{name}.py` - end-to-end flow
- [ ] Test: `backend/tests/e2e/test_{name}.py` - run Playwright tests

---

## L6: AI Observability (REQUIRED for AI features)

> **CRITICAL**: If this feature uses AI (LLM, embeddings, agents), this section is MANDATORY.
> Skip ONLY if the feature has NO AI components.

### Telemetry Setup
- [ ] Create: `backend/lib/telemetry/{name}.py` - OpenTelemetry spans
- [ ] Update: `backend/config/observability.py` - LangSmith/LangFuse tracing
- [ ] Add: `backend/middleware/ai_metrics.py` - latency and token tracking

### Evaluation Framework
- [ ] Create: `evals/datasets/{name}.json` - eval dataset
- [ ] Create: `evals/{name}/test_accuracy.py` - accuracy eval
- [ ] Create: `evals/{name}/test_quality.py` - quality eval
- [ ] Create: `evals/baselines/{name}.json` - baseline metrics
- [ ] Create: `scripts/run-evals.sh` - eval runner

### Monitoring & Alerts
- [ ] Update: `backend/config/sentry.py` - Sentry AI tracing
- [ ] Create: `infra/alerts/ai-monitoring.yaml` - error rate alerts

### Guardrails
- [ ] Create: `backend/lib/guardrails/input.py` - input validation
- [ ] Create: `backend/lib/guardrails/output.py` - output validation
- [ ] Add: `backend/middleware/rate_limit.py` - rate limiting

---

## L7: Production Ready

- [ ] Validate: `backend/tests/` - performance (<500ms response)
- [ ] Validate: `backend/api/` - security review (auth, RLS, input validation)
- [ ] Test: `backend/tests/` - verify all tests pass
- [ ] Update: `docs/guides/{name}.md` - documentation
- [ ] Update: `roadmap/features.json` - status to "completed"

---

## Checkpoint Summary

| Layer | Purpose | Completion Criteria |
|-------|---------|---------------------|
| L0 | Prerequisites | All dependencies verified |
| L1 | Tests | All tests written and failing |
| L2 | Database | Schema migrated, RLS applied |
| L3 | Backend | API endpoints functional |
| L4 | Frontend | UI complete and connected |
| L5 | Integration | All layers working together |
| L6 | AI Observability | Telemetry, evals, monitoring (if AI) |
| L7 | Production | All tests pass, docs updated |
