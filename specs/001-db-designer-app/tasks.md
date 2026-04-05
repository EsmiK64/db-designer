---

description: "Tasks for Database Designer App"
---

# Tasks: Database Designer App

**Input**: Design documents from `/specs/001-db-designer-app/`
**Prerequisites**: plan.md (required), spec.md (required), research.md, data-model.md, contracts/

**Tests**: Include test tasks for any work that changes behavior. Bug fixes MUST add regression tests.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Web app**: `backend/` (Laravel), `frontend/` (React + TypeScript)

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create repository app directories `backend/` and `frontend/` per `specs/001-db-designer-app/plan.md`
- [ ] T002 Initialize Laravel project in `backend/` and verify `backend/artisan` runs
- [ ] T003 Initialize React + TypeScript project (React Starterkit) in `frontend/` and verify dev server starts
- [ ] T004 Configure shadcn/ui in `frontend/` and add baseline UI primitives in `frontend/src/components/ui/`
- [ ] T005 [P] Add shared formatting/linting configs for frontend in `frontend/.eslintrc.*` and `frontend/.prettierrc*`
- [ ] T006 [P] Add backend code style tooling baseline (Laravel Pint) config in `backend/pint.json`
- [ ] T007 Add root-level developer docs in `docs/dev.md` describing how to run frontend/backend locally

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T008 Setup backend database configuration and local dev defaults in `backend/.env.example`
- [ ] T009 Create initial backend migrations for core entities in `backend/database/migrations/*_create_designs_tables_columns_relationships_exports_tables.php`
- [ ] T010 Implement Eloquent models in `backend/app/Models/{Design,Table,Column,Relationship,Export}.php`
- [ ] T011 Implement ownership authorization policy in `backend/app/Policies/DesignPolicy.php` and register it in `backend/app/Providers/AuthServiceProvider.php`
- [ ] T012 Implement auth endpoints per `specs/001-db-designer-app/contracts/api.md` in `backend/routes/api.php` and `backend/app/Http/Controllers/Auth/*`
- [ ] T013 Implement API error format (code + message) via exception handling in `backend/app/Exceptions/Handler.php`
- [ ] T014 Implement request validation classes for designs/schema payloads in `backend/app/Http/Requests/*`
- [ ] T015 Create frontend API client wrapper in `frontend/src/services/apiClient.ts`
- [ ] T016 Create frontend auth state scaffolding in `frontend/src/services/auth.ts` and `frontend/src/hooks/useAuth.ts`
- [ ] T017 Add backend feature tests for auth endpoints in `backend/tests/Feature/Auth/*.php`

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Create and Save a Database Design (Priority: P1) 🎯 MVP

**Goal**: Users can sign up/sign in, create a design with tables/columns, save it, and reopen it

**Independent Test**: Create account → create design with 2 tables + columns → save → refresh → reopen → state restored

### Tests for User Story 1 (REQUIRED for behavior changes) ⚠️

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [ ] T018 [P] [US1] Backend feature tests for designs CRUD in `backend/tests/Feature/Designs/DesignCrudTest.php`
- [ ] T019 [P] [US1] Backend feature tests for schema persistence in `backend/tests/Feature/Designs/DesignSchemaPersistenceTest.php`

### Implementation for User Story 1

- [ ] T020 [US1] Implement designs CRUD endpoints in `backend/routes/api.php` and `backend/app/Http/Controllers/DesignController.php`
- [ ] T021 [US1] Implement schema save/load service in `backend/app/Services/DesignSchemaService.php`
- [ ] T022 [US1] Implement “list designs” and “open design” responses to include tables/columns/relationships in `backend/app/Http/Resources/DesignResource.php`
- [ ] T023 [P] [US1] Create frontend auth pages in `frontend/src/pages/LoginPage.tsx` and `frontend/src/pages/RegisterPage.tsx`
- [ ] T024 [P] [US1] Create frontend app routing shell in `frontend/src/App.tsx` and `frontend/src/pages/*`
- [ ] T025 [US1] Implement designs list UI in `frontend/src/pages/DesignsPage.tsx` calling `frontend/src/services/designs.ts`
- [ ] T026 [US1] Implement design workspace page scaffold in `frontend/src/pages/DesignWorkspacePage.tsx`
- [ ] T027 [US1] Implement internal diagram domain types in `frontend/src/diagram/types.ts`
- [ ] T028 [US1] Implement diagram canvas wrapper component in `frontend/src/diagram/DiagramCanvas.tsx`
- [ ] T029 [US1] Implement React Flow adapter layer in `frontend/src/diagram/adapters/reactflow/ReactFlowAdapter.tsx`
- [ ] T030 [P] [US1] Implement table node UI component (shadcn/ui) in `frontend/src/diagram/nodes/TableNode.tsx`
- [ ] T031 [US1] Implement create/edit table + column UI panels in `frontend/src/components/schema/TableEditorSheet.tsx`
- [ ] T032 [US1] Persist design state (tables/columns/positions) in frontend store in `frontend/src/services/designState.ts`
- [ ] T033 [US1] Implement save/open integration: map diagram state ↔ API payload in `frontend/src/services/designs.ts`

**Checkpoint**: User Story 1 is fully functional and testable independently

---

## Phase 4: User Story 2 - Manage Relationships Interactively (Priority: P2)

**Goal**: Users can create/edit/delete relationships visually and see validation feedback

**Independent Test**: In an existing design, add a FK relationship and see it persisted; attempt invalid relationship and see actionable validation

### Tests for User Story 2 (REQUIRED for behavior changes) ⚠️

