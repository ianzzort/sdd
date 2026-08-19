---
name: audit-project
description: "Use on demand to audit the whole project. Checks language and consistency, architecture, performance, security, and code quality. Writes the result to AUDIT_REPORT.md at the repository root. It reports only; it does not change code."
---

# audit-project

This skill audits the whole project. It is an optional (selective) skill. Use it
when the user asks for an audit, or before a release. It writes a report to
`AUDIT_REPORT.md`. It does not change the code.

## 1. Purpose

This skill finds problems across the whole project. It checks many dimensions.
It gives each finding a severity and a fix. It writes one report.

## 2. Before you start

1. Read [Documentation.md](../../../context/Documentation.md) to understand the
   project.
2. If the project uses a knowledge graph, update it and use it to find the
   structure and the connections. If the project uses no graph, read the folder
   tree and the router files instead.

## 3. Audit dimensions

Check each dimension. Record every finding.

### 3.1 Language and consistency

- The identifiers follow the naming rules in `Documentation.md`.
- The filenames follow the naming rules in `Documentation.md`.
- The comment rule of the project is respected.
- The UI strings follow the language rule of the project.

### 3.2 Architecture

- The pages hold no business logic.
- Only the service layer calls the database client.
- Each router validates its input with a schema.
- A complex form acts as an orchestrator, not one large file.
- Each domain follows the extra rules that `Documentation.md` declares for it.

### 3.3 Performance

- Find N+1 queries. A query inside a loop is a warning.
- Find a list query with no pagination or no limit.
- Find a missing database index on a common filter or join.
- Find a heavy client bundle or a large list with no virtualization.

### 3.4 Security

- A private endpoint uses a protected procedure or a permission procedure.
- A scoped query filters by the owner.
- The code holds no secret. Secrets come from the environment.
- The input validation covers each trust boundary.
- No personal or secret data goes in a URL or a log.

### 3.5 Code quality and over-engineering

- Find dead code and unused exports.
- Find duplicate logic that two or more files repeat.
- Find speculative abstractions: an interface with one use, a factory for one
  product, or config for a value that never changes.
- Find an unused dependency.

### 3.6 Domain correctness

Check the extra rules that `Documentation.md` declares for the domains of this
project. Skip this dimension when the project declares none.

## 4. Severity

Give each finding a severity:

- **High** — a security hole, data loss, a wrong calculation, or a broken layer
  boundary.
- **Medium** — a performance problem, a large duplication, or a broken
  convention that affects many files.
- **Low** — a small style problem or a single dead export.

## 5. Write the report

Write the report to `AUDIT_REPORT.md` at the repository root. Use this
structure:

```
# AUDIT_REPORT.md

## Summary
- Date: <date, project time zone>
- High findings: <count>
- Medium findings: <count>
- Low findings: <count>

## Findings by dimension

### Language and consistency
- [<severity>] <file>: <problem> -> <fix>

### Architecture
### Performance
### Security
### Code quality
### Domain correctness

## Recommended order of work
1. <the highest-value fix first>
2. ...
```

## 6. Rules

- Report only. Do not change the code in this skill.
- Give a file path and a concrete fix for each finding.
- Sort the findings by severity, high first.
- Do not invent a problem. If you are not sure, mark the finding as a question.
