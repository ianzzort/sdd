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

---

# Installation

The installation has four steps. Do them in this order.

| Step | What you do | Where it lands |
|---|---|---|
| 1 | Copy the architecture | `sdd_architecture/` |
| 2 | Point the agent at the brain | `CLAUDE.md` at the repo root |
| 3 | Register the skills | `.claude/skills/<name>/SKILL.md` |
| 4 | Fill the context | `sdd_architecture/context/` |

After the installation, the target repository looks like this:

```
<your-repo>/
  CLAUDE.md                   <- the agent file, at the ROOT (not inside sdd_architecture/)
  .claude/
    skills/
      add-feature/SKILL.md    <- thin wrapper
      layers-architect/SKILL.md
      security-and-quality/SKILL.md
      audit-project/SKILL.md
  sdd_architecture/
    Instruction.md
    context/
    skills/
```

## Step 1 — Copy the architecture to `sdd_architecture/`

The architecture goes in one folder at the root of the target repository. The
folder name is `sdd_architecture`. Copy three items into it: `Instruction.md`,
`context/`, and `skills/`.

The `CLAUDE.md` file is different. It goes at the **root** of the repository. Do
not put it inside `sdd_architecture/`.

Run these commands. Replace `<your-repo>` with the path of your repository.

```bash
git clone https://github.com/ianzzort/sdd.git /tmp/sdd

mkdir -p <your-repo>/sdd_architecture
cp -R /tmp/sdd/Instruction.md /tmp/sdd/context /tmp/sdd/skills <your-repo>/sdd_architecture/
```

Now check the result:

```bash
ls <your-repo>/sdd_architecture
# Instruction.md  context  skills
```

## Step 2 — Point the root `CLAUDE.md` at the brain

This step is the important one. The agent reads `CLAUDE.md` at the root of the
repository. That file must send the agent to `sdd_architecture/Instruction.md`
before any change. Without this step, the agent does not enter the SDD flow.

The section is short. It is only a door. The full detail (the context, the
skills, and the graph update) stays in the brain.

Copy this block into `CLAUDE.md`:

```markdown
## Arquitectura SDD

- En cada cambio de este proyecto, entrar primero a
  `sdd_architecture/Instruction.md` (el cerebro del SDD) y seguir su flujo. Es
  la puerta de entrada: el detalle completo (contexto, skills y actualización
  del grafo) vive ahí, no en este archivo.
```

There are two cases.

**Case A — the target repository has no `CLAUDE.md`.** Copy the `CLAUDE.md` of
this repository. It already carries the section above, plus the general rules:
working mode, code style, dates, commits, tickets, database, seeds, agent cost,
and secrets.

```bash
cp /tmp/sdd/CLAUDE.md <your-repo>/CLAUDE.md
```

Then open the file. Delete each rule that does not apply to your project. Fill
each `<...>` placeholder.

**Case B — the target repository already has its own `CLAUDE.md`.** Do not
overwrite that file. Append the section by hand instead:

```bash
cat >> <your-repo>/CLAUDE.md <<'EOF'

## Arquitectura SDD

- En cada cambio de este proyecto, entrar primero a
  `sdd_architecture/Instruction.md` (el cerebro del SDD) y seguir su flujo. Es
  la puerta de entrada: el detalle completo (contexto, skills y actualización
  del grafo) vive ahí, no en este archivo.
EOF
```

Write this section in the language of your team. The example is in Spanish,
because the `CLAUDE.md` of this repository is in Spanish. The meaning is what
counts: the agent enters `sdd_architecture/Instruction.md` first, on every
change.

## Step 3 — Register the skills so you can invoke them

The skills live in `sdd_architecture/skills/`. To invoke them with a slash
command, register a thin wrapper for each one in `.claude/skills/`. After this
step, you can call `/add-feature`, `/layers-architect`, `/security-and-quality`,
and `/audit-project`.

A wrapper holds no steps. It holds a frontmatter with `name` and `description`,
and a body that sends the agent to the real file. The file under
`sdd_architecture/` stays the source of truth. Edit the SDD file when you change
a skill. Never copy the steps into the wrapper.

