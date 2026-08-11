---
type: process
status: living
version: 2.0
updated: 2026-07-29
tags: [contribution, git, branches, process]
aliases: [branch naming, branch conventions]
---

# Branch naming

Detail behind the one line about branches in the adopting project's own `CONTRIBUTING.md`. The convention exists for one reason: **a branch name outlives the branch.** Merges land as merge commits, so the name is embedded in the permanent history — `Merge pull request #6 from <owner>/skill/inquiry` is in the source project's log forever, and `skill/inquiry` does not say what it did.

---

## 1. The pattern

```
<type>/<short-kebab-description>
<type>/<NNNN>-<kebab-title>          when the work is tied to a numbered record
```

| Rule | Reason |
|---|---|
| Lowercase ASCII, `kebab-case`, no spaces, no `&` | The same rule that governs file names — [document lifecycle](../docs/document-lifecycle.md) §*Naming*. Branch names are typed into shells and pasted into CI configs; a capital or a space is a bug waiting for a case-insensitive filesystem |
| Exactly one `/` | Git refs are a directory tree. `feat/vendor/compare` cannot coexist with a branch named `feat/vendor` — git refuses to create a file and a directory at the same path |
| **Description: 2–5 words, ~40 characters** | It is read in `git branch`, in PR lists, in the merge commit subject, and in CI log headers. All four truncate |
| Name the outcome, not the activity | `feat/vendor-compare` beats `feat/add-compare-skill` — the type already said "add" |

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

The rule is packaging-invariant by construction: it ranges over the commit vocabulary and the repo's record series, neither of which changes when the project is repackaged — one distributable today, a service or an application later. Commit *scopes* do change with the packaging — a second reason they stay out of branch names.

### Current values — as of 2026-07-29, revisable

Seven types. A branch may carry commits of more than one — pick the type matching the *point* of the branch. The first five are the commit vocabulary and are the same in any project; the last two depend on which record series the adopting project keeps, so its own `CONTRIBUTING.md` is what settles them. Worked examples of each are in §7.

| Type | Source | For |
|---|---|---|
| `docs/` | commit type | anything under `docs/`, plus root `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md` |
| `feat/` | commit type | new capability in the released artifact: a new component, a new field it writes, a new output contract |
| `fix/` | commit type | a defect in shipped behaviour |
| `refactor/` | commit type | structure changes, behaviour does not |
| `chore/` | commit type | repo maintenance that is none of the above: deletions, manifest bumps, index and changelog sweeps |
| `adr/` | record series — the decision records | writing, superseding, or resolving a decision record |
| `rfd/` | **provisional — no series exists** | a proposal put up to be argued with, before it is a decision |

Two notes on the edges:

- **`chore/` is not a bin for "everything else."** Deletions in particular belong here and belong visibly — the source project's layout decision records the cost of leaving one ambiguous: *a deletion sitting unstaged in the working tree is the only state that loses information.*
- **`rfd/` is the one value the rule does not fully generate.** There is no record series for requests-for-discussion, so it has a name and no numbers. Most exploratory writing belongs in a decision record with `status: proposed` instead — see [decision records](../docs/decisions.md). Use `rfd/` only when the writing is not yet shaped like a decision.

`skill/` is **not** a type, despite appearing once in the source project's history — it fails the rule as an area. `skill/inquiry` should have been `feat/vendor-inquiry-tone-modes`.

### How the list changes

| Event | Procedure |
|---|---|
| A commit type is added or retired in [commit-messages.md](commit-messages.md) §2 | This list follows in the same PR. No separate decision — that is the point of sourcing it there |
| A new numbered record series is created under `docs/` | It gains a branch type and the `<NNNN>` form in the PR that creates the series. This is what would resolve `rfd/` |
| Anything else proposed as a type | Rejected by the rule. Neither a commit type nor a record series means it is an area, and areas belong in the description |

Retired types are dropped from this table and **left alone in history.** `skill/inquiry` stays in the merge commit for PR #6 forever — a fact about the log, not a defect to rewrite.

---

