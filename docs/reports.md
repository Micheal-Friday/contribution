---
type: process
status: living
updated: 2026-07-29
tags: [contribution, reports, naming, process]
aliases: [report conventions, writing reports]
---

# Writing reports

A report is a **document that leaves the repository**. It is stored here, and it is read somewhere else — attached to an email, pasted into a document, forwarded to someone who will never clone this repo and may not have access to it.

That single fact generates every rule below. **A report is not a documentation page.** Documentation assumes its reader is inside the tree; a report must assume its reader has nothing but the file in front of them.

Reports are produced **after every pull request, research pass, benchmark, or milestone** — so there will be many, they accumulate, and they need to be identifiable years later.

---

## 1. The self-containment rule

> **A report must be readable and complete with no access to this repository.**

| Rule | Why |
|---|---|
| **No repo-relative links in the body.** Not `[some-doc.md](../some-section/some-doc.md)`, not `docs/…`, not anchors | The link resolves to nothing once the file is attached to an email. A dead link is worse than no link — it tells the reader something exists and then denies it |
| **Cite sources as plain text**: document name, then section — `positioning.md §3`, `ADR-0010` | The reader can ask for it by name. Someone with repo access can find it in seconds |
| **External URLs are fine and encouraged** | They resolve everywhere. Vendor documentation, standards, press releases |
| **Every figure carries its source inline** | The reader cannot follow a link to check it |
| **Expand an abbreviation on first use, every time** | The reader may not have read the previous report, and probably has not read the tree |
| **State the scope in the header block**, not by implication | The reader does not know what branch or PR this came from |
| **Never cite a commit SHA from an unmerged branch** | It is unstable until it reaches `main`, and doubly useless to an external reader who cannot resolve it either way. Cite the pull request number instead — see [Commit messages](../git/commit-messages.md) |

Internal cross-references *between reports in the same set* are the one exception: refer to them by **report ID and title** (`R003 — Benchmark`), never by file path. A recipient who was sent all six can find it; a recipient who was sent one is not left with a broken link.

## 2. Naming

```
R<NNN>-<subject-slug>-<YYYY-MM-DD>.md
```

| Part | Rule |
|---|---|
| `R<NNN>` | Three digits, zero-padded, **sequential and never reused** — including for withdrawn reports. Same rule as ADR numbers, for the same reason: a citation must never become ambiguous |
| `<subject-slug>` | Lowercase kebab-case. What the report is *about*, not what triggered it. Short enough to read in a file listing |
| `<YYYY-MM-DD>` | The date the report describes, ISO-8601. **Not** the date the file was last touched |

Example: `R003-competitor-benchmark-2026-07-29.md`

**Why both a number and a date.** The number gives a stable citation (`R003`) that survives the subject being renamed. The date is what makes a shelf of reports navigable — reports describe a state at a moment, and the same subject will be reported on repeatedly.

**Why the date is last.** It reads as a qualifier on the subject rather than as the primary key, and it keeps reports on the same subject visually adjacent in a listing.

## 3. Section structure

Every report has the same shape. The reader learns it once and can then skim any report in the series.

**Mandatory, in this order:**

| Section | Contains |
|---|---|
| **Header block** | Report ID · date · subject · scope (branch, PR, or research pass) · reading time. Enough to identify the document with nothing else present |
| **Summary** | What this report says, in three to six sentences. Written so that reading only this is a defensible use of the reader's time |
| **The argument** *(or: Findings)* | The substance. Tables over prose |
| **The weakest link** | What to attack first, and why it might be wrong |
| **Sources** | Plain text. Document names and sections; external URLs in full |

**Add when the report type calls for it:**

| Section | Add when |
|---|---|
| **Coverage and confidence** | Research or benchmark reports. What was surveyed, what was not reachable, and how much of each finding to trust |
| **Decisions needed** | The report asks something of the reader. Put it **near the top**, not at the end — a decision buried on page nine does not get made |
| **What changed because of it** | The work described produced concrete changes elsewhere |
| **Status** | The subject is in flight — merged or not, shipped or not |

### The weakest-link section is mandatory, and here is why

A report that presents only strengths reads as advocacy and gets discounted whole. Naming the thinnest part of your own argument is faster for the reader than making them find it, and it is the difference between a report that can be *checked* and one that can only be *believed*.

It should name a specific claim, say what would falsify it, and say what it would cost if it were wrong. "There are some uncertainties" is not a weakest link.

## 4. Summary reports

A **summary report digests the findings of other reports**. It is not a table of contents and not a navigation page.

- It restates the substantive conclusions, condensed, so that reading only the summary is defensible.
- It is written **last**, and every claim in it must be defended in one of the reports it covers.
- It carries its own report number and date like any other.
- Navigation belongs in the index (§5), not in a summary.

The failure to avoid: a "summary" that tells the reader which document to open. That is an index wearing the wrong title, and a recipient who was sent only the summary learns nothing from it.

## 5. The index

The reports folder's own `README.md` is a **registry**, not a reading guide. One row per report, and it stays inside the repo — it is the one document in the folder that is not sent externally. Where that folder lives is the project's choice; record it in the project's own `CONTRIBUTING.md`.

Required columns: **file name · report ID · date · subject · area of context**.

*Area of context* is what a reader scanning for relevance needs: which part of the work this concerns — product definition, competitive research, architecture, delivery planning, and so on. It is the column that makes a shelf of forty reports usable.

## 6. Lifecycle

| Rule | Reason |
|---|---|
| **A report is never edited after it is issued** | It describes a state at a date. Editing it destroys the record of what was believed when, which is most of why reports are kept |
| **Corrections are issued as a new report** that names what it corrects | Same discipline as superseding an ADR |
| `status: complete` on issue. No draft state | A draft report has not been sent, so it is not yet a report |
| Reports are **not** living documents | The document lifecycle table in `CONTRIBUTING.md` records this explicitly |

## 7. Before sending

- [ ] No repo-relative link anywhere in the body.
- [ ] Every abbreviation expanded on first use.
- [ ] Every figure carries its source in the same sentence.
- [ ] The header block identifies the document with no other context present.
- [ ] The weakest-link section names a specific falsifiable claim.
- [ ] Any decision being asked for is near the top.
- [ ] Read it once as someone who has never seen the repository. Anything that only makes sense from inside is a defect.

---

## References

- [Document lifecycle](document-lifecycle.md) — the type table, where `report` is defined and its status vocabulary set
- [Contribution index](../README.md) — the three-layer rule these conventions follow
- [Pull requests](../github/pull-requests.md) — a PR merge is one of the triggers that produces a report
