# sdd

SDD means **Skill-Driven Development**. It is a small, project-neutral
architecture for AI coding agents. It keeps the skills, the project knowledge,
and the change history in one folder, so every change follows the same steps.

## What it gives you

- One entry point: [Instruction.md](Instruction.md), the brain.
- One place for project facts: [context/Documentation.md](context/Documentation.md).
- One history of every change: [context/Memory.md](context/Memory.md).
- Three mandatory skills that run on every change, and a loop that stops only
  when the checks pass.

## Layout

```
CLAUDE.md                     <- template for the repo-root agent file
sdd_architecture/
  Instruction.md              <- the brain, read it first
  context/
    Documentation.md          <- the full description of the project (template)
    Memory.md                 <- the history of every change
    Integration.md            <- the guide to other repositories
  skills/
    requirements/             <- mandatory, used on every change
      layers-architect/       <- keeps the architecture pattern
      security-and-quality/   <- checks security and code quality
      add-feature/            <- makes a change and runs the loop
    selectives/               <- optional, used on demand
      audit-project/          <- audits the project, writes AUDIT_REPORT.md
```

## The loop

```
plan
  -> layers-architect (place each file in its layer)
    -> implement (the smallest change that works)
      -> security-and-quality (typecheck + lint + rules)
        -> FAIL: fix, then loop again (max 3 passes)
        -> PASS: update docs -> write memory -> update graph
```

## Install it in a project

```bash
git clone https://github.com/ianzzort/sdd.git /tmp/sdd
mkdir -p <your-repo>/sdd_architecture
cp -R /tmp/sdd/Instruction.md /tmp/sdd/context /tmp/sdd/skills <your-repo>/sdd_architecture/
cp /tmp/sdd/CLAUDE.md <your-repo>/CLAUDE.md
```

Then:

1. Fill every `<...>` placeholder in `sdd_architecture/context/Documentation.md`
   with the facts of your project.
2. Trim [CLAUDE.md](CLAUDE.md) to what applies. It already points the agent at
   `sdd_architecture/Instruction.md` and carries the general rules: working
   mode, code style, dates, commits, tickets, database, seeds, agent cost, and
   secrets.
3. Optional: register each skill in `.claude/skills/` as a thin wrapper that
   points to the file in `sdd_architecture/`. The SDD file stays the source of
   truth.

## Rules

- The brain and the skills stay neutral. They hold no business rule of one
  product.
- Every project fact lives in `context/Documentation.md`.
- Every change writes one entry in `context/Memory.md`.
- All the files use ASD-STE100 Simplified Technical English: short sentences,
  active voice, one instruction per sentence. `CLAUDE.md` is the exception: it
  is the project agent file, so it goes in the language of the team.