## 3. Numbered branches

**The `<NNNN>` form is available exactly when a numbered record series exists to draw the number from** — the second source in §2, which in most projects means decision records only.

| Case | Form | Example |
|---|---|---|
| Writing the ADR | `adr/<NNNN>-<title>` | `adr/0011-bundled-mcp-server` |
| Implementing exactly one ADR | `<type>/<NNNN>-<short-title>` | `feat/0010-scoped-qualification` |
| Implementing several, or none | plain description | `docs/product-definition` |

Four digits, zero-padded, **matching the ADR file name exactly** — the same naming rule that governs every record — [document lifecycle](../docs/document-lifecycle.md) §*Naming*, and [decision records](../docs/decisions.md) §3 on why a number is never reused. Reserve the number first, so the branch, the file, and the PR all agree before anyone else picks the same one. Numbers are never reused, including for withdrawn records, so a branch that dies still burns its number.

The payoff is one command:

```bash
git branch -a --list '*0010*'
```

**Everything that touched a decision is findable from its number** — the ADR branch, the implementation branch, and (via the merge commit subject) the merged history, long after both branches are deleted.

---

## 4. When a branch is warranted

**The rule: a branch exists when a second person needs to see the change before it lands** — not when the diff is large. A 40-file docs tree and a one-line frontmatter correction sit on opposite sides of that line.

**Branch and PR is the norm.** In the source project a layout decision made it a driver: strategy, decisions, research, and code all ship through the same branch → PR → review pipeline, so a skill change and the reference page describing it get reviewed together.

The table is the rule applied, not a separate list; a situation not in it is decided by the rule.

| Situation | Branch? | Reason |
|---|---|---|
| Any change to a shipped skill definition | Yes | The adopting project's own `CONTRIBUTING.md` governs this — typically evals and a reviewer who is not the author. Both need a PR to happen in |
| Any new or superseding ADR | Yes | The PR thread *is* the discussion. Cross-cutting ones are held open three business days — see [pull requests](../github/pull-requests.md) |
| A new or substantially revised document | Yes | Review is the point |
| Fixing a typo, a broken link, a wrong date in your own draft | No | Commit to `main`. A PR for a one-character fix costs a reviewer's attention and buys nothing |
| Updating `CHANGELOG.md` or an index to match a merge that already happened | No | It is bookkeeping for a decision already reviewed |
| Anything you are unsure about | Yes | The cost of an unnecessary branch is one extra click; the cost of an unreviewed skill change is a routing regression across every skill that routes to it |

---

## 5. Lifetime and hygiene

| Rule | Reason |
|---|---|
| **Short-lived — days, not weeks** | A branch is an unmerged claim about the repo. The longer it lives, the more of `main` it has not seen |
| Rebase onto `main` freely **until review starts** | Keeps the diff honest — a stale branch reviews against a repo that no longer exists |
| **Never rebase after review has started.** Merge `main` in instead | Rebasing rewrites the SHAs the reviewer's comments are anchored to. The comments survive; their context does not |
| Land via merge commit, not squash | The merge commit subject preserves the branch name, and the individual commits stay individually revertible — see [commit messages](commit-messages.md) §9 on why the docs tree and the prototype deletion were kept apart |
| **Delete the branch after merge** | The merge commit already records the name. A merged branch still listed in `git branch -a` reads as work in progress |
| Do not rename a branch with a PR under review | The old URL is in people's tabs and notifications. Fix the PR title instead; the branch name is already fossilised in the eventual merge commit either way |

**When a branch outlives its issue,** it has grown a second scope. That is the diagnosis, not bad luck. Do this:

1. Split the finished part into its own PR and merge it. The commit subject keeps `refs #N` — see [commit messages](commit-messages.md) §6 — because the issue is not done.
2. Rebase the remainder onto `main` under a new, accurately named branch.
3. Close the old PR pointing at the new one, and open an issue for the residue if it is not going to be picked up this week.

