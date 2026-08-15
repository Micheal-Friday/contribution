---
type: process
status: living
updated: 2026-07-29
tags: [contribution, research, benchmark, evidence, process]
aliases: [research conventions, benchmark conventions, research passes]
---

# Research

> **Opt-in.** This document applies only to projects that produce benchmark or survey work — competitive analysis, standards surveys, capability comparisons, anything where a conclusion rests on gathered external evidence. A project that produces no such work does not need these conventions and should not adopt them for completeness. Adopting a convention nobody exercises teaches people that conventions here are decorative.

A **research pass** is one bounded investigation, written down. Its job is to put evidence somewhere a decision can cite it — and, just as importantly, to say what it could not find out.

The rules below all serve one property: **a reader must be able to tell what is established from what is assumed.** A research document that blurs that line is more dangerous than no document, because its conclusions get built on and its gaps do not.

---

## 1. Scope and naming

| Rule | Reason |
|---|---|
| **One file per research pass** | A pass has a question, a date, and a boundary. Two passes in one file share none of those, so nothing in it can be cited precisely or superseded independently |
| **Named for the area, not the trigger** — e.g. `benchmark-<area>.md` | The file will be cited long after whatever prompted it is forgotten. Name the subject; the prompt goes in the document's opening lines |
| **Lowercase ASCII `kebab-case`** | Same rule as every other file — see [The document lifecycle](document-lifecycle.md) §*Naming* |
| **Frontmatter carries `type: research`, `status: complete`, and `researched:`** | `researched:` is the date the **evidence was gathered**, which is not the date the file was written and not the date it was last touched. It is the only field that tells a reader in 2028 how stale the findings are |

The exact prefix and the folder are a project choice — set them in your own `CONTRIBUTING.md` — see the [stub template](../templates/CONTRIBUTING.md). One file per pass, named for its subject, is not.

---

## 2. Evidence

> **Every material claim carries a source URL.**

| Rule | Reason |
|---|---|
| **Every material claim carries a source URL** | A claim without a source cannot be rechecked, which means it cannot be updated when the world moves and cannot be defended when someone disagrees. It is an assertion in a document that is supposed to contain evidence |
| **Vendor-reported figures are labelled as such** | A vendor's own number is evidence of what the vendor claims, which is a different fact from the one it looks like. Labelling it costs four words and prevents a marketing figure being cited later as a measurement |
| **"Unverified" means "could not be confirmed from a primary source," not "false"** | These are opposite conclusions and get collapsed constantly. Marking something unverified says the pass reached its limit; it says nothing about the claim's truth, and treating it as a refutation is how real capabilities get written off |
| **Never silently promote an unverified claim** | Promotion happens by retyping — the claim reappears in a summary, a decision record, or a slide without its qualifier, and by the third hop it is a fact. If evidence later confirms it, promote it explicitly and say what confirmed it |
| **Conflicting published figures are recorded as conflicts** — not averaged, not picked | The disagreement is itself a finding, and often the most useful one in the pass. An average of two numbers that cannot both be true is a third number that is definitely not true, and picking one silently hides the judgement in a place no reviewer can see it |

---

## 3. Gaps

> **State what the pass could not reach.**

A benchmark that hides its gaps is worse than no benchmark, because **conclusions get built on absence.** A reader who cannot see the boundary of the survey assumes there was none — that everything relevant was looked at and what is not in the document is not out there. That inference is wrong almost every time, and it is invisible.

Every pass ends with an explicit coverage statement naming at least:

| State | What to say |
|---|---|
| **Surveyed** | What was actually examined, by name |
| **Not reachable** | What was looked for and could not be obtained — paywalled, undisclosed, no primary source, no response |
| **Out of scope** | What was deliberately excluded, and why. Different from unreachable, and the difference matters to whoever extends the pass |
| **Confidence** | How much weight each finding will bear. A finding drawn from one vendor's own page and a finding confirmed across three independent sources are not the same finding |

This is the same discipline as the weakest-link section in [Writing reports](reports.md) §3, for the same reason: **naming the thinnest part of your own work is faster for the reader than making them find it**, and it is the difference between a document that can be checked and one that can only be believed.

---

## 4. Research does not make decisions

> **Decisions cite research. Research does not decide.**

| Boundary | Why it is drawn here |
|---|---|
| A pass may state that the evidence **points** somewhere | That is a finding, and it is what the pass is for |
| A pass may **not** record the choice | A choice has alternatives, drivers, consequences, and a status — none of which a research document carries, and all of which the next reader needs. See [Architecture decisions](decisions.md) |
| A decision record **cites** the pass; it does not restate it | A restatement is a second copy that drifts, and then two documents disagree about what the evidence said |

The practical failure this prevents: a benchmark that ends with "we should therefore do X" becomes the de facto decision, without ever having been argued as one, and without a status anyone can check. When somebody later asks whether X was ever decided, the honest answer is no — it was concluded in a document that had no mechanism for deciding anything. **A finding that is strong enough to act on is strong enough to justify writing the decision record.**

Research correcting an existing design is the record working as intended, not a failure of the design — but the correction still lands as a new decision record, not as a paragraph in the benchmark.

---

## 5. A research pass is read-only once written

> **Corrections are a new pass, not an edit.**

`status: complete` on issue; there is no draft state and no revised state. A pass describes **what could be established at a date**, and that is the whole of its value. Editing it destroys the record of what was known when, which is precisely the thing later readers need in order to judge a decision that was made on it — was the decision reasonable given the evidence available, or was the evidence there and ignored? An edited document cannot answer that.

| Situation | Do this | Not this |
|---|---|---|
| A figure turns out to be wrong | New pass naming what it corrects, with the new evidence | Fix the number in place |
| The market or the field has moved | New pass with a later `researched:` date | Refresh the old file |
| An unverified claim is now confirmed | New pass, saying what confirmed it | Delete the qualifier |
| A typo, a broken link, a wrong heading level | Fix it | — presentation is not findings |

This is the same rule [Writing reports](reports.md) §6 applies to reports, for the same reason, and the same rule [Architecture decisions](decisions.md) §5 applies to accepted decisions. The three document types that are **dated claims about a state of the world** are all append-only. Living documents — strategy, product, architecture, process — are edited freely, because they claim to describe *now*.

Superseding a pass is additive: the new document names what it corrects, and the old one keeps its name, its date, and its text.

---

## 6. Before publishing a pass

- [ ] Every material claim has a source URL in the same row or sentence.
- [ ] Every vendor-reported figure is labelled as vendor-reported.
- [ ] Every unverified claim says *unverified*, and says what was tried.
- [ ] Conflicts are recorded as conflicts, with both figures and both sources.
- [ ] The coverage statement names what was surveyed, what was unreachable, and what was excluded.
- [ ] No sentence decides anything. Findings point; records decide.
- [ ] `researched:` is the date the evidence was gathered, not today.

---

## References

- [`CONTRIBUTING.md` stub template](../templates/CONTRIBUTING.md) — where a project declares whether it produces research, and where passes live
- [The document lifecycle](document-lifecycle.md) — the `research` type, `researched:`, naming, and the append-only family
- [Architecture decisions](decisions.md) — what a finding has to become before anything is built on it
- [Writing reports](reports.md) — the coverage-and-confidence and weakest-link sections, and the never-edited rule shared with this document
- [Writing issues](../github/issues.md) — how a research pass is scoped and reported against as a unit of work
- [Pull requests](../github/pull-requests.md) — a research pass lands through review like anything else
