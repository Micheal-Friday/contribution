<!--
  Copy this file to the root of a project that follows these conventions.
  Replace every <angle-bracket> placeholder and delete what does not apply.

  Keep it short. This file holds three things and nothing else:
    1. the link to the shared conventions
    2. this project's own values
    3. this project's own exceptions

  If you find yourself restating a rule that lives in the shared repo, stop —
  two copies of a rule is exactly the drift the split exists to prevent.
-->

# Contributing

This project follows the shared conventions at
**[Micheal-Friday/contribution](https://github.com/Micheal-Friday/contribution)**.

Read those for the rules. This file records only what is **specific to this
project**: the values the rules take here, and the places this project
deliberately differs.

| Area | Where |
|---|---|
| Branch names | [branch-naming](https://github.com/Micheal-Friday/contribution/blob/main/git/branch-naming.md) |
| Commit messages | [commit-messages](https://github.com/Micheal-Friday/contribution/blob/main/git/commit-messages.md) |
| Issues | [issues](https://github.com/Micheal-Friday/contribution/blob/main/github/issues.md) |
| Pull requests | [pull-requests](https://github.com/Micheal-Friday/contribution/blob/main/github/pull-requests.md) |
| Status updates | [issue-updates](https://github.com/Micheal-Friday/contribution/blob/main/github/issue-updates.md) |
| Document lifecycle and naming | [document-lifecycle](https://github.com/Micheal-Friday/contribution/blob/main/docs/document-lifecycle.md) |
| Decision records | [decisions](https://github.com/Micheal-Friday/contribution/blob/main/docs/decisions.md) |
| Reports *(if this project issues them)* | [reports](https://github.com/Micheal-Friday/contribution/blob/main/docs/reports.md) |
| Research *(if this project produces benchmarks)* | [research](https://github.com/Micheal-Friday/contribution/blob/main/docs/research.md) |

**Pinned to:** `main` · *or* `<tag>` — pin only if a convention change would
disrupt work in flight. Say which, so nobody has to guess whether a rule that
arrived yesterday applies here.

---

## This project's values

The shared guides define the **rules**. These are the **values** they take here.

### Commit scopes

A scope names the smallest part of this system that ships, is reviewed, and is
retired as one unit.

| Family | Values |
|---|---|
| `<the released artifact>` | `<name>` |
| `<a component>` | `<name>`, `<name>` |
| `<a docs section>` | `<name>`, `<name>` |

Omit the scope when a change genuinely spans the project.

### Commit types in use

`<feat>` · `<fix>` · `<docs>` · `<chore>` — and anything else this project has
adopted, with what it means here.

### Branch types in use

`<docs/>` · `<feat/>` · `<fix/>` · `<chore/>` — plus one per numbered record
series this project keeps, e.g. `<adr/>`.

### Labels

One label answers one question a person filtering the list needs answered that
the title cannot. Within an axis, exactly one value applies.

| Axis | Values in use |
|---|---|
| `<kind of work>` | `<label>`, `<label>` |
| `<area>` | `<label>`, `<label>` |

Labels never applied are noise — prune rather than accumulate.

### Where work is recorded

| Question it answers | Where |
|---|---|
| who is doing what, and when is it finished | `<issue tracker>` |
| what was decided, and what it cost | `<path to decision records>` |
| what we want, and in what order | `<path to backlog, or "none">` |

---

## This project's exceptions

Anything this project does **differently** from the shared conventions, and
why. An exception with no reason is drift with better manners.

- `<exception, and the reason>`

If there are none, say so — silence reads as an omission rather than a
decision.

---

## What this project cannot honestly enforce

A convention nobody here can follow is worse than none: it teaches people the
document is decorative. Record the gap rather than the aspiration.

| Convention | Reality here |
|---|---|
| Author ≠ reviewer | `<e.g. aspirational at one maintainer; substitute is a self-review pass plus a cooling-off period, and the change request says which it got>` |
| `<other>` | `<reality>` |

**Revisit this table whenever the team size changes.** Each row is a rule
waiting for a precondition, not a rule that was rejected.

---

## Prior art that is not the model

These conventions start on `<YYYY-MM-DD>`. Earlier history does not comply and
is **not** being rewritten — a log edited to look tidy is worth less than one
that is accurate.

- `<what predates the rules, specifically>`

**Cite this file for what to do next, never as a description of what the log
already looks like.**

---

## Project-specific rules

Anything that governs *this* product and belongs nowhere else — how to change a
component, what must be tested, domain constraints a contributor cannot guess.

`<or delete this section>`
