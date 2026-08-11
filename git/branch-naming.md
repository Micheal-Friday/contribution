---
type: process
status: living
version: 1.2
updated: 2026-07-29
tags: [contribution, git, branches, process]
aliases: [branch naming, branch conventions]
---

# Branch naming

Detail for the one line in [`CONTRIBUTING.md`](../../CONTRIBUTING.md) §*Branches and commits*. The convention exists for one reason: **a branch name outlives the branch.** Merges land as merge commits, so the name is embedded in the permanent history — `Merge pull request #6 from Micheal-Friday/skill/inquiry` is in this repo's log forever, and `skill/inquiry` does not say what it did.

---

## 1. The pattern

```
<type>/<short-kebab-description>
<type>/<NNNN>-<kebab-title>          when the work is tied to a numbered record
```

| Rule | Reason |
|---|---|
| Lowercase ASCII, `kebab-case`, no spaces, no `&` | Same rule as file names in `CONTRIBUTING.md` §*Naming*. Branch names are typed into shells and pasted into CI configs; a capital or a space is a bug waiting for a case-insensitive filesystem |
| Exactly one `/` | Git refs are a directory tree. `feat/trace/compare` cannot coexist with a branch named `feat/trace` — git refuses to create a file and a directory at the same path |
| **Description: 2–5 words, ~40 characters** | It is read in `git branch`, in PR lists, in the merge commit subject, and in CI log headers. All four truncate |
| Name the outcome, not the activity | `feat/trace-compare` beats `feat/add-compare-skill` — the type already said "add" |

**The description names the thing that will be different afterwards.** If you cannot name it in five words, you probably have two branches.

---

## 2. Types

### The rule

**A branch type names the kind of outcome the branch produces — and the vocabulary is generated, not chosen.** It has exactly two sources, and nothing else qualifies:

| Source | Why it is a source |
|---|---|
| **The commit types** — [commit-messages.md](commit-messages.md) §2 | The branch and its commits describe the same work at two grains. Two vocabularies for one thing drift, and then `git branch` and `git log` disagree about what happened |
| **One per numbered record series in the repo** | A branch producing a *record* rather than a change has no commit type to borrow — its commits are `docs:`, which does not say which record. The series name fills that gap and brings the `<NNNN>` form with it (§3) |

In particular **a type is never an area.** `skill/`, `frontend/`, `api/` name *where*, which the description already does.

The rule is packaging-invariant by construction: it ranges over the commit vocabulary and the repo's record series, neither of which changes as TRACE climbs the migration ladder in [`skill-architecture.md`](../30-architecture/skill-architecture.md) §7. Commit *scopes* do change with the packaging — a second reason they stay out of branch names.

### Current values — as of 2026-07-29, revisable

Seven types. A branch may carry commits of more than one — pick the type matching the *point* of the branch.

| Type | Source | For | Example from this repo |
|---|---|---|---|
| `docs/` | commit type | anything under `docs/`, plus root `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md` | `docs/product-definition` — the branch that carried the seven-section docs tree |
| `feat/` | commit type | new capability in the released artifact: a new skill, a new field a skill writes, a new output contract | `feat/trace-compare` — the evaluate-stage skill in [[skill-architecture]] §5 |
| `fix/` | commit type | a defect in shipped behaviour | `fix/trace-search-customs-rung` — the market-conditional customs-data rung in [[current-state]] §5 |
| `refactor/` | commit type | structure changes, behaviour does not | `refactor/trace-inquiry-to-trace-rfx` — the planned upgrade in [[skill-architecture]] §5 |
| `chore/` | commit type | repo maintenance that is none of the above: deletions, manifest bumps, index and changelog sweeps | `chore/remove-supplier-quest` — the deletion of the prototype |
| `adr/` | record series — `docs/40-decisions/` | writing, superseding, or resolving a decision record | `adr/0011-bundled-mcp-server` — rung 2 of the migration ladder |
| `rfd/` | **provisional — no series exists** | a proposal put up to be argued with, before it is a decision | `rfd/second-pack-for-china-electronics` |

Two notes on the edges:

- **`chore/` is not a bin for "everything else."** Deletions in particular belong here and belong visibly — [[0009-repo-and-plugin-layout]] §*Prototypes* records the cost of leaving one ambiguous: *a deletion sitting unstaged in the working tree is the only state that loses information.*
- **`rfd/` is the one value the rule does not fully generate.** There is no `docs/` series for requests-for-discussion, so it has a name and no numbers. Most exploratory writing belongs in an ADR with `status: proposed` instead — [`40-decisions/README.md`](../40-decisions/README.md) rule 4 exists for exactly that. Use `rfd/` only when the writing is not yet shaped like a decision.

