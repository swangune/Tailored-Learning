# Study Session Records

**Every study session must be recorded**, including theory, programming, simulation, review, catch-up and mastery-only sessions.

Use one Markdown file per session:

`YYYY-MM-DD[-module].md`

If more than one session for the same module occurs on the same date, add a short suffix such as `-2`, `-lab` or `-review`.

## Required session mode

Every record must declare exactly one primary mode:

- `theory` — concepts, derivations, hand calculations, source study;
- `programming` — coding, debugging, numerical implementation, automated tests;
- `simulation` — CAD/meshing/solver setup, engineering simulation software;
- `review` — retrieval, quiz, corrections, exam practice;
- `mixed` — use only when no single mode clearly dominates.

The record must also state whether programming/tool work was actually completed, not merely planned.

## Minimum record

```markdown
# Session

- Date:
- Scheduled module:
- Official syllabus item:
- Session mode:
- Foundation bridge:
- Mastery target:
- Programming/tool work completed:
- Next session mode:
- Next action:

## Diagnostic

## First-principles derivation / explanation

## Worked problem / implementation

## Errors and corrections

## Mastery gate

## Next action
```

## Session ledger

Every session must also append one row to `state/session_history.csv`.

The ledger is the authority for deciding whether the next session for a programming-heavy module should emphasize theory or programming. Do not infer this from weekday alone.

The next mode should be selected from evidence:

- if the active topic has theory/derivation but no implementation evidence, choose `programming`;
- if programming exposes a conceptual or mathematical gap, choose `theory` or `review` for repair;
- if both are secure but mastery is not passed, choose `review`/transfer;
- if mastery is passed and the next official topic begins, start the new topic in `theory` mode.
