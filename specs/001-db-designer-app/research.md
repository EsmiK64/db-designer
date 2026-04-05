# Research: Database Designer App

**Feature**: specs/001-db-designer-app/spec.md  
**Date**: 2026-04-05

## Decision 1: Diagramming engine (React + TypeScript)

**Decision**: Use **React Flow** (`@xyflow/react`, MIT license) as the diagram interaction engine for the MVP, wrapped behind an internal `Diagram` module so we keep product ownership of the UI/UX and can swap implementations later.

**Rationale**:
- React Flow is explicitly **MIT-licensed** and widely used, with built-in interactions we need (dragging nodes, pan/zoom, edge creation, selection).
- Nodes are React components, which fits the **shadcn/ui** approach (composition, styling in our code).
- Building a robust editor (hit-testing, edge routing, selection box, pan/zoom inertia, keyboard support) from scratch is high-risk and likely to violate the constitution’s performance and UX consistency principles.

**How we “own the code” anyway**:
- We will not leak React Flow primitives throughout the app.
- We define an internal domain model (`DiagramNode`, `DiagramEdge`, `Viewport`, `Selection`) and write adapter code:
  - `packages/frontend/src/diagram/DiagramCanvas.tsx` (our public API)
  - `packages/frontend/src/diagram/adapters/reactflow/*` (React Flow specific)
- Styling and behavior (table node UI, relationship handles, keyboard shortcuts, validation overlays, snapping, and layout rules) remains entirely in-repo.

**Alternatives considered**:
- **Build a custom diagram engine with Motion**: Possible for a minimal MVP (SVG + transforms + drag), but high complexity for correct edge routing, selection, and performance.
- **D3 force graphs**: Great for visualization but not ideal as an ergonomic schema editor.
- **Commercial libs (GoJS/others)**: Powerful but incompatible with “own the code”/licensing goals.

## Decision 2: Animation/motion

**Decision**: Use **Motion** (formerly Framer Motion, `motion/react`) for micro-interactions and layout animation (selection transitions, node hover affordances, panel transitions).

**Rationale**:
- Motion integrates cleanly with React and supports layout animations and gesture-driven interactions.
- Motion can be layered onto our own components without coupling the diagram engine to animation primitives.

## Decision 3: Backend framework

**Decision**: Use **Laravel** for the backend API (authentication, design persistence, export jobs).

**Rationale**:
- Strong ecosystem for auth, validation, queues/jobs, and JSON APIs.

## Open questions (deferred to planning tasks)

- Choose the first **JS migration export target** (e.g., Knex, Prisma Migrate, TypeORM, Sequelize).  
- Choose the first **PHP migration export target beyond Laravel** (e.g., Doctrine Migrations).  
- Decide if exports are synchronous downloads or asynchronous “export jobs” for large designs.
