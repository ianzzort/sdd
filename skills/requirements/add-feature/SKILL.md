---
name: add-feature
description: "Use to make any change or new feature. Reads the context, plans the change, calls layers-architect to place the files, implements the smallest change that works, then calls security-and-quality. Runs a loop: it fixes and rechecks until both checks pass. Updates memory and the graph at the end."
---

# add-feature

This skill makes a change. It is the main entry point for work. It runs a loop.
The loop always calls [layers-architect](../layers-architect/SKILL.md) and
[security-and-quality](../security-and-quality/SKILL.md). The loop stops only
when the change is complete and correct.

## 1. Purpose

This skill turns a request into a correct, safe change. It keeps the same
architecture pattern. It checks the security and the quality. It records the
change.

## 2. Before you start

1. Read the request. Understand the goal.
2. Read [Documentation.md](../../../context/Documentation.md). Find the domain
   and the layers.
3. If the change needs another repository, read
   [Integration.md](../../../context/Integration.md).
4. Write a short plan. Use three to five bullets. Show the plan before the code.

## 3. The loop

Repeat these steps until the exit condition is true.

1. **Place the files.** Run the
   [layers-architect](../layers-architect/SKILL.md) skill. Get the plan of files
   and layers. Make sure each part is in the correct layer.
2. **Make the change.** Write the smallest change that works. Reuse an existing
   helper, type, or component first. Add no speculative abstraction.
3. **Check the change.** Run the
   [security-and-quality](../security-and-quality/SKILL.md) skill. This skill
   runs the typecheck, runs the lint, and checks the rules.
4. **Read the verdict.**
   - If the verdict is `PASS`, go to section 4 (After the change).
   - If the verdict is `FAIL`, fix each problem in the list. Then start the loop
     again at step 1.

### Loop limit

Count the loop passes. After three failed passes, stop the loop. Report the
remaining problems to the user. Ask for help. Do not loop without end.

## 4. After the change

Do these steps only after the verdict is `PASS`.

1. **Update the documentation.** Update
   [Documentation.md](../../../context/Documentation.md) if the architecture, a
   layer, a domain, or a rule changed. Skip this step if nothing structural
   changed.
2. **Write the memory entry.** Add a new entry at the top of
   [Memory.md](../../../context/Memory.md). Do this for every change. Use the
   entry format in that file.
3. **Update the graph.** If the project uses a knowledge graph, run the update
   command from `Documentation.md`. Skip this step if the project uses no graph.

## 5. Rules

- Always call `layers-architect` and `security-and-quality`. Do not skip them.
- Do not finish while a check fails.
- Keep the change small. The smallest correct change wins.
- Follow every rule in `CLAUDE.md` (or the equivalent agent file) at the
  repository root.

## 6. Summary flow

```
plan
  -> layers-architect (place files)
    -> implement (smallest change)
      -> security-and-quality (typecheck + lint + rules)
        -> FAIL: fix, then loop again (max 3 passes)
        -> PASS: update docs -> write memory -> update graph
```
