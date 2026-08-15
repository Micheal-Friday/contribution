---
type: process
status: living
updated: 2026-07-29
tags: [contribution, adr, decisions, madr, process]
aliases: [architecture decision records, ADR conventions, decision records]
---

# Architecture decisions

An **architecture decision record** (ADR) answers one question and nothing else: *what did we choose, and why?*

It exists because the reasoning behind a choice decays faster than the choice does. Six months later the constraint that forced the decision is invisible — the code, the schema, or the package layout looks arbitrary, and the next person either works around it or reverses it without knowing what it was defending. **An ADR is a message to whoever hits that constraint next.**

It is deliberately *not* a unit of work, and not a ranked intention. Those are different questions with different lifecycles — see [Writing issues](../github/issues.md) §*Issue, ADR, or backlog row*.

| The question | Produces | Ends by |
|---|---|---|
| *Who is doing what, and when is it finished?* | a unit of work — an issue | being delivered |
| *What did we choose, and why?* | **a decision — an ADR** | being superseded; never edited, never deleted |
| *What do we want, and in what order?* | a ranked intention — a backlog row | being promoted, or re-triaged |

A single piece of thinking usually produces all three, and that is not duplication — it is the point. The issue tracks the doing, the ADR records the choosing, the backlog records the ordering.

---

## 1. The bar

> **An ADR is warranted when the choice would be expensive to reverse and confusing to rediscover.**

Four triggers. They are deliberately about *consequence*, not about effort or size — a one-line change that narrows what the product is clears the bar, and a two-week feature build does not.

| Trigger | Why it clears the bar |
|---|---|
| **Anything that changes what the product is or is not** | The boundaries are the thing everything else is measured against. Move one silently and every downstream judgement — scope, prioritisation, what counts as out of scope — is being made against a definition nobody agreed to |
| **Anything that changes the shape of persisted data** | Persisted data outlives every process around it. A field's meaning, once written by real users, cannot be changed by editing a document — it needs a migration, and the migration needs to know what the old shape meant and why |
| **Anything that changes how the product is packaged, distributed, or extended** | These choices are load-bearing for consumers you cannot contact. Some are irreversible in practice: a published package name, a public extension point, an install path already in someone's config |
| **Any reversal of an earlier decision — even one that had no record** | This is the trigger people skip, and it is the most valuable one. If the original choice was never recorded, the reversal is the *first* chance to capture why the project believes what it believes. Skipping it because "there was nothing to supersede" throws away the only artifact the episode was going to produce |

### What does not need one

**Feature decisions do not need an ADR.** A capability the project wants, ranked against the other things it wants, is a backlog row — it ends by being promoted or re-triaged, not by being superseded. Writing an ADR for it produces a permanent record of a temporary preference, and it trains people to skim the decision folder, which is exactly the folder that must not be skimmed.

The tell: **if the output is a rank or a schedule, it is not a decision.** If the output is a constraint that later work has to live inside, it is.

### Two mistakes worth naming

| Mistake | What goes wrong |
|---|---|
| **"Decide whether X" opened as an issue** | If the deliverable is a judgement and no files change, that is an ADR in `proposed`. Opened as an issue, the alternatives and the rationale end up in a comment thread that nothing cites and no document links — and the next person rediscovers all of it |
| **A decision taken in a comment thread** | A comment thread is not durable, not linkable from the docs tree, and not indexed. **Anything that must still be findable in a year goes into a record, and the thread links to it.** The comment reports the move; the record *is* the move |

The legitimate hybrid: when a decision has been taken and files now have to move as a result, open **both** — the ADR carries the reasoning, the issue carries the labour.

---

## 2. Format