`skill/` is **not** a type, despite appearing once in the history — it fails the rule as an area. `skill/inquiry` should have been `feat/trace-inquiry-tone-modes`.

### How the list changes

| Event | Procedure |
|---|---|
| A commit type is added or retired in [commit-messages.md](commit-messages.md) §2 | This list follows in the same PR. No separate decision — that is the point of sourcing it there |
| A new numbered record series is created under `docs/` | It gains a branch type and the `<NNNN>` form in the PR that creates the series. This is what would resolve `rfd/` |
| Anything else proposed as a type | Rejected by the rule. Neither a commit type nor a record series means it is an area, and areas belong in the description |

Retired types are dropped from this table and **left alone in history.** `skill/inquiry` stays in the merge commit for PR #6 forever — a fact about the log, not a defect to rewrite.

---

## 3. Numbered branches

**The `<NNNN>` form is available exactly when a numbered record series exists to draw the number from** — the second source in §2, and today that means ADRs only.

| Case | Form | Example |
|---|---|---|
| Writing the ADR | `adr/<NNNN>-<title>` | `adr/0011-bundled-mcp-server` |
| Implementing exactly one ADR | `<type>/<NNNN>-<short-title>` | `feat/0010-scoped-qualification` |
| Implementing several, or none | plain description | `docs/product-definition` |

Four digits, zero-padded, **matching the ADR file name exactly** — same rule as `CONTRIBUTING.md` §*Naming*. Reserve the number first (`adr new "Title"`), so the branch, the file, and the PR all agree before anyone else picks the same one. Numbers are never reused, including for withdrawn records, so a branch that dies still burns its number.

The payoff is one command:

```bash
git branch -a --list '*0010*'
```

**Everything that touched a decision is findable from its number** — the ADR branch, the implementation branch, and (via the merge commit subject) the merged history, long after both branches are deleted.

---

## 4. When a branch is warranted

**The rule: a branch exists when a second person needs to see the change before it lands** — not when the diff is large. The 40-file docs tree and a one-line frontmatter correction sit on opposite sides of that line.

**Branch and PR is the norm here.** The history shows it — `Merge pull request #6 from Micheal-Friday/skill/inquiry` — and [[0009-repo-and-plugin-layout]] made it a decision driver: strategy, decisions, research, and code all ship through the same branch → PR → review pipeline, so a skill change and the reference page describing it get reviewed together.

The table is the rule applied, not a separate list; a situation not in it is decided by the rule.

| Situation | Branch? | Reason |
|---|---|---|
| Any change to a `SKILL.md` | Yes | `CONTRIBUTING.md` §*Changing a skill* requires evals and a reviewer who is not the author. Both need a PR to happen in |
| Any new or superseding ADR | Yes | The PR thread *is* the discussion. Cross-cutting ones are held open three business days |
| A new or substantially revised document | Yes | Review is the point |
| Fixing a typo, a broken wikilink, a wrong date in your own draft | No | Commit to `main`. A PR for a one-character fix costs a reviewer's attention and buys nothing |
| Updating `CHANGELOG.md` or an index to match a merge that already happened | No | It is bookkeeping for a decision already reviewed |
| Anything you are unsure about | Yes | The cost of an unnecessary branch is one extra click; the cost of an unreviewed skill change is a routing regression across nine skills |

---

## 5. Lifetime and hygiene

| Rule | Reason |
|---|---|
| **Short-lived — days, not weeks** | A branch is an unmerged claim about the repo. The longer it lives, the more of `main` it has not seen |
| Rebase onto `main` freely **until review starts** | Keeps the diff honest — a stale branch reviews against a repo that no longer exists |
| **Never rebase after review has started.** Merge `main` in instead | Rebasing rewrites the SHAs the reviewer's comments are anchored to. The comments survive; their context does not |
| Land via merge commit, not squash | The merge commit subject preserves the branch name, and the individual commits stay individually revertible — see [[commit-messages]] §9 on why the docs tree and the prototype deletion were kept apart |
| **Delete the branch after merge** | The merge commit already records the name. A merged branch still listed in `git branch -a` reads as work in progress |
| Do not rename a branch with a PR under review | The old URL is in people's tabs and notifications. Fix the PR title instead; the branch name is already fossilised in the eventual merge commit either way |

**When a branch outlives its issue,** it has grown a second scope. That is the diagnosis, not bad luck. Do this:

1. Split the finished part into its own PR and merge it. The commit subject keeps `refs #N` — see [commit messages](commit-messages.md) §6 — because the issue is not done.
2. Rebase the remainder onto `main` under a new, accurately named branch.
3. Close the old PR pointing at the new one, and open an issue for the residue if it is not going to be picked up this week.