- [ ] T034 [P] [US2] Backend feature tests for relationship CRUD and validation in `backend/tests/Feature/Designs/DesignRelationshipsTest.php`
- [ ] T035 [P] [US2] Backend feature tests for validate endpoint in `backend/tests/Feature/Designs/DesignValidateTest.php`

### Implementation for User Story 2

- [ ] T036 [US2] Implement validate endpoint `POST /api/designs/{designId}/validate` in `backend/routes/api.php` and `backend/app/Http/Controllers/DesignValidationController.php`
- [ ] T037 [US2] Implement validation rules service in `backend/app/Services/DesignValidationService.php`
- [ ] T038 [US2] Add relationship persistence support to schema service in `backend/app/Services/DesignSchemaService.php`
- [ ] T039 [US2] Implement relationship edge rendering in `frontend/src/diagram/edges/RelationshipEdge.tsx`
- [ ] T040 [US2] Implement relationship creation UX (connect handles) in `frontend/src/diagram/interactions/relationshipCreate.ts`
- [ ] T041 [US2] Implement relationship editor panel in `frontend/src/components/schema/RelationshipEditorSheet.tsx`
- [ ] T042 [US2] Implement frontend validation call + overlay UI in `frontend/src/services/validation.ts` and `frontend/src/components/validation/ValidationPanel.tsx`
- [ ] T043 [US2] Ensure invalid designs block export/save flows with clear messaging in `frontend/src/pages/DesignWorkspacePage.tsx`

**Checkpoint**: User Story 2 is fully functional and remains independently testable

---

## Phase 5: User Story 3 - Export to SQL and Migration Formats (Priority: P3)

**Goal**: Users can export valid designs to SQL and migration formats and download results

**Independent Test**: Export a valid design to SQL for a selected DB + export to Laravel migrations; outputs downloadable; invalid designs blocked

### Tests for User Story 3 (REQUIRED for behavior changes) ⚠️

- [ ] T044 [P] [US3] Backend feature tests for export creation/status/download in `backend/tests/Feature/Exports/ExportFlowTest.php`
- [ ] T045 [P] [US3] Backend tests for SQL generator invariants in `backend/tests/Unit/Exports/SqlExportTest.php`
- [ ] T046 [P] [US3] Backend tests for Laravel migration generator invariants in `backend/tests/Unit/Exports/LaravelMigrationExportTest.php`

### Implementation for User Story 3

- [ ] T047 [US3] Decide first JS migration export target and record in `specs/001-db-designer-app/research.md`
- [ ] T048 [US3] Decide first PHP migration export target beyond Laravel and record in `specs/001-db-designer-app/research.md`
- [ ] T049 [US3] Implement export endpoints in `backend/routes/api.php` and `backend/app/Http/Controllers/ExportController.php`
- [ ] T050 [US3] Implement export orchestration service in `backend/app/Services/ExportService.php`
- [ ] T051 [US3] Implement SQL DDL generator module in `backend/app/Exports/Sql/*` (dialect-aware)
- [ ] T052 [US3] Implement Laravel migrations generator module in `backend/app/Exports/LaravelMigrations/*`
- [ ] T053 [US3] Implement JS migration generator module skeleton in `backend/app/Exports/JavaScriptMigrations/*`
- [ ] T054 [US3] Implement PHP migration generator module skeleton in `backend/app/Exports/PhpMigrations/*`
- [ ] T055 [US3] Implement export artifact storage and download in `backend/app/Services/ExportArtifactStore.php`
- [ ] T056 [US3] Implement frontend export UI (modal/panel) in `frontend/src/components/export/ExportDialog.tsx`
- [ ] T057 [US3] Implement frontend export service calls in `frontend/src/services/exports.ts`
- [ ] T058 [US3] Implement download handling UX in `frontend/src/components/export/ExportDownloads.tsx`

**Checkpoint**: User Story 3 is fully functional and remains independently testable

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T059 [P] Add accessibility pass for diagram interactions (keyboard nav + focus) in `frontend/src/diagram/a11y/*`
- [ ] T060 Add performance measurement notes and quick profiling steps in `docs/performance.md`
- [ ] T061 [P] Add consistent error/toast handling UX in `frontend/src/components/feedback/toast.tsx`
- [ ] T062 Security hardening: ensure all design/export endpoints enforce ownership in `backend/app/Policies/*` and controllers
- [ ] T063 Run and update `specs/001-db-designer-app/quickstart.md` with exact commands used

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: Depend on Foundational completion
- **Polish (Final Phase)**: Depends on desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Starts after Phase 2
- **User Story 2 (P2)**: Starts after Phase 2; uses saved design state from US1
- **User Story 3 (P3)**: Starts after Phase 2; depends on validation from US2 for best UX

### Parallel Opportunities

- Setup configuration tasks can run in parallel (`T005`, `T006`).
- Frontend UI tasks within a user story can often run in parallel when in separate files (`T023`, `T024`, `T030`).
- Backend resource/controller/test work can be parallelized across endpoints (auth vs designs vs exports).

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1 (Setup)
2. Complete Phase 2 (Foundational)
3. Complete Phase 3 (US1)
4. Validate independent test for US1

### Incremental Delivery

- Add US2 next (relationships + validation)
- Add US3 last (exports)

## Notes

- Keep React Flow isolated behind `frontend/src/diagram/*` to preserve ownership and enable replacement.
- Ensure every behavior-affecting change includes tests per constitution.
- Track performance regressions during diagram interactions (pan/zoom/drag/connect).
