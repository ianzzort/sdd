# Documentation.md — Project Description

This file describes the whole project. Read it first, before any change. It
shows the purpose, the stack, the layers, the domains, and the rules.

This file is the only place for project facts. The brain
([Instruction.md](../Instruction.md)) and the skills stay neutral.

This file uses ASD-STE100 Simplified Technical English. Keep it current. Update
it when the architecture or a rule changes.

> **This file is a template.** Replace each `<...>` placeholder with the facts
> of your project. Delete a section only when the project does not have it.

---

## 1. Purpose and domain

`<Write what the product does, in three to six sentences. Name the users. Name
the core problem that the product solves.>`

The main areas are:

- `<area 1>`
- `<area 2>`

---

## 2. Technology stack

- **Framework**: `<for example, Next.js 15, App Router, React 19>`
- **Language**: `<for example, TypeScript 5>`
- **API**: `<for example, tRPC v11 over TanStack React Query v5>`
- **Database**: `<for example, Prisma 6 on PostgreSQL 16>`
- **Validation**: `<for example, Zod>`
- **UI**: `<for example, Tailwind CSS 4 and shadcn/ui>`
- **Forms and state**: `<for example, react-hook-form and zustand>`
- **Auth**: `<for example, Auth.js v5 with the Google provider>`
- **Runtime**: `<for example, Node >= 20>`
- **Deployment**: `<for example, Docker, Vercel>`

---

## 3. Top-level layout

`<List each top-level folder and say what it holds. One line for each folder.>`

```
<repo>/
  src/            <- <what it holds>
  prisma/         <- <what it holds>
  docs/           <- <what it holds>
  scripts/        <- <what it holds>
```

---

## 4. Layered architecture and request flow

A request flows in this order:

```
UI page / component  ->  API entry  ->  router  ->  service  ->  data client
                     ->  database
```

1. **UI (page or component)** in `<path>` calls the typed API client.
2. **API entry**: `<path>` is the single API endpoint.
3. **Context**: `<path>` reads the session, verifies it, and loads the user and
   the access rights.
4. **Root router**: `<path>` merges the domain routers.
5. **Core**: `<path>` sets up the base procedures. The base procedures are:
   `<publicProcedure, protectedProcedure, adminProcedure, ...>`.
6. **Domain router** (`<path>`) defines the queries and the mutations. It
   validates the input with a schema. It delegates to a service.
7. **Service layer** (`<path>`) holds the business logic. It is the only layer
   that talks to the database client.
8. **Data**: `<path>` is the database client singleton. The environment is
   validated in `<path>`.

**The layer rule**: keep business logic out of the pages. Keep database calls
inside the services.

---

## 5. Domains and modules

| Domain | Router | Pages |
|---|---|---|
| `<domain>` | `<router path>` | `<pages path>` |

---

## 6. Database

`<Name the schema file. Say how many models the project has. List the model
groups and their prefix. Give the naming convention.>`

**Migration convention**: `<describe the folder name format, the tool command,
and the rules. For example: the tool generates every migration; never hand-edit
the SQL; an applied migration is immutable.>`

---

## 7. Components and UI conventions

`<Say where the shared UI lives. Name the layout shells. Give the rules for the
primitive components, the forms, and the tables.>`

---

## 8. Auth and permissions

- **Provider**: `<for example, Auth.js v5, Google OAuth>`
- **Session**: `<for example, a database session, or a JWT of 7 days>`
- **Route protection**: `<name the middleware or the guard>`
- **Roles and permissions**: `<name the model and the service>`
- **Admin surface**: `<name the path and how it is protected>`

---

## 9. Testing and tooling

- **Unit**: `<tool and command>`
- **E2E**: `<tool and command>`
- **Lint and format**: `<tool and command>`
- **Types**: `<command>`
- **Database scripts**: `<commands>`
- **Seeds**: `<where they live and how they run>`
- **Knowledge graph** (optional): `<the commands of the graph tool, or "none">`

---

## 10. Conventions

- **Working mode**: `<for example, use the simplest solution that works; reuse
  before you create; add no speculative abstraction.>`
- **Code style**: `<for example, write no code comments; filenames in English;
  kebab-case for modules and PascalCase for React components.>`
- **Dates and time zone**: `<the time zone and the helper to use>`
- **Commits**: `<for example, Conventional Commits, in English, no emoji, no AI
  attribution.>`
- **Database**: `<the rules>`
- **Secrets**: `<where the environment variables are declared and validated>`

---

## 11. Key files reference

- Core API: `<paths>`
- Auth: `<paths>`
- Database and config: `<paths>`