**ADRs use [MADR 4.0.0](https://adr.github.io/madr/).** Not because the sections are magic, but because a shared shape means a reader can find the *Decision Outcome* of a record they have never seen in under five seconds, and because tooling — [`adr-tools`](https://github.com/npryce/adr-tools) and its relatives — already understands it. A project that invents its own format pays for the invention twice: once writing the format, and again every time someone has to learn it.

| Section | Job | Failure mode when it is thin |
|---|---|---|
| **Context and Problem Statement** | what the situation is, and what forces the choice **now** | Without a *now*, the record reads as opinion and gets reversed by the next person with a different opinion |
| **Decision Drivers** | the constraints the answer had to satisfy | These are what survive the decision. When a driver later turns out to be false, the record tells you exactly which conclusion to revisit |
| **Considered Options** | the alternatives, stated fairly | An option list with one plausible entry is an advertisement. The rejected options are the record's main defence against being relitigated |
| **Decision Outcome** | what was chosen, and why *this* one | The one section a hurried reader will read. It must be readable alone |
| **Consequences** | good and bad, stated honestly | A record with no listed downside is not trusted, and rightly — every real choice costs something |

Two rules about content, both about staying in lane:

- **ADRs cite the strategy, product, and research documents; they do not restate them.** The documents explain; the ADRs commit. A restatement is a second copy that drifts, and then two documents disagree about what the project believes.
- **Every material claim that came from evidence names its source.** A driver sourced from a benchmark or a standard is checkable; a driver sourced from nowhere is a preference wearing a citation's clothes.

Frontmatter carries at least `status` and `date`, plus `supersedes:` / `superseded-by:` where they apply. The `date` is **the date the decision was made**, not the date the file was written — records written retroactively, at the point a project establishes its decision folder, should say when the thinking actually happened.

---

## 3. Numbering

> **Numbers are sequential, monotonic, and never reused — including for withdrawn records.**

`NNNN-kebab-title.md`, four digits, zero-padded, one counter for the series. This is the general naming rule in [The document lifecycle](document-lifecycle.md) §*Naming*, and it binds hardest here because **an ADR number is a citation.** `ADR-0012` appears in commit bodies, branch names, other records, issue comments, and code comments. If it can mean two things depending on when you read it, every one of those references is now ambiguous — including the ones written before the reuse, which nobody will think to check.

| Rule | Reason |
|---|---|
| **Reserve the number before you start writing** | The branch, the file, and the pull request all have to agree before anyone else picks the same number. Reserving last means discovering the collision during review |
| **A withdrawn or abandoned record still burns its number** | A gap in the sequence reads as a decision that was withdrawn — recoverable information. A reused number reads as correct and is wrong |
| **The branch carries the number too** — `adr/<NNNN>-<title>`, and `<type>/<NNNN>-<short-title>` for a branch implementing exactly one record | Decision records have no automatic backlink from a version-control host, the way issues do from commit trailers. The number in the branch name is what makes everything that touched a decision findable from the decision. See [Branch naming](../git/branch-naming.md) |
| **The number never changes**, including when the record moves to an archive | Moving a file is a location change, not an identity change |

---

## 4. Status

> **`proposed` is not a soft `accepted`. A recommendation is not a decision.**

This is the rule that decays first and costs the most when it does. `proposed` marks a decision **that has not been taken** — the alternatives are still live, the record can still be rejected outright, and nothing downstream may be built on it.

| Status | Means | What may be built on it |
|---|---|---|
| `proposed` | written, argued, **not decided** | Nothing. Work that assumes it is speculative work |
| `accepted` | decided. Binding on later work | Everything. This is the constraint others live inside |
| `rejected` | considered and declined | Nothing — but the record stays, permanently. **A rejected record is one of the most useful kinds**, because the next person to propose the same thing gets the reasons for free |
| `deprecated` | no longer applies, and nothing replaced it | Nothing. The situation it addressed is gone |
| `superseded by ADR-NNNN` | replaced by a specific later record | Follow the pointer |

Why the distinction is worth defending: a `proposed` record that is treated as settled gets implemented, and then the decision is made by whoever wrote the code rather than by the review that was supposed to make it. The record's status is the only signal telling a reader which of those two happened. **The moment `proposed` starts meaning "probably yes", the folder stops being a decision record and becomes a folder of drafts.**

Practical consequence: the status change from `proposed` to `accepted` is itself a reviewed change. It happens in a pull request, and the pull request thread *is* the discussion. Reporting the move in an issue comment is fine and useful — but the comment reports the move; the record is the move. See [Status updates on issues](../github/issue-updates.md).

Most exploratory writing belongs in a record with `status: proposed` rather than in some separate proposal venue. If the writing is not yet shaped like a decision — no options, no drivers, just a question — that is the one case for a different vehicle, and it should converge to a `proposed` record quickly or be abandoned visibly.

---

## 5. Supersession

> **Supersession is additive. A new record, a status change on the old one, and never an edit.**

| Step | Change | Why |
|---|---|---|
| 1 | **Write a new record** with its own next number, `supersedes: ADR-NNNN`, and the reasoning for the reversal | The reversal's reasoning is new information and deserves a first-class record, not a paragraph appended to something else |
| 2 | **Change the old record's status** to `superseded by ADR-MMMM` — the status line only | A reader arriving at the old record via a stale citation must be told inside the record that it has been replaced, and by what |
| 3 | **Leave the old record's body exactly as written** | It is the evidence of what was believed, with what information, at that date. That is most of why it was kept |

The status line is the **only** part of an accepted record that ever changes. Everything else is frozen at merge.

Two consequences that surprise people:

- **An accepted record never gains an "implemented in …" line.** That would be an edit. The implementation cites the decision, not the reverse — and implementation status is tracked separately, because **merged ≠ built**. See [The document lifecycle](document-lifecycle.md) §*Merged ≠ built*.
- **A record is not corrected in place, even for a factual error in its reasoning.** If a driver turns out to have been false, that is a new record superseding the old one, and it is a far more useful artifact than a silent fix: it names a way this project's reasoning went wrong, which is the kind of thing worth being able to find again.

---

## 6. Template

A worked skeleton. Keep it short — **the shortest record that answers the question is the best one**, because a record nobody finishes reading is a record nobody is bound by.

```markdown
---
status: proposed
date: 2026-07-29
decision-makers: [role or name]
consulted: []
informed: []
tags: [area, area]
---

# 0012 — Short imperative title naming the choice

## Context and Problem Statement

What the situation is, and what forces a choice now. Two or three
sentences. Cite the strategy, product, or research document that
supplies the evidence — do not restate it.

## Decision Drivers

- The constraint the answer must satisfy, with its source.
- Another one. If a driver later proves false, this list is what
  tells the next reader which conclusion to revisit.

## Considered Options

1. The option that looks obvious.
2. The cheap option.
3. **The chosen one.**

## Decision Outcome

**Chosen: option 3**, because <the reason, in one sentence a reader
can disagree with>.

### Consequences

- Good: what this buys.
- Bad: what it costs. If this section is empty, the options were
  not stated fairly.
- Neutral: what changes that is neither.

## More Information

What would trigger a revisit — a named condition, not "if things
change". Links to the issue carrying the implementation labour, if
there is one.
```

Where records live, and which file indexes them, is a project choice — set it in your own `CONTRIBUTING.md` — see the [stub template](../templates/CONTRIBUTING.md). What is not a project choice: they live somewhere **durable, linkable, and append-only.** A comment thread is none of the three.

---

## References

- [`CONTRIBUTING.md` stub template](../templates/CONTRIBUTING.md) — where a project sets its decision folder, index, and any additional triggers
- [Decision record template](../templates/adr.md)
- [The document lifecycle](document-lifecycle.md) — the `decision` type, the naming rule numbering follows, supersession, and *merged ≠ built*
- [Research](research.md) — where the evidence a record cites is produced; research informs decisions, it does not make them
- [Writing reports](reports.md) — how a decision is communicated outside the repository
- [Branch naming](../git/branch-naming.md) — the `adr/<NNNN>` and `<type>/<NNNN>` forms, and why the number goes in the branch
- [Commit messages](../git/commit-messages.md) — citing a record from a commit body
- [Writing issues](../github/issues.md) — issue vs decision vs backlog row, and the two mistakes above in full
- [Status updates on issues](../github/issue-updates.md) — reporting a status change without deciding it in a comment
- [Pull requests](../github/pull-requests.md) — the review thread where `proposed` becomes `accepted`
- [MADR 4.0.0](https://adr.github.io/madr/) · [`adr-tools`](https://github.com/npryce/adr-tools) — the format, and tooling that understands it
