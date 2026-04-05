# API Contracts (Draft)

**Feature**: specs/001-db-designer-app/spec.md  
**Date**: 2026-04-05

## Conventions

- All endpoints use JSON.
- Authenticated endpoints require a session/token.
- Errors return a machine-parseable code and a human message.

## Auth

### POST /api/auth/register

**Request**
- email
- password

**Response**
- user

### POST /api/auth/login

**Request**
- email
- password

**Response**
- user
- auth context

### POST /api/auth/logout

**Response**
- ok

## Designs

### GET /api/designs

Lists designs owned by the current user.

### POST /api/designs

Creates a new design.

### GET /api/designs/{designId}

Returns the design with tables/columns/relationships.

### PUT /api/designs/{designId}

Updates design metadata and/or schema.

### DELETE /api/designs/{designId}

Deletes a design.

## Validation

### POST /api/designs/{designId}/validate

Returns validation errors/warnings preventing export.

## Exports

### POST /api/designs/{designId}/exports

Creates an export request.

Request includes:
- target: sql | laravel_migrations | js_migrations | php_migrations
- targetDb (optional)

### GET /api/exports/{exportId}

Returns status and (when ready) download information.

### GET /api/exports/{exportId}/download

Downloads the generated artifact.
