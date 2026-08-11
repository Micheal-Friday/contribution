---
type: process
status: living
version: 1.0
updated: 2026-07-29
tags: [contribution, docs, lifecycle, naming, frontmatter, process]
aliases: [document lifecycle, naming conventions, frontmatter, versioning]
---

# The document lifecycle

A document in a docs-as-code repository has four things a reader has to be able to trust: a **name** that stays valid, a declared **type**, a **status** that says how much weight to put on it, and an **end** — the state in which it stops being current. This file governs all four.

The rules exist because documents outlive the person who wrote them, and because a document whose status is ambiguous is worse than no document: it gets cited as settled by someone who was not there.

**The values in the tables below are a starting set.** Where a project needs its own types, folders, or archive location, it sets them in its own `CONTRIBUTING.md` — see the [stub template](../templates/CONTRIBUTING.md) — and states the rule it used — not a preference, a rule.

---

## 1. Naming

> **ASCII, lowercase, `kebab-case`. No spaces, no `&`, no Title Case.**

| Rule | Reason |
|---|---|
| **Files: ASCII, lowercase, `kebab-case`** | Names get typed into shells, pasted into URLs, and checked out onto case-insensitive filesystems. A capital letter or a space is a defect waiting for the first contributor on a different OS. `&` and spaces additionally need escaping in half the tools that will touch the file |
| **Numbered series: `NNNN-kebab-title.md`** — four digits, zero-padded | Zero-padding sorts correctly in every file listing without a custom comparator. Four digits is enough that no series will need to widen, and widening later renames every existing file |
| **Sequential per series, never reused** — including for withdrawn records | The number *is* the citation. If `0012` can mean two different things depending on when you read it, every citation of `0012` becomes ambiguous, including ones written before the reuse. A withdrawn record still burns its number: a gap reads as a decision, a reused number reads as data loss |
| **Each series keeps its own counter** | Two series sharing a counter means neither can be numbered without consulting the other, and a citation of "0007" no longer identifies a document |
| **Archive: ISO-8601 date prefix** — `2026-07-28-positioning-v1.md` | An archived document is identified by *when it was true*, not by where it sat. The prefix sorts chronologically and survives being moved or attached to something |
| **Dates: ISO-8601 everywhere**, including inside prose | `2026-07-29` is unambiguous in every locale; `07/29/26` is not. **Convert relative dates before writing them down** — "last Tuesday" is unresolvable the moment the reader is not you, and "recently" was already false when it was committed |

**Where the archive lives is a project choice; the date prefix is not.** Set the path in your `CONTRIBUTING.md`. Every project needs somewhere superseded documents go, and it needs to be the same place every time, or "is this still current?" becomes a search problem.

---

## 2. Frontmatter

**Every document carries YAML frontmatter with at least `type`, `status`, and `updated`.** Those three answer the only questions a reader has before deciding whether to keep reading: what kind of thing is this, how settled is it, and how stale is it.

### The current set

This is the type table as it stands. It is not a closed list — see *Adding a type* below.

| `type` | What it holds | `status` values | Versioned? |
|---|---|---|---|
| `strategy` | what the project is trying to be, and why | `draft` → `accepted` → `superseded` | No — **living document**; the git log is the history |
| `product` | what the product does, for whom, in what order | `draft` → `accepted` → `superseded` | No |
| `architecture` | how it is built and how the pieces fit | `draft` → `accepted` → `superseded` | No |
| `decision` (ADR) | one choice, its alternatives, and its consequences | `proposed` → `accepted` \| `rejected` → `deprecated` \| `superseded by ADR-NNNN` | No — supersession instead. See [Architecture decisions](decisions.md) |
| `research` | evidence gathered at a point in time | `complete` | No — carries `researched:`, the date the evidence was gathered. See [Research](research.md) |
| `process` | how work is done here — the contribution and workflow guides | `living` | No |
| `report` | a document that leaves the repository | `complete` | No — carries `audience:` and describes a state at a date. **Never edited after the fact**; write a new one, so the record of what was believed when survives. See [Writing reports](reports.md) |
| `index` | a registry of other documents | `living` | No |

### Adding a type

> **A type earns its place when a reader needs to treat the document differently — not when a new folder appears.**

| Test | Fails when | Reason |
|---|---|---|
| **It implies a different status ladder** | the proposed type would use `draft → accepted → superseded` like three existing ones | If the lifecycle is identical, the distinction is a topic, and topics are what folders and `tags` are for |
| **It implies a different end state** | the document ends the same way an existing type ends | The end state is most of what `type` communicates: superseded, complete, or living forever |
| **A tool or a rule keys off it** | nothing reads it | An unread field drifts, and a drifted field is worse than a missing one because people still trust it |