The failure mode this prevents is real and is in the source project's history: the docs-tree commit on `docs/product-definition` shipped with the note *"the deletion of `Supplier Management App/` is left unstaged pending a decision."* The branch was carrying an undecided item alongside forty decided files. It was resolved in the following PR as its own prototype-removal commit rather than smuggled into the big one.

---

## 6. Branches, issues, and ADRs

Three links, each made exactly once, in the place that survives longest:

| Link | Made by | Survives |
|---|---|---|
| Branch → issue | the commit subject: `refs #7` / `closes #4` | forever, in the git object store |
| Branch → ADR | the `<NNNN>` in the branch name, and `per ADR-NNNN` in the commit body | forever |
| Branch → review | the PR | until the tab is closed |

**Issue numbers do not go in branch names.** The forge cross-references an issue from commit trailers and from the PR, never from a ref name — so a number in the branch name is a second, unlinked copy of a fact that is already recorded, and it goes stale when scope moves between issues.

ADR numbers *do* go in branch names, because ADRs have no equivalent automatic backlink.

**The link from an ADR to its implementation runs the other way — and it is not a branch link.** [Decision records](../docs/decisions.md) forbid editing an accepted decision, so an ADR never gains an "implemented in `feat/...`" line. The content conventions state the same thing from the other side: **merged ≠ built.** Implementation status lives in the roadmap and current-state documents. The branch number makes the work *findable* from the decision; the roadmap makes it *tracked*.

---

## 7. Good and bad

Good — the examples are from the source project, with its own names:

| Branch | Why it works |
|---|---|
| `docs/product-definition` | Names the outcome. Reads correctly in the merge commit two years from now |
| `feat/vendor-compare` | Type + the exact skill that will exist afterwards |
| `fix/vendor-search-customs-rung` | Names the skill and the defect. Greppable against the defect list |
| `refactor/vendor-inquiry-to-vendor-rfx` | A rename of a shipped skill; behaviour does not change, and the type says so |
| `adr/0011-bundled-mcp-server` | Number matches the file; the ADR and the branch cannot drift |
| `chore/remove-supplier-quest` | A deletion, typed as a deletion, visible in the branch list |
| `feat/0010-scoped-qualification` | Implementation branch findable from the decision it implements |

Bad:

| Branch | Why it fails |
|---|---|
| `skill/inquiry` | `skill` is not a type. It says where, never what — and it is now permanent in the merge commit for PR #6 |
| `user-patch-1` | The forge's default. Names the author and a counter; the author is already in `git log` and the counter means nothing |
| `docs/Product-Definition` | Title Case. Breaks the ASCII-lowercase-kebab rule that governs every other name in the repo, and burns on case-insensitive checkouts |
| `feature/add-comparison-skill-for-quotes` | Two faults: `feature` is not the type (`feat` is, matching Conventional Commits), and `add-` restates what `feat` already said |
| `fix/bug` | No referent. Which defect? The project's defect list names five |
| `adr/0009-repo-and-plugin-layout-decline-plugin-move-and-withdraw-prototypes` | 74 characters. Truncated in every list that displays it. `adr/0009-layout-resolution` carries the same information |
| `docs/update-docs` | The type and the description say the same word. Zero information |
| `dev`, `wip`, `temp`, `wip-branch` | No type, no scope, no end condition. Nothing tells you when they are done, so they never are |

---

## References

- The adopting project's own `CONTRIBUTING.md` — the naming and branching lines this details, and the rules for changing a skill
- [Document lifecycle](../docs/document-lifecycle.md) — naming, numbered series, and the lifecycle the `<NNNN>` form in §3 draws on
- [Decision records](../docs/decisions.md) — ADR numbering, the bar for writing one, and why an accepted record is never edited
- [Commit messages](commit-messages.md) §2 — the type vocabulary §2 here is generated from; also scopes and the `refs` / `closes` rule
- [Pull requests](../github/pull-requests.md) — what happens to the branch once it is pushed
- [Writing issues](../github/issues.md) · [Status updates](../github/issue-updates.md) — where the numbers in the footers come from
- The repository index — the three-layer contract every list in these guides is written to
