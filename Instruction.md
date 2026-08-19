# Instruction.md — SDD Architecture Brain

This file is the brain of the SDD architecture. SDD means Skill-Driven
Development. The SDD architecture keeps all the skills and the instructions in
one place. It makes each change follow the same steps every time.

This architecture is project-neutral. It holds no business rule of one product.
The project facts live in `context/Documentation.md`. Fill that file for each
project.

Read this file first. Then go to the correct folder. This file sends you to the
folder that you need.

All the files in this architecture use ASD-STE100 Simplified Technical English.
Write new content in the same style: short sentences, active voice, and one
instruction per sentence.

## 1. Folder map

```
sdd_architecture/
  Instruction.md              <- this file (the brain)
  context/
    Documentation.md          <- the full description of the project
    Memory.md                 <- the history of every change
    Integration.md            <- the guide to other repositories
  skills/
    requirements/             <- the mandatory skills (always used)
      layers-architect/       <- keeps the architecture pattern
      security-and-quality/   <- checks security and code quality
      add-feature/            <- makes a change and runs the loop
    selectives/               <- the optional skills (used on demand)
      audit-project/          <- audits the project, writes AUDIT_REPORT.md
```

## 2. The context folder

The context folder gives you the knowledge about the project. Read it before
you make a change.

- [Documentation.md](context/Documentation.md) — the full description of the
  project. It shows the stack, the layers, the domains, and the rules. Read it
  first to understand the project. This is the only file that holds project
  facts.
- [Memory.md](context/Memory.md) — the history of every change. Add a new entry
  after each change. Never delete an old entry.
- [Integration.md](context/Integration.md) — the guide to other repositories.
  Use it only when a change needs code or behavior from another repository.

## 3. The skills folder

A skill is a set of steps for one job. Each skill is a folder with a `SKILL.md`
file inside.

You can also register each SDD skill in `.claude/skills/`, so you can invoke it
directly (for example, `/add-feature`, `/layers-architect`,
`/security-and-quality`, `/audit-project`). The file in `.claude/skills/` is a
thin wrapper. The real steps live here in the SDD folder. Keep the SDD file as
the source of truth; the wrapper only points to it.

### 3.1 Mandatory skills (`skills/requirements/`)

Use these skills for every change.

- [layers-architect](skills/requirements/layers-architect/SKILL.md) — reads the
  project and keeps the same architecture pattern. It makes sure that each new
  file goes in the correct layer.
- [security-and-quality](skills/requirements/security-and-quality/SKILL.md) —
  checks the security and the quality of the code for each change.
- [add-feature](skills/requirements/add-feature/SKILL.md) — makes a change. It
  always calls `layers-architect` and `security-and-quality`. It runs a loop
  until the change is complete and correct.

### 3.2 Optional skills (`skills/selectives/`)

Use these skills only when you need them.

- [audit-project](skills/selectives/audit-project/SKILL.md) — audits the whole
  project. It checks the language, the architecture, the performance, and the
  security. It writes a report to `AUDIT_REPORT.md`.

### 3.3 Add a new skill

Add a mandatory skill only when every change needs it. In all other cases, add
the skill to `skills/selectives/`. Give the skill one job. Write the steps in
the same format as the other skills: frontmatter with `name` and `description`,
then the sections Purpose, Steps, Rules, and Output.

## 4. The knowledge graph (optional)

A knowledge graph turns the codebase into a map of files and connections. Use it
to answer questions about a large codebase.

This architecture does not require one tool. If the project uses a graph tool,
write the commands of that tool in `context/Documentation.md`, in the section
Testing and tooling. If the project uses no graph tool, skip every graph step in
the skills.

## 5. The operating procedure (the loop)

Follow these steps for every change.

### Before the change

1. Read [Documentation.md](context/Documentation.md) to understand the project.
2. If the change needs another repository, read
   [Integration.md](context/Integration.md).

### During the change

3. Use the [add-feature](skills/requirements/add-feature/SKILL.md) skill to make
   the change. This skill runs the loop:
   - It calls [layers-architect](skills/requirements/layers-architect/SKILL.md)
     to keep the architecture pattern.
   - It calls
     [security-and-quality](skills/requirements/security-and-quality/SKILL.md)
     to check the security and the quality.
   - If a check fails, it fixes the problem and runs the checks again.
   - It stops only when both checks pass.

### After the change

4. Update [Documentation.md](context/Documentation.md) if the architecture or a
   rule changed.
5. Add a new entry in [Memory.md](context/Memory.md). Do this for every change.
6. Update the knowledge graph, if the project uses one.

### Optional

- To audit the whole project, use the
  [audit-project](skills/selectives/audit-project/SKILL.md) skill. It writes
  `AUDIT_REPORT.md`.

## 6. Rules

- Use the mandatory skills for every change. Do not skip them.
- Keep the same architecture pattern. The `layers-architect` skill is the guide.
- Keep every project fact in `context/Documentation.md`. Do not write a business
  rule of one project in this file or in a skill.
- Write one entry in `Memory.md` for every change.
- Write all the files in this architecture in ASD-STE100 Simplified Technical
  English.
- Follow the project rules in `CLAUDE.md` (or the equivalent agent file) at the
  repository root. Those rules come first when there is a conflict.
