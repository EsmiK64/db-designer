# Quickstart: Database Designer App

**Branch**: `001-db-designer-app`  
**Date**: 2026-04-05

## Goal

Run the app locally with:
- **Backend**: Laravel API
- **Frontend**: React + TypeScript + shadcn/ui

## Repository structure (planned)

```text
backend/   # Laravel application
frontend/  # React + TypeScript application
specs/     # Feature docs
```

## Prerequisites

- PHP (version TBD by Laravel version)
- Composer
- Node.js (LTS)
- A local database for the backend (SQLite is acceptable for development)

## Backend (Laravel)

1. Install dependencies
2. Configure environment
3. Run migrations
4. Start the server

## Frontend (React + TS + shadcn/ui)

1. Install dependencies
2. Start dev server
3. Verify you can:
   - Create account / login
   - Create a design
   - Add tables/columns
   - Create relationships
   - Export

## Diagramming approach

- Diagram canvas uses an internal `diagram/` module.
- MVP uses React Flow as an engine but keeps our own UI components for nodes, edges, panels, and interactions.
