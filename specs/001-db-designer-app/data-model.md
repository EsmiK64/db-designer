# Data Model: Database Designer App

**Feature**: specs/001-db-designer-app/spec.md  
**Date**: 2026-04-05

## Overview

The system stores user-owned **Designs** that contain a schema model (tables, columns, relationships) plus export configuration and history.

This document is domain-focused (not tied to a particular database engine implementation).

## Entities

### User

- **id**: unique identifier
- **email**: unique
- **password/credentials**: stored securely
- **created_at / updated_at**

**Relationships**:
- User **owns many** Designs

### Design

- **id**: unique identifier
- **owner_user_id**: FK to User
- **name**: human-friendly name
- **description**: optional
- **target_db**: one of {postgresql, mysql, mariadb, sqlite, mssql, oracle} (initial supported set)
- **schema_version**: integer for internal evolution of the saved format
- **canvas_state**: viewport/zoom/pan, optional
- **created_at / updated_at**

**Relationships**:
- Design **has many** Tables
- Design **has many** Relationships
- Design **has many** Exports

### Table

- **id**
- **design_id**
- **name**
- **position**: x/y (for diagram layout)
- **created_at / updated_at**

**Constraints / invariants**:
- Table name MUST be unique within a design.

**Relationships**:
- Table **has many** Columns

### Column

- **id**
- **table_id**
- **name**
- **type**: logical type (string/number/bool/datetime/json/etc.) plus optional parameters
- **nullable**: boolean
- **default**: optional expression/value
- **is_primary_key**: boolean
- **is_unique**: boolean
- **created_at / updated_at**

**Constraints / invariants**:
- Column name MUST be unique within its table.
- If `is_primary_key` is true, column MUST be non-nullable.

### Relationship

Represents a foreign-key-style relationship.

- **id**
- **design_id**
- **from_table_id**
- **from_column_id**
- **to_table_id**
- **to_column_id**
- **cardinality**: one-to-one | one-to-many | many-to-one | many-to-many
- **on_delete**: restrict | cascade | set_null | no_action (optional)
- **on_update**: restrict | cascade | set_null | no_action (optional)
- **created_at / updated_at**

**Constraints / invariants**:
- A relationship MUST reference existing tables/columns within the same design.

### Export

Represents an export request and resulting output.

- **id**
- **design_id**
- **requested_by_user_id**
- **target**: sql | laravel_migrations | js_migrations | php_migrations
- **target_db**: optional override for SQL exports
- **status**: pending | running | succeeded | failed
- **error_message**: optional
- **artifact_ref**: reference to generated download (path/id)
- **created_at / updated_at**

## Validation rules (derived from requirements)

- Duplicate table names MUST be rejected with an actionable error.
- Duplicate column names within a table MUST be rejected with an actionable error.
- Relationship creation MUST validate referenced key compatibility.
- Exports MUST be blocked when validation errors exist.