This is the full content of `.claude/skills/add-feature/SKILL.md`:

```markdown
---
name: add-feature
description: "Wrapper. The real steps live in sdd_architecture/skills/requirements/add-feature/SKILL.md. Read that file and follow it."
---

Read `sdd_architecture/skills/requirements/add-feature/SKILL.md` and follow its steps.
That file is the source of truth. Do not duplicate its content here.
```

The other three wrappers use the same shape. Only the name and the path change:

| Slash command | Wrapper file | Real file |
|---|---|---|
| `/add-feature` | `.claude/skills/add-feature/SKILL.md` | `sdd_architecture/skills/requirements/add-feature/SKILL.md` |
| `/layers-architect` | `.claude/skills/layers-architect/SKILL.md` | `sdd_architecture/skills/requirements/layers-architect/SKILL.md` |
| `/security-and-quality` | `.claude/skills/security-and-quality/SKILL.md` | `sdd_architecture/skills/requirements/security-and-quality/SKILL.md` |
| `/audit-project` | `.claude/skills/audit-project/SKILL.md` | `sdd_architecture/skills/selectives/audit-project/SKILL.md` |

Do not write the four files by hand. Run this script from the root of your
repository. It creates all of them:

```bash
for entry in \
  "add-feature:requirements/add-feature" \
  "layers-architect:requirements/layers-architect" \
  "security-and-quality:requirements/security-and-quality" \
  "audit-project:selectives/audit-project"
do
  name="${entry%%:*}"
  dir="${entry#*:}"
  mkdir -p ".claude/skills/$name"
  cat > ".claude/skills/$name/SKILL.md" <<EOF
---
name: $name
description: "Wrapper. The real steps live in sdd_architecture/skills/$dir/SKILL.md. Read that file and follow it."
---

Read \`sdd_architecture/skills/$dir/SKILL.md\` and follow its steps.
That file is the source of truth. Do not duplicate its content here.
EOF
done
```

To add a skill later, add the folder under `sdd_architecture/skills/`, then add
one more line to the list in the script and run it again.

## Step 4 — Fill the context

The files in `sdd_architecture/context/` need your input.

1. `Documentation.md` is a template. Replace every `<...>` placeholder with the
   facts of your project: the stack, the layers, the domains, the database
   conventions, and the tooling commands. Delete a section only when your
   project does not have it. This file is the only place for project facts.
2. `Memory.md` is empty. Write the first entry. Use the entry format in that
   file. Delete the placeholder line at the end.
3. `Integration.md` is optional. Fill the table only when your project depends
   on another repository. Write `none` when it does not.

You can also let the agent do this step. Run `/add-feature` and ask it to fill
`Documentation.md` from the codebase.

---

## Verification checklist

Check each item before you start to work.

- [ ] The folder `sdd_architecture/` is at the root of your repository.
- [ ] It holds `Instruction.md`, `context/`, and `skills/`.
- [ ] `CLAUDE.md` is at the **root**, not inside `sdd_architecture/`.
- [ ] `CLAUDE.md` holds the section that sends the agent to
      `sdd_architecture/Instruction.md`. Verify it:
      `grep -n "sdd_architecture/Instruction.md" CLAUDE.md`
- [ ] `Documentation.md` has no `<...>` placeholder left. Verify it:
      `grep -n "<.*>" sdd_architecture/context/Documentation.md`
- [ ] `Memory.md` holds the first entry, and the placeholder line is gone.
- [ ] The four wrapper files exist. Verify it:
      `ls .claude/skills`
- [ ] The skills appear in the agent. Type `/` and look for `add-feature`,
      `layers-architect`, `security-and-quality`, and `audit-project`. Restart
      the agent session if a skill does not appear.

## Rules

- The brain and the skills stay neutral. They hold no business rule of one
  product.
- Every project fact lives in `context/Documentation.md`.
- Every change writes one entry in `context/Memory.md`.
- All the files use ASD-STE100 Simplified Technical English: short sentences,
  active voice, one instruction per sentence. `CLAUDE.md` is the exception: it
  is the project agent file, so it goes in the language of the team.
