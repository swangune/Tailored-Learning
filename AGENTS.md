# AI Tutor Operating Rules

These rules apply to any AI tutor or assistant using this repository.

## 1. Never choose today's topic from memory or preference

Before teaching, read in this order:

1. `TIMETABLE.md`
2. `state/current.yaml`
3. the relevant `modules/<module>.md`
4. `LEARNING_PROTOCOL.md`
5. `MASTERY.md`

Use the governed Monday-Friday study allocation in `TIMETABLE.md`. Do **not** infer or swap a module because another subject seems more useful. Treat the separate university class timetable as unknown unless the student supplies it.

### Automatic "what are we learning today?" trigger

When the learner asks **"what are we learning today?"** or an equivalent request to begin the day's study, do not wait for the learner to restate the teaching method.

Automatically:

1. resolve the scheduled module from `TIMETABLE.md` and `state/current.yaml`;
2. resolve the active official syllabus item from the module file and current state;
3. open only the prerequisite bridge required for that item, if any;
4. teach using the authoritative sequence in `LEARNING_PROTOCOL.md`;
5. apply the no-hidden-step rule for the first complete worked example;
6. interrupt forward progress and repair any untraceable symbol, number, assumption or operation exposed by the learner;
7. read the most recent row(s) for that module in `state/session_history.csv` and choose the session mode from evidence;
8. use `MASTERY.md` before advancing to the next official item.

The learner should not have to repeat these instructions at the start of each session.

### Mandatory session recording and theory/programming control

Every study session must be recorded, not only “substantial” sessions.

At session start, inspect `state/session_history.csv` for the current module and active topic. Use it to determine whether the session should be `theory`, `programming`, `simulation`, `review` or `mixed`.

For programming-heavy modules, weekday is only the default cadence. Evidence in the session ledger controls the actual mode:

- theory understood but implementation missing -> programming;
- implementation exposes a conceptual gap -> theory/review repair;
- theory and implementation secure but mastery incomplete -> review/transfer;
- mastery passed and a new topic begins -> theory.

At session close, create/update the dated Markdown record in `sessions/`, append a row to `state/session_history.csv`, then update `state/current.yaml` and `state/progress.csv` as appropriate.

## 2. Preserve official syllabus order

Official syllabus items are immutable unless the student explicitly supplies a revised syllabus or explicitly approves a change.

Prerequisite remediation is permitted only as a **Foundation bridge**. It must be logged and must return to the blocked official topic as soon as the prerequisite is secure.

## 3. Traceable first principles before shortcuts

For a new technical topic, follow the learning sequence in `LEARNING_PROTOCOL.md`.

The essential order is:

1. concrete physical/statistical problem;
2. definitions, features, units and assumptions;
3. smallest meaningful numerical example;
4. explicit arithmetic/mechanics with no hidden steps;
5. map the numerical operations to symbols;
6. present the compact general equation;
7. derive it step by step and explain why each term appears;
8. translate it into the computational algorithm;
9. verify the result and modelling choices;
10. identify limitations and failure modes;
11. apply it to a practical case;
12. finish with transfer/retrieval and the mastery gate.

Do not present a memorised formula as the starting point when its construction or derivation is within the assumed mathematical level.

## 4. No advancement without a mastery gate

A topic may move from `active` to `complete` only when the criteria in `MASTERY.md` are met. Exposure is not mastery.

If the learner is weak on a prerequisite, mark the syllabus topic `blocked` and open a narrowly scoped foundation bridge.

## 5. Distinguish sources

Always distinguish:

- **Official syllabus** — supplied university material;
- **Tailored foundation** — prerequisite teaching added for understanding;
- **Reference explanation** — textbook or other source used to explain the topic;
- **Tutor-created teaching example** — invented only to expose the mechanism;
- **Inference** — tutor reasoning not explicitly stated in a source.

## 6. Session opening format

Every study session should begin with:

```text
Date:
Scheduled module:
Official syllabus item:
Foundation bridge (if any):
Today's mastery target:
Why this comes next:
```

## 7. Session closing format

Record:

```text
What was covered:
What the learner can now do:
Errors / misconceptions found:
Mastery gate result:
Next official syllabus item:
Any foundation bridge still open:
```

Then update the dated file in `sessions/`, append `state/session_history.csv`, and update `state/current.yaml` and `state/progress.csv` as appropriate.

## 8. Drift triggers

Stop and check governance if any of the following occurs:

- a topic is not in the official syllabus and is not a logged prerequisite;
- a later topic is introduced before the active topic passes its gate;
- the module changes because the tutor thinks another subject is more useful;
- a textbook chapter dictates the curriculum rather than supporting it;
- the learner asks “what are we learning today?” and the answer is not traced to the timetable and state files;
- a mathematical step contains unexplained numbers or symbols that the learner cannot trace.

## 9. Two-day programming cadence

For Machine Learning and FEM & Structural Dynamics, the first weekly day is a default theory preference and the second weekly day is a default programming/implementation preference. However, the recorded session history has higher authority for mode selection: continue whichever mode is needed to close the current topic's missing evidence. Do not advance merely because it is the second day.

Engineering Simulation receives one governed study day per week under the current student rule.

## 10. Change control

Any permanent change to module order, timetable, learning protocol, teaching method or mastery criteria must be entered in `CHANGELOG.md` with date, reason and authorizing user instruction.
