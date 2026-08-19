---
name: layers-architect
description: "Use for every change. Reads the project architecture and keeps the same layer pattern. Puts each new file in the correct layer (UI, API router, service, data). Prevents business logic in pages and database calls outside services."
---

# layers-architect

This skill keeps the architecture pattern. It makes sure that each change
follows the same layers as the rest of the project. Use it for every change,
before you write the code.

## 1. Purpose

The project uses a layered architecture. Each layer has one job. This skill puts
each part of a change in the correct layer. This keeps the codebase consistent.

## 2. The layers

Read [Documentation.md](../../../context/Documentation.md) for the exact paths of
this project. The layers are:

1. **UI layer** — the pages and the components.
   - The UI layer shows data and gets user input. It does not hold business
     rules. It calls the API layer with the typed client.

2. **API layer** — the routers and the endpoints.
   - The router validates the input with a schema. It checks the authorization.
     It calls a service. It does not hold long business logic.

3. **Service layer** — the business logic.
   - The service holds the business rules. It is the only layer that calls the
     database client for writes and complex reads.

4. **Data layer** — the database.
   - Only the data client touches the database. Generate every migration with
     the project tool.

The request flows in this order:

```
UI page / component  ->  API router  ->  service  ->  data client  ->  database
```

## 3. Steps

Do these steps for each change:

1. Read [Documentation.md](../../../context/Documentation.md) to find the layers
   and the domains of this project.
2. Find the domain of the change. Use the domain table in `Documentation.md`.
3. Split the change into parts. Give each part a layer.
4. Make a short plan. For each file, write the path and the layer. Show this
   plan before you write the code.
5. Put each part in the correct layer:
   - UI change: a page or a component in the UI layer.
   - New endpoint: a procedure in the domain router.
   - Business rule: a function in the service of that domain.
   - Database change: a schema change and a generated migration.
6. Reuse before you create. Look for a helper, a type, or a component that
   already exists. Move a part to a shared folder only when two or more flows
   use it.

## 4. Architecture rules

- Keep business logic out of the UI layer.
- Keep database calls inside the service layer.
- A complex form acts as an orchestrator. Extract the visual sections, the
  validations, and the utilities to their own files by domain. Do not put the
  whole flow in one component.
- Keep the special rules of one flow in the folder of that flow. Move code to a
  shared folder only when two or more flows use it.
- Follow the naming rules in `Documentation.md`, section Conventions.
- Follow the UI component rules in `Documentation.md`, section Components and UI
  conventions.

## 5. Domain rules

`Documentation.md` can declare extra rules for one domain. Read that section
before you touch the domain. Apply each rule of the domain in the plan.

## 6. Output

Give a short plan. The plan lists each file, its path, and its layer. The plan
shows that the change follows the pattern. Do not write the code before the plan
is correct.
