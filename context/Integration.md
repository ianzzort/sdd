# Integration.md — Guide to Other Repositories

This file tells you how to review another repository. Use it only when a change
needs code or behavior from a repository that is not this one.

Most changes do not need another repository. Read
[Documentation.md](Documentation.md) first. Come here only when the change needs
an external source.

## 1. When to use this guide

Use this guide when:

- You port a feature from another repository into this one.
- You compare an implementation in this repository with the same feature in
  another one.
- You check that this repository keeps the same capability as another one
  (parity).

## 2. The other repositories

`<List each related repository and say what it holds. Write "none" when the
project has no related repository.>`

| Repository | Purpose | When to read it |
|---|---|---|
| `<name or URL>` | `<what it holds>` | `<the case that needs it>` |

## 3. The method (parity audit)

1. Compare the two source trees with `diff -rq` first. Use the shell before you
   use an agent. The line count gives most of the answer for a low cost.
2. Sort each file into a class: OK, RECONCILE, PORT, REVIEW, DROP, or DEFER.
3. Write the result to a ledger file, for example
   `docs/<other-repo>-parity-ledger.md`.
4. Classify by capability, not by file name. Two files with different names can
   do the same job.
5. A domain is not done until every file in its ledger is closed.

## 4. Rules when you port code

- Regenerate each database migration with the project tool. Do not copy the SQL
  or the migration name from the other repository.
- Translate the identifiers to the language of this project. Follow the naming
  rules in [Documentation.md](Documentation.md).
- Keep an identifier in its original language only when an external contract
  requires it. Write the reason in `Documentation.md`.
- Follow every rule in `CLAUDE.md` (or the equivalent agent file) at the
  repository root.

## 5. Read another repository with a knowledge graph (optional)

If the project uses a graph tool, you can clone and graph another repository,
then merge the two graphs and ask one question across both. Write the exact
commands of your tool in [Documentation.md](Documentation.md), section Testing
and tooling.
