---
name: express-mongodb-backend
description: Use for any Express route, controller, or Mongoose schema/query work — indexing, .lean(), populate/aggregation to avoid N+1 queries, and pagination. Route/controller structure and ESM rules that apply to all backend work already live in the always-on backend rule; this skill covers the Mongoose-specific detail.
---

# Express + MongoDB (Mongoose)

## Mongoose / queries
- Define indexes explicitly in the schema for any field used in a filter, sort, or lookup.
- Use `.lean()` for read-only queries that don't need Mongoose document methods.
- Avoid N+1 queries — use `.populate()` or aggregation instead of looping queries.
- Paginate any list endpoint that can return more than ~50 documents.

## File size / structure
- Split large controllers into smaller service functions once they approach the 120-line guidance (see the always-on backend rule).
