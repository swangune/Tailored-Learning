# Computational Engineering Learning Repository

A governed study system for the computational-engineering modules supplied by the student.

The repository exists to solve two problems:

1. learn each module from first principles without losing the official syllabus;
2. prevent topic drift by making the syllabus, timetable, prerequisites, mastery gates and session state explicit.

## Source of truth hierarchy

When deciding what to study, use this order:

1. **User-provided university syllabus / timetable**
2. `TIMETABLE.md`
3. `SYLLABUS.md` and the relevant file in `modules/`
4. `state/current.yaml` and `state/session_history.csv`
5. `LEARNING_PROTOCOL.md`
6. `TEACHING_STANDARD.md`
7. reference textbooks and external material

A textbook may explain a syllabus topic, but it may not silently replace, reorder or expand the syllabus.

## Current modules

- **CSCM445 — Machine Learning**
- **Finite Element Method & Structural Dynamics** — module code not visible in the supplied syllabus image
- **EG-M384 — Engineering Simulation**

## Session startup

Every study session begins by checking:

```text
DATE -> TIMETABLE -> MODULE -> CURRENT SYLLABUS ITEM -> PREREQUISITES -> LEARNING OBJECTIVE
```

The governed study timetable currently identifies one module for every Monday-Friday study day. The separate university lecture/lab timetable remains unknown and must not be invented.

## Repository map

- `AGENTS.md` — rules for ChatGPT/AI tutors working in this repository
- `SYLLABUS.md` — official syllabus snapshot from the supplied material
- `GOVERNANCE.md` — anti-drift rules and change control
- `TIMETABLE.md` — university timetable + governed self-study allocation
- `LEARNING_PROTOCOL.md` — first-principles learning sequence
- `TEACHING_STANDARD.md` — authoritative teaching-execution standard
- `MASTERY.md` — progression gates
- `modules/` — module-specific learning paths
- `state/current.yaml` — single current learning state
- `state/session_history.csv` — session-mode history controlling theory/programming/simulation/review emphasis
- `state/progress.csv` — auditable topic progress
- `sessions/` — dated study-session records
- `references/` — mapping of supporting books/resources
- `CHANGELOG.md` — approved changes to syllabus/timetable/governance

## Important distinction

Sections labelled **Official syllabus** are derived from the material supplied by the student. Sections labelled **Foundation bridge** or **Tailored learning path** are study scaffolding added to build the prerequisite understanding needed to master the official syllabus.

## Current weekly study allocation

- Monday — CSCM445 Machine Learning
- Tuesday — FEM & Structural Dynamics
- Wednesday — EG-M384 Engineering Simulation
- Thursday — CSCM445 Machine Learning
- Friday — FEM & Structural Dynamics

This implements the student rule that programming-heavy modules receive two days per week and other modules one day per week.
