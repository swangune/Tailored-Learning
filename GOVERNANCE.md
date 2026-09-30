# Learning Governance

## Purpose

The governance system prevents three common failure modes:

1. **curriculum drift** — studying whatever is interesting rather than what the module requires;
2. **prerequisite drift** — spending indefinitely on background material and never returning to the syllabus;
3. **progress illusion** — moving on because material was explained, without demonstrating mastery.

## G1 — Syllabus authority

The official topic list in `SYLLABUS.md` may only change when the student supplies new official material or explicitly instructs a change.

## G2 — Timetable authority

The module for a study session is chosen by the governed Monday-Friday allocation in `TIMETABLE.md`, not by tutor preference. The university class timetable is tracked separately and must not be invented.

Current allocation: Monday/Thursday Machine Learning; Tuesday/Friday FEM & Structural Dynamics; Wednesday Engineering Simulation.

## G3 — Ordered progression

Within each module, study proceeds in official syllabus order unless:

- the university lecturer has demonstrably moved to a different topic; or
- the student explicitly requests a different order.

Any deviation is logged in `CHANGELOG.md`.

## G4 — Foundation bridge rule

A foundation bridge may be opened only when a prerequisite gap blocks the current syllabus item.

It must contain:

- the specific missing prerequisite;
- why it blocks the official topic;
- the smallest set of concepts needed;
- an exit test;
- a return target to the official syllabus.

Foundation bridges do not become new syllabus units.

## G5 — Mastery before progression

A topic is not complete because it was read, watched or explained. Completion requires the mastery evidence in `MASTERY.md`.

## G6 — One active syllabus topic per module

At most one official syllabus item per module may have status `active`. Background prerequisites may be `bridge-active`, but must reference the blocked official topic.

## G7 — Session scope budget

A normal session contains:

- one official syllabus item or one tightly scoped foundation bridge;
- at most one substantial derivation;
- one worked example;
- one implementation/engineering application where appropriate;
- one mastery check.

This rule prevents broad, shallow “tour” sessions.

## G8 — Evidence trail

Each session creates or updates a dated file in `sessions/` and the topic status in `state/progress.csv`.

## G9 — Assessment alignment

Teaching should eventually connect each topic to the relevant assessed skill: derivation, coding, simulation setup, interpretation, report writing or examination problem-solving. Assessment alignment must not replace first-principles understanding.

## G10 — No silent assumptions

When information is absent from the supplied syllabus—such as exact class days/times or an unseen module code—record it as unknown. Do not fill it from guesswork.

## G11 — Weekly allocation invariant

Under the student-authorized scheduling rule, a programming-heavy module receives two study days per Monday-Friday week; a non-programming module receives one. Classification must be supported by the supplied syllabus, not assumed from the module title.

With the current three modules, this produces exactly five governed study days. Permanent reallocations require explicit student authorization and a changelog entry.

## G12 — Weekly review

At the end of each academic week:

- compare actual progress with the official syllabus;
- list unresolved prerequisite gaps;
- identify any drift events;
- reconcile next week's plan with the university timetable;
- do not advance the calendar merely to “catch up” if mastery is absent.

## G13 — Controlled acceleration

If the learner already masters a prerequisite/topic, it may be fast-tracked only after a diagnostic problem demonstrates mastery. The diagnostic result is logged.