The failure mode this prevents is real and is in this history: the docs-tree commit on `docs/product-definition` shipped with the note *"the deletion of `Supplier Management App/` is left unstaged pending a decision."* The branch was carrying an undecided item alongside forty decided files. It was resolved in PR #8 as its own prototype-removal commit rather than smuggled into the big one.

---

## 6. Branches, issues, and ADRs

Three links, each made exactly once, in the place that survives longest:

| Link | Made by | Survives |
|---|---|---|
| Branch → issue | the commit subject: `refs #7` / `closes #4` | forever, in the git object store |
| Branch → ADR | the `<NNNN>` in the branch name, and `per ADR-NNNN` in the commit body | forever |
| Branch → review | the PR | until the tab is closed |

**Issue numbers do not go in branch names.** GitHub cross-references an issue from commit trailers and from the PR, never from a ref name — so a number in the branch name is a second, unlinked copy of a fact that is already recorded, and it goes stale when scope moves between issues.

ADR numbers *do* go in branch names, because ADRs have no equivalent automatic backlink.

**The link from an ADR to its implementation runs the other way — and it is not a branch link.** [`40-decisions/README.md`](../40-decisions/README.md) rule 2 forbids editing an accepted decision, so an ADR never gains a "implemented in `feat/...`" line. `CONTRIBUTING.md` states the same thing from the other side: **merged ≠ built.** Implementation status lives in [[roadmap]] and [[current-state]]. The branch number makes the work *findable* from the decision; the roadmap makes it *tracked*.

---

## 7. Good and bad

Good:

| Branch | Why it works |
|---|---|
| `docs/product-definition` | Names the outcome. Reads correctly in the merge commit two years from now |
| `feat/trace-compare` | Type + the exact skill that will exist afterwards |
| `fix/trace-search-customs-rung` | Names the skill and the defect. Greppable against [[current-state]] §5 |
| `adr/0011-bundled-mcp-server` | Number matches the file; the ADR and the branch cannot drift |
| `chore/remove-supplier-quest` | A deletion, typed as a deletion, visible in the branch list |
| `feat/0010-scoped-qualification` | Implementation branch findable from the decision it implements |

Bad:

| Branch | Why it fails |
|---|---|
| `skill/inquiry` | `skill` is not a type. It says where, never what — and it is now permanent in the merge commit for PR #6 |
| `Micheal-Friday-patch-1` | GitHub's default. Names the author and a counter; the author is already in `git log` and the counter means nothing |
| `docs/Product-Definition` | Title Case. Breaks the ASCII-lowercase-kebab rule that governs every other name in the repo, and burns on case-insensitive checkouts |
| `feature/add-comparison-skill-for-quotes` | Two faults: `feature` is not the type (`feat` is, matching Conventional Commits), and `add-` restates what `feat` already said |
| `fix/bug` | No referent. Which defect? [[current-state]] §5 lists five |
| `adr/0009-repo-and-plugin-layout-decline-plugin-move-and-withdraw-prototypes` | 74 characters. Truncated in every list that displays it. `adr/0009-layout-resolution` carries the same information |
| `docs/update-docs` | The type and the description say the same word. Zero information |
| `dev`, `wip`, `temp`, `karun-working` | No type, no scope, no end condition. Nothing tells you when they are done, so they never are |

---

## References

- [`CONTRIBUTING.md`](../../CONTRIBUTING.md) §*Naming*, §*Branches and commits* — the conventions this details
- [Contribution index](README.md) — the three-layer contract every list in this folder is written to
- [Commit messages](commit-messages.md) §2 — the type vocabulary §2 here is generated from; also scopes and the `Refs` / `Closes` rule
- [Pull requests](pull-requests.md) — what happens to the branch once it is pushed
- [Writing issues](issues.md) · [Status updates](issue-updates.md) — where the numbers in the footers come from
- [ADR-0009 — Repo and plugin layout](../40-decisions/0009-repo-and-plugin-layout.md) — the docs-as-code pipeline, and the unstaged-deletion lesson
- [ADR-0001 — Record architecture decisions](../40-decisions/0001-record-architecture-decisions.md) · [`40-decisions/README.md`](../40-decisions/README.md) — ADR numbering, and why an accepted record is never edited
- [`skill-architecture.md`](../30-architecture/skill-architecture.md) §5 — the nine-skill inventory the `feat/` and `refactor/` examples come from; §7 — the migration ladder the type rule must survive
- [`current-state.md`](../30-architecture/current-state.md) §5 — the known defects the `fix/` examples come from
