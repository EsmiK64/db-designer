# Feature Specification: Database Designer App

**Feature Branch**: `001-db-designer-app`  
 **Created**: 2026-04-05  
 **Status**: Draft  
 **Input**: User description: "Build a databbase designer app. It has to support all major databases, have user accounts with db-saving, table creating, interactive relationship management, and export to SQL code or Laravel migrations, and other major migration managers in JS or PHP."

## User Scenarios & Testing _(mandatory)_

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.

  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - Create and Save a Database Design (Priority: P1)

As a user, I can create a new database design by adding tables and columns, defining keys, and saving it to my account so I can return to it later.

**Why this priority**: This is the minimum viable value of a database designer: capturing a schema and not losing it.

**Independent Test**: A new user can sign up, create a design with 2 tables, save it, refresh/reopen, and see the same design restored.

**Acceptance Scenarios**:

1.  **Given** I am not signed in, **When** I create an account and sign in, **Then** I land in an empty workspace where I can start a new design.
2.  **Given** I have an empty design, **When** I add a table and define at least one column, **Then** the table is shown in the canvas/list with the column details persisted.
3.  **Given** I created a design, **When** I save it, **Then** it is stored under my account and appears in my list of saved designs.
4.  **Given** I previously saved a design, **When** I reopen it, **Then** all tables, columns, and constraints are restored accurately.

---

### User Story 2 - Manage Relationships Interactively (Priority: P2)

As a user, I can create, edit, and delete relationships between tables visually, and the app validates the design for common schema errors.

**Why this priority**: Relationship management is the core “designer” experience and is required for correct exports.

**Independent Test**: In a saved design, the user can create a one-to-many relationship and see validation messages for an invalid relationship.

**Acceptance Scenarios**:

1.  **Given** I have two tables with compatible key columns, **When** I create a relationship between them, **Then** the relationship is displayed and stored in the design.
2.  **Given** a relationship exists, **When** I edit its cardinality or referenced columns, **Then** the diagram and saved design update accordingly.
3.  **Given** I attempt to create an invalid relationship (e.g., missing referenced key), **When** I try to save/export, **Then** I see a clear validation error explaining what must be fixed.
4.  **Given** I delete a relationship, **When** I save the design, **Then** the relationship is removed and not present when reopening.

---

### User Story 3 - Export to SQL and Migration Formats (Priority: P3)

As a user, I can export my design into SQL (for the chosen database dialect) and into common migration tool formats for PHP and JS so I can implement the schema.

**Why this priority**: Exports turn a design into usable output and are a key differentiator.

**Independent Test**: A user can export a valid design to SQL for a chosen database and to Laravel migrations, and the exported output is downloadable.

**Acceptance Scenarios**:

1.  **Given** my design is valid, **When** I choose an export target "SQL" and select a database type, **Then** I receive SQL DDL output compatible with that database type.
2.  **Given** my design is valid, **When** I export to Laravel migrations, **Then** I receive migration files/code that represents the same schema.
3.  **Given** my design is valid, **When** I export to a JS migration manager target, **Then** I receive migration output in that target’s expected structure.
4.  **Given** my design has validation errors, **When** I export, **Then** export is blocked and I am shown the errors to resolve.

No additional user stories are required for v1 beyond the three above.

### Edge Cases

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right edge cases.
-->

- What happens when a user attempts to use duplicate table names or duplicate column names within a table?
- What happens when a user changes a primary key column that is referenced by relationships?
- How does the system handle circular relationships or self-referential relationships?
- How does the system handle reserved keywords for a selected database type?
- What happens when a user loses connectivity during save/export?
- What happens when an export target cannot represent a feature (e.g., partial index, vendor-specific type)?

## Requirements _(mandatory)_

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right functional requirements.
-->

### Functional Requirements

- **FR-001**: The system MUST allow users to create an account, sign in, and sign out.
- **FR-002**: The system MUST allow an authenticated user to create a new database design.
- **FR-003**: The system MUST allow a user to add, edit, and delete tables within a design.
- **FR-004**: The system MUST allow a user to add, edit, and delete columns within a table.
- **FR-005**: The system MUST support defining at minimum: primary keys, foreign keys, uniqueness constraints, and nullable/non-nullable columns.
- **FR-006**: The system MUST allow users to create, edit, and delete relationships between tables and persist them in the design.
- **FR-007**: The system MUST validate designs and present actionable errors for schema issues that would prevent a correct export.
- **FR-008**: The system MUST allow a user to save a design to their account and reopen it later.
- **FR-009**: The system MUST support multiple saved designs per user.
- **FR-010**: The system MUST allow exporting a valid design to SQL DDL for at least these database types: PostgreSQL, MySQL/MariaDB, SQLite, Microsoft SQL Server, and Oracle.
- **FR-011**: The system MUST allow exporting a valid design to Laravel migrations.
- **FR-012**: The system MUST provide at least one export target for a JavaScript migration manager.
- **FR-013**: The system MUST provide downloadable export outputs (single file and/or archive) from the UI.
- **FR-014**: The system MUST ensure a user can only access designs owned by that user.

### Key Entities _(include if feature involves data)_

- **User**: Account identity; owns saved designs.
- **Design**: A saved database design; metadata (name, timestamps) and schema content.
- **Table**: Part of a design; has a name and a set of columns.
- **Column**: Part of a table; has name, type, nullability, default, and constraint flags.
- **Relationship**: Connection between two tables/columns with cardinality and FK semantics.
- **ExportJob/Export**: An export request and its generated outputs (target type, timestamps, artifacts).

## Success Criteria _(mandatory)_

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: A new user can create an account and save a design with 2 tables and 1 relationship within 5 minutes.
- **SC-002**: Reopening a saved design restores schema content with no data loss (0 missing/extra tables, columns, or relationships).
- **SC-003**: Export to SQL succeeds for each supported database type for a valid design and is blocked for invalid designs with clear errors.
- **SC-004**: Export outputs represent the same schema semantics as the design (tables, columns, PK/FK, uniqueness, nullability).
- **SC-005**: Interactive editing actions (add table/column, create relationship) provide user feedback within 250ms for typical designs (e.g., up to 50 tables).

## Assumptions

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right assumptions based on reasonable defaults
  chosen when the feature description did not specify certain details.
-->

- Users access the app via a modern web browser.
- Offline-first behavior is out of scope for v1; temporary connectivity loss may delay saves/exports but must not corrupt saved designs.
- The initial release focuses on schema design and exports; reverse-engineering an existing database is out of scope.
- The initial release includes at least one JavaScript migration export format and at least one PHP migration export format beyond Laravel; exact targets can be selected during planning.