A type that passes all three is added by a pull request to the project's `CONTRIBUTING.md` in the same change that creates the first document using it. **Do not add a type speculatively** — an unused type is an invitation to file things under it wrongly.

Retired types are dropped from the table and **left alone in existing documents**. Rewriting old frontmatter to match a new taxonomy destroys the record of how the project used to think, in exchange for tidiness nobody asked for.

---

## 3. The versioning boundary

> **Only shipped artifacts get SemVer. Documents get status and supersession.**

| Thing | Carries | Why |
|---|---|---|
| **The released artifact** — a package, a plugin, a library, whatever this project actually ships | SemVer in its manifest, bumped in the same pull request as the change, with a changelog entry | Consumers depend on it and need a compatibility contract. That is what SemVer *is*: a promise about breakage, made to someone downstream |
| **Every document** | `status`, `updated`, and supersession | Nobody depends on revision 3 of a strategy document. They depend on the *current* one, and on being able to tell that it is current |

**A version number on a living document is a lie about how it changes.** SemVer claims that a bump means something specific — that MAJOR broke a contract, that MINOR added compatibly. A document has no contract to break, so every bump is a judgement call, which means the numbers stop carrying information almost immediately. Worse, a stale `version: 1.2` on a document edited eleven times since reads as authoritative precision. `updated:` plus the git log tells the truth and costs nothing to maintain.

Which artifacts carry SemVer, and where their manifest lives, is a project fact — state it in your own `CONTRIBUTING.md`. Two consequences that hold regardless:

- **A pre-1.0 artifact may leave `version` unset** and let the commit SHA drive updates. That is honest; `0.0.1` bumped by hand is not.
- **The commit type drives the bump** — `fix:` → PATCH, `feat:` → MINOR, `!` or `BREAKING CHANGE:` → MAJOR — and it applies **only** to the shipped artifact. See [Commit messages](../git/commit-messages.md). A docs-only commit bumps nothing, because nothing downstream can break.

---

## 4. Supersession

> **Never delete or rewrite an accepted decision.**

The procedure is three steps and all three are required:

| Step | What | Why it is not optional |
|---|---|---|
| 1 | Write a **new** record | The reasoning that led to the reversal is the valuable part. Editing the old record destroys it |
| 2 | Set the old record's status to `superseded by ADR-NNNN` | A reader who arrives at the old record via a stale link must be told, in the record itself, that it has been replaced and by what |
| 3 | Add `supersedes:` to the new record | The link has to be traversable in both directions, or the new record reads as though it appeared from nowhere |

Superseded documents keep their number when they move to the archive. **The number is the citation** — moving a file is a location change, not an identity change, and a citation that breaks because someone tidied a folder was never a citation.

This is the same discipline reports use for the same reason — see [Writing reports](reports.md) §*Lifecycle* — and the same one research passes use, in [Research](research.md). **The record of what was believed when is the point of keeping records at all.** A repository that silently edits its history can tell you what it thinks today, which is also what a conversation can do.

---

## 5. Merged ≠ built

> **A merged decision record is a decision, not an implementation.**

This is the single most common misreading of a docs-as-code pipeline, and it is worth stating flatly because the merge button feels like completion. It is not. The pull request that lands an accepted decision has delivered exactly one thing: a record that the decision was taken.

| Question | Answered by |
|---|---|
| *What did we choose, and why?* | the decision record — permanent, never edited |
| *Has it been built?* | the project's implementation tracker — a roadmap, a status document, or the issue tracker |
| *Where is the work?* | the branch and pull request that carry the record's number |

Two rules follow, and they are the reason the split is worth maintaining:

- **A decision record never gains an "implemented in …" line.** That would be an edit to an accepted record, which rule 4 forbids. The link runs the other way: the implementation cites the decision.
- **"The pull request is merged" is not a definition of done.** A done test names an observable event — see [Writing issues](../github/issues.md) §*Definition of done*.

Which document tracks implementation status is a project choice. Name it in your `CONTRIBUTING.md`, and name exactly one, because two trackers reporting one thing diverge and then both are wrong.

---

## References

- [`CONTRIBUTING.md` stub template](../templates/CONTRIBUTING.md) — where a project sets its own types, folders, archive location, and versioned artifacts
- [Architecture decisions](decisions.md) — the ADR bar, MADR format, numbering, and the supersession procedure in full
- [Research](research.md) — the `research` type, `researched:`, and why a pass is read-only once written
- [Writing reports](reports.md) — the `report` type, and the never-edited rule this document generalises
- [Commit messages](../git/commit-messages.md) — the commit types that drive the SemVer bump on the shipped artifact
- [Branch naming](../git/branch-naming.md) — the `<NNNN>` branch form, and why numbered records get one
- [Writing issues](../github/issues.md) — issue vs decision vs backlog row, and the definition-of-done test
- [Pull requests](../github/pull-requests.md) — what a merge does and does not establish
