# Timetable and Scheduling Control

## 1. Governed Monday-Friday study timetable — authoritative for daily study selection

This timetable is authorized by the student on 2026-09-30 using the rule:

> A module involving programming receives **two study days per week**; otherwise **one study day per week** is sufficient. Study days are Monday-Friday.

The classification below is grounded in the supplied syllabus material:

- **CSCM445 Machine Learning — programming-heavy:** the supplied learning outcomes explicitly require Python implementation/evaluation.
- **FEM & Structural Dynamics — programming-heavy:** the supplied module states MATLAB programming is used throughout.
- **EG-M384 Engineering Simulation — simulation-tool-led:** the supplied syllabus requires PC simulation tools but does not state a programming requirement.

### Weekly allocation

| Day | Governed study module | Programming allocation | Status |
|---|---|---|---|
| Monday | CSCM445 Machine Learning | Day 1 of 2 | locked |
| Tuesday | FEM & Structural Dynamics | Day 1 of 2 | locked |
| Wednesday | EG-M384 Engineering Simulation | 1 day of 1 | locked |
| Thursday | CSCM445 Machine Learning | Day 2 of 2 | locked |
| Friday | FEM & Structural Dynamics | Day 2 of 2 | locked |

Saturday and Sunday are **not scheduled study days** under the current rule. They may be used only if the student explicitly requests catch-up, review, assessment work, or a timetable change.

### Daily selection rule

When asked “What are we learning today?” use:

```text
DAY -> TABLE ABOVE -> MODULE -> CURRENT OFFICIAL SYLLABUS ITEM -> PREREQUISITE CHECK -> LESSON
```

Do not swap modules because another topic seems more useful or interesting.

## 2. University class timetable — separate and currently unknown

The supplied syllabus screenshots contain delivery/contact-hour information but do **not** provide exact university class days/times. The governed study timetable above is therefore a **self-study allocation**, not a claim about university lecture/lab times.

If the student later supplies the university class timetable, record it here separately. A known university class/assessment obligation may take precedence for preparation on that date, but any such override must be logged.

## 3. Syllabus-derived workload constraints

| Module | Taught/contact time stated in supplied material | Independent/private study stated |
|---|---:|---:|
| CSCM445 Machine Learning | 40 hours lectures and labs total | not specified in supplied screenshot |
| FEM & Structural Dynamics | 50 hours total; approximately 2-3 h/week | 70 hours total; approximately 3-4 h/week |
| EG-M384 Engineering Simulation | 10 h lectures + 30 h PC labs | not specified in supplied screenshot |

## 4. Within-day learning rhythm

The day determines the module; the current syllabus state determines the topic.

- **Opening (10-20 min):** retrieval of the previous session + prerequisite diagnostic.
- **Core (45-90 min):** first-principles derivation/concept development and one worked problem.
- **Applied block (30-90 min):** programming or simulation work when the syllabus requires it.
- **Closing (10-20 min):** mastery check, error log, and progress-state update.

For the second weekly day of a programming-heavy module, prefer implementation, debugging, verification, and retrieval of the same/current syllabus topic before advancing.

## 5. Timetable change protocol

A permanent change requires:

1. an explicit student instruction;
2. an entry in `CHANGELOG.md`;
3. an update to this file;
4. an update to `state/current.yaml` if the current day is affected;
5. no silent rearrangement of official syllabus order.
