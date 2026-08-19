---
name: security-and-quality
description: "Use for every change. Checks the security and the code quality of the change. Verifies input validation, authorization, tenant scoping, no secrets, safe data access, types, lint, and the project style rules. Returns PASS or a list of fixes."
---

# security-and-quality

This skill checks the security and the quality of a change. Use it for every
change, after you write the code and before you finish. The
[add-feature](../add-feature/SKILL.md) skill calls this skill in its loop.

## 1. Purpose

This skill finds security problems and quality problems in a change. It gives a
clear result: PASS, or FAIL with a list of fixes. It does not change the code by
itself. It reports. The caller fixes the problems and runs the check again.

## 2. Security checks

Check each item for the files that the change touches:

1. **Input validation.** Each endpoint validates its input with a schema. The
   schema rejects bad data at the trust boundary.
2. **Authorization.** An endpoint that reads or writes private data uses a
   protected procedure or a permission procedure. It does not use a public
   procedure for private data.
3. **Scope.** A query on a scoped model filters by the owner (the tenant, the
   organization, the branch, or the user). The change must not read or write
   the data of another owner.
4. **No secrets in code.** The code reads secrets from the environment. The code
   holds no hardcoded token, key, or password.
5. **Safe data access.** The data access uses the project client. The code
   builds no raw SQL from strings. A raw query uses parameters, never string
   concatenation.
6. **No sensitive data in the URL.** The change puts no personal or secret data
   in a URL, a query string, or a log line.
7. **Safe uploads.** A file upload checks the type and the size.
8. **Auth primitives.** The change does not weaken the password hash, the token
   signature, or the session rules.
9. **Server-only code stays on the server.** No secret and no server module
   reaches the client bundle.

## 3. Quality checks

1. **Types pass.** The typecheck command reports no error.
2. **Lint passes.** The lint command reports no error.
3. **Comments.** Follow the comment rule in `Documentation.md`, section
   Conventions.
4. **Reuse first.** The change reuses an existing helper, type, or component
   when one fits. It adds no speculative abstraction.
5. **Correct dates.** A date value follows the time zone rule in
   `Documentation.md`.
6. **Correct UI primitives.** The change uses the components that
   `Documentation.md` requires.
7. **Naming.** New files follow the naming rules in `Documentation.md`.
8. **A check for new logic.** New non-trivial logic (a branch, a money path, a
   parser) has one runnable test.

## 4. Domain checks

`Documentation.md` can declare extra rules for one domain (for example, a
calculation contract or an external format). When the change touches that
domain, check each rule of the domain. When the change touches no such domain,
mark this section `N/A`.

## 5. How to run the checks

1. Run the typecheck command from `Documentation.md`. Note each error.
2. Run the lint command from `Documentation.md`. Note each error.
3. Read the changed files. Check each item in sections 2, 3, and 4.

## 6. Output

Give a short report. Use this format:

```
Security:  PASS  |  FAIL
Quality:   PASS  |  FAIL
Domain:    PASS  |  FAIL  |  N/A

Verdict:   PASS  |  FAIL

Fixes (only if FAIL):
- <file>: <problem> -> <fix>
```

Return `PASS` only when every check passes. If the verdict is `FAIL`, the caller
must fix the problems and run this skill again.
