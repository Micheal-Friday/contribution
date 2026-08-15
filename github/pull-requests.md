---
type: process
status: living
updated: 2026-07-29
tags: [process, contribution, pull-requests, review, github]
aliases: [PR conventions, pull request template, review norms]
---

# Pull requests

A PR is a **request for a specific kind of attention**. A 40-file docs set and a three-line fix are both PRs and share almost nothing about how they should be read — not because their subject matter differs, but because almost none of the risk in the first one is visible in the diff. This document says what a PR must contain, how to tell the reviewer what review means for this change, and when a merge is allowed to close an issue.

These rules sit under the adopting project's own `CONTRIBUTING.md`. Where the two disagree, that file wins and this one is the bug.

The worked examples come from the source project's `docs/product-definition` branch — its docs-tree commit and its prototype-removal commit, on a draft PR against that project's tracking issue — and from two earlier merged PRs. All of them are named rather than cited by SHA, because an unmerged branch has no stable one — see [commit messages](../git/commit-messages.md).

---

## Anatomy

| Part | Required | Job |
|---|---|---|
| Title | always | Conventional Commits grammar, same as the commit |
| `Summary` | always | what changed and why, in prose, for the reviewer's decision |
| `How to review this` | always | the route through the diff — **the most valuable section** |
| `Commits` | multi-commit PRs | what each commit is and why it is separate |
| `Not included` | always | what a reviewer might reasonably expect and will not find |
| `Known defects shipped knowingly` | when there are any | the defect and where it is tracked |
| `Outward-facing changes` | always, even if "none" | anything a consumer outside this repo would have to act on — see the tests below |
| `Links` | always | `Refs #N` or `Closes #N`, plus decision-record status changes |

### Title

**Same Conventional Commits grammar as the commit** — see [commit messages](../git/commit-messages.md).

```
docs(strategy): re-derive the product definition against the new operating model
fix(search): qualify the trade-records rung by market
feat(core)!: replace the record's single qualification stage with a scoped collection
```

**If the PR title is not a valid Conventional Commit, the merge commit is not one either.** `fix:` → PATCH, `feat:` → MINOR, `!` or `BREAKING CHANGE:` → MAJOR, and that mapping is what makes changelog generation automatable later. A PR title that breaks the grammar breaks it at exactly the point where a machine will read it.

| Case | Title |
|---|---|
| Single-commit PR | the commit subject, verbatim |
| Multi-commit PR | describes the whole; take the type of the **most significant** change — one `feat:` among four `docs:` commits makes the PR a `feat:` |
| Any commit is breaking | the PR carries `!` |

Scope uses the same vocabulary as commits, whatever that vocabulary currently is — [commit messages](../git/commit-messages.md) defines how scopes are generated and how the set changes. Do not invent a PR-only scope.

**Known inconsistency:** the source project's two earlier merged PRs use prose titles — *"Refine capability gate to include spec-class matching"*, *"Clarify synthesis and tone guidance"*. They predate this rule. They are not the model; the commits on the docs branch are.

### Summary

Prose. What changed and why. Two to five sentences for a small PR; two short paragraphs for a large one.

**The PR summary is for the reviewer's decision; the commit body is for the archaeologist.** The docs-tree commit's body runs fifty lines across four headed sections — that detail is correct in the commit and wrong in the PR, where it buries the review route under material the reviewer will meet again in the diff. Do not paste one into the other. Say what changed, say why now, and hand off to `How to review this`.

A `Key Changes` bullet list is acceptable but not required, and it is the part most likely to duplicate the commit body. Prefer prose; if the change genuinely has separable parts, that is what the `Commits` table is for.

---

## How to review this

The section that earns the PR its review. **The author decides what review means for this change and says so, because a reviewer given no route reviews what is easy to review** — prose style on a 40-file docs PR — **and misses the argument.**

**Derive the route from what makes *this* change expensive to review, not from what kind of file it touches.** File types come and go; the ways a change defeats a reviewer do not. There are four, and a change can carry more than one:

| Cost driver | What it does to a reviewer | So the route must give them |
|---|---|---|
| **Volume** — more diff than anyone will read | attention spreads evenly and settles on the cheapest thing in view, which is style | 3–5 entry points, the claim everything rests on, and what to skim or skip |
| **Blast radius** — the effects are not in the diff | the consequence is invisible where they are looking | the surface that moves, and evidence it still behaves |
| **Irreversibility** — being wrong is asymmetrically expensive | a decision that reads like a paragraph gets weighed like a paragraph | the alternatives, stated fairly, and what the choice forecloses |
| **Unfalsifiable assertion** — the change claims something works or can be expressed | there is nothing to disagree with, so they agree | one adversarial case the change must survive |
| *(none of the four)* | nothing — the diff **is** the review | nothing. Do not manufacture a route |

**Which change shapes recur is per-project.** Keep that lookup — shape, drivers, what to give the reviewer — in the adopting project's own `CONTRIBUTING.md`, dated and revisable, and **add a row when a shape recurs and you had to think about its route.** No decision record; a change request against the list. Two rows from the source project's version, to show the mapping: a large docs set (40 files, 7,014 insertions) carries volume and irreversibility, so its route names 3–5 entry points and the claim everything rests on; a three-line fix carries none of the four, so its route says so and stops. A research report carries unfalsifiable assertion, so its route points at the gaps section first — sources, and what the pass could not reach ([research conventions](../docs/research.md)).

**A shape that does not exist yet already has a route.** A migration on a shared database, a tool-schema change, or an auth change are not in anybody's table until they happen — but the drivers already say what their routes will need: blast radius plus irreversibility, so name the consumers and what the change forecloses.

The shape of the section, using the source project's largest change as the example:

```markdown
## How to review this

**Time:** ~30 minutes, not 3 hours. Most of the diff is bulk.
**Start here:** `docs/strategy/positioning.md` §2, then ADR-0002.
**The load-bearing claim:** because production is fully outsourced, the
pillar everyone assumes we own is somebody else's. If that is wrong, ADRs
0002–0005 all fall and the roadmap is mis-sequenced. Argue with this first.
**Argue with:** ADR-0006 — generalize on mechanism, packs for domain knowledge.
The two-column comparison is the evidence; disagree with the columns.
**Skim:** the seven benchmark reports. The findings are summarized in
`capability-map.md`; go to the reports only to check a specific claim.
**Do not review:** `docs/archive/` — verbatim copies of superseded files.
```

Four elements, each with a reason:

| Element | Reason |
|---|---|
| **Time budget** | an unbudgeted large PR gets deferred, and a deferred PR gets rubber-stamped a week later |
| **Start here** | reading order is not file order; GitHub shows you alphabetical paths |
| **The load-bearing claim** | names the thing that, if wrong, invalidates the rest — concentrates scarce attention where a mistake is expensive |
| **Do not review** | moved, generated and archived files consume review attention and yield nothing |

**Name what should be argued with, not only what should be read.** A reviewer who is told everything is up for discussion checks spelling. A reviewer told *"ADR-0006 is the contestable one"* contests ADR-0006.

---

## Linking: `Refs` vs `Closes`

| Trailer | Use when | Effect on merge |
|---|---|---|
| `Refs #N` | **any part of the issue's scope is still open** — including open decisions | no auto-close |
| `Closes #N` | this merge completes the issue's **whole** scope of work | auto-closes `#N` |

**`Closes` is a claim about the issue's entire scope of work, not about this PR's contents.** A PR that delivers everything it set out to deliver still uses `Refs` if scope items 4 and 5 are untouched. Getting this backwards auto-closes an issue with open decisions, and since a GitHub issue is the only list those decisions are on, they stop existing.

### Where each one goes

**Put `refs #N` in the commit subjects, and make the `Closes #N` decision in the PR body.** A commit is a snapshot: at the moment you write it you usually cannot know whether the branch will end up completing the issue. A PR body is editable up to the instant of merge, which is exactly when you can know.

This is derived from a real case, not invented. Both commits on that docs branch carry `Refs`:

| Commit | Trailer | Correct? |
|---|---|---|
| The docs-tree commit | `Refs #N` | **Yes** — a decision record was still `proposed` and a prototype deletion was unstaged. Scope was not complete |
| The prototype-removal commit | `Refs #N` | **Yes** — it resolved both open items, so the *scope* became complete, but the issue was not: the PR had not landed. Keeping `Refs` makes the close a deliberate act rather than a side effect of merging |

The second status comment on that issue states the consequence plainly, and this is the behaviour to copy:

> Both commits use `Refs #N` rather than `Closes #N`, so merging will not auto-close — close it manually once the PR lands, or switch the PR body to `Closes #N`.

Naming the consequence is not optional. **A PR that uses `Refs` must say who closes the issue and when.** Otherwise the issue stays open indefinitely and the tracker stops meaning anything.

### Multiple issues

One `Refs`/`Closes` line per issue. Do not use `Closes` on an issue whose scope you do not own — closing somebody else's issue from your PR destroys their open-decisions list.

---

## Size, and splitting into commits

**A PR is sized by review effort, not by diff size.** Forty files carrying one argument is one PR. Three files carrying three unrelated fixes is three PRs, because there is no single route through them and no single decision to make about them.

### Splitting within one PR

**Split where the parts will be handled differently later — reverted separately, versioned separately, or read separately.** That is the rule the axes below are instances of; a new axis earns its place by naming a fourth kind of "differently".

The model is the docs branch: the docs tree in one commit, the prototype removal in another. The issue's first status comment set the split before the commits were written:

> Suggested split: one commit for the docs tree, a separate one for whichever way the packaging decision goes.

| Split axis | Why | Example |
|---|---|---|
| **Reversibility** | things that might be reverted independently must be revertable independently | the docs tree vs. the prototype removal |
| **Type** | `docs:` and `chore:` and `feat:` drive different SemVer outcomes; a mixed commit has no correct type | the docs-tree commit `docs:`, the prototype-removal commit `chore:` |
| **Mechanical vs. meaningful** | a rename mixed with edits produces an unreadable diff | if the proposed directory move ever happens: move in one commit, re-point references in the next |
| **Schema before consumer** | the schema and its decision record land first, so nothing is written against a shape that is still being argued about | the data model lands before anything that reads it. Which change classes this binds is the adopting project's call, in its own `CONTRIBUTING.md` |

**Reversibility is the strongest axis.** It is also why a deliberately split branch should not be squash-merged; see Merge policy.

### When it should be two PRs instead

- The parts need different reviewers.
- One part is blocked and the other is not — do not hold a ready change hostage.
- There is no single `How to review this` that covers both. If you cannot write one route, there is not one PR.

---

## What a PR must state

Three things a reviewer cannot discover from the diff, plus two more that are cheap to state and expensive to omit.

| Must state | Because |
|---|---|
| **What is not included, and why** | an omission the reviewer notices and the author did not mention reads as an oversight, and review turns into a discovery exercise instead of a judgement |
| **Known defects shipped knowingly, and where they are tracked** | **a known defect shipped without a written home is an unknown defect three weeks later** |
| **Anything outward-facing** | consumers outside the repo cannot be consulted after the fact |
| **Any decision record whose status changes** | `proposed` → `accepted` is a decision being taken, and a one-line frontmatter change is invisible in a 40-file diff |
| **Whatever evidence the adopting project's own `CONTRIBUTING.md` requires for the class of artifact you touched** | in the source project that meant evals on any change to a component whose behaviour is triggered rather than called: 3–5 queries covering should-trigger, should-not-trigger, ambiguous edges, and coexistence with its neighbours |

### Not included

The docs-tree commit's body is the model:

> The directory move proposed in the packaging decision is not included here, and the deletion of the prototype is left unstaged pending a decision.

Two sentences that convert two apparent oversights into two deliberate positions. Write these even — especially — when the omission is obvious to you.

### Known defects shipped knowingly

Also from the docs-tree commit:

> Known defect, fixed in phase 0: one rung of the verification ladder is market-conditional but carries no condition, so it reports "unverified" where the truth is "not applicable in this market".

The defect is named, its mechanism is stated, and it has a home — a named roadmap phase. **A defect stated without a home is a confession, not a plan.** Acceptable homes: a known-defect inventory, a named roadmap phase, or an open issue. "We'll get to it" is not one.

### Outward-facing changes

State these explicitly, or state "none".

**A change is outward-facing when somebody outside this repository would have to do something about it, and cannot be consulted first.** Three tests generate the list; any one is enough:

| Test | Because |
|---|---|
| **Named from outside** — someone else has written this identifier down | you cannot edit their copy. Some identifiers are immutable once anyone has one |
| **On a resolution path** — install, import, routing, lookup or query passes through it | changing it does not fail loudly; it fails at whoever's machine resolves next |
| **Drawn from a shared budget or contract** — capacity or shape that other things also depend on | taking more leaves less, silently, for something you are not looking at |

**What the tests yield is per-project, and it changes with the packaging.** Keep that list in the adopting project's own `CONTRIBUTING.md`, dated and revisable. It typically catches a published package name (immutable once anyone has installed it), a path that installs or imports resolve through, a version that consumers pin, and anything drawn from a shared budget — a startup budget, a rate limit, a namespace — where growing one thing's share silently shrinks another's. **The rows are a consequence of the packaging a project has now; the three tests do not change with it.**

Which is why the tests are worth applying to packaging that does not exist yet: a versioned workspace makes file names a migration rather than an edit, a served interface makes its schema and its auth outward-facing, and a shared database makes consumers *concurrent*, so a change needs a migration path rather than a mention.

**Do not wait for the row to exist before applying the tests.** The row is written after somebody outside is already affected, which is one PR too late.

---

## Review norms

### "Yes, if"

The norm this document adopts: *"Reviewers state conditions for approval rather than blocking outright where that is possible — 'yes, if' beats 'no'."*

**A verdict must say what would satisfy the reviewer and who acts next.** Those two facts — is approval conditional, and can the author check the conditions alone — generate three states and only three. There is no fourth because there is no fourth pair of answers: conditions the author cannot check are not conditions, they are a second review.

| Verdict | Means | Use when |
|---|---|---|
| **Yes** | merge it | no conditions |
| **Yes, if** | I approve subject to these named conditions; the author applies them and merges without a second round | the conditions are checkable by the author alone — **this should be the common case** |
| **No, because** | the change is wrong in **direction**, not in detail | reserved. A "no" on detail is a "yes, if" that was not thought through |

Two reasons, and the second is the one that actually changes reviewer behaviour:

1. It takes a round-trip off the critical path. A "yes, if" with three named conditions merges the same day; a bare "changes requested" merges when both people are next awake at the same time.
2. **It forces the reviewer to name what would satisfy them.** A blocking review that does not state its conditions is unanswerable, and unanswerable reviews are how PRs die quietly.

Conditions must be **checkable and finite**. *"Yes, if the claim in §4 is qualified by market"* is a condition. *"Yes, if it's clearer"* is a mood.

### The three-day hold

Hold anything **cross-cutting** open for at least three business days.

**Cross-cutting means the change becomes a premise for later work, so undoing it later costs more than the change itself.** What qualifies is per-project and belongs in the adopting project's own `CONTRIBUTING.md`; in the source project the list named a positioning boundary, the shape of persisted data, a change touching more than one component, adding or accepting a decision record, and anything outward-facing per the tests above. Everything else can merge as soon as it is reviewed. The list grows as the product does — anything touching shared state, once state is shared, is cross-cutting by construction, because other people's work is already resting on it. Add a bullet by change request.

**Say the honest thing about this:** with one maintainer the three-day hold is a self-imposed cooling-off period rather than a window for other people to object. It is still worth keeping. A decision you still agree with three days later is a materially different artifact from one merged in the hour it was written, and the decision record is the thing a project is least able to undo.

### Author ≠ reviewer

*Nobody reviews their own work* — and the adopting project's own `CONTRIBUTING.md` says which classes of change it binds absolutely. When no second person is available, the substitutes are the self-review pass below and the three-day hold — and **the PR must say which it got**, so a later reader can weigh the review the change actually received rather than assuming one it did not.

---

## The self-review pass

Before requesting review, read your own PR as the reviewer. On GitHub, not in your editor — different rendering, and you see the whitespace, the rendered tables, and the broken links that your editor resolves for you.

**A check belongs on this list when its failure is silent — nothing errors, nothing fails, and the cost lands on somebody later.** That is why the list is what it is, and it is also the retirement rule: **when a machine can perform a check, delete the row.** A manual check that tooling already does trains people to skim the list, which costs the checks that only a human can run.

| Check | Why |
|---|---|
| Every new document has frontmatter with at least `type`, `status`, `updated` | [document lifecycle](../docs/document-lifecycle.md) |
| Every `References` section and link list uses **relative markdown links with real paths**, and every path resolves | GitHub renders `[[wikilink]]` as literal text and does not link it. Relative markdown links work on GitHub *and* in an editor. Never emit `[[foo]](path)` — it is malformed in both |
| Wikilinks appear only inline in prose, where they degrade to readable text | inline they cost a reader nothing; in a list of links they are the whole point of the list, and they fail **silently** |
| Dates are ISO-8601 everywhere, including inside prose; relative dates converted | "last week" is unreadable in six months |
| No `proposed` decision described in prose as though it were decided | `proposed` is not a soft `accepted` ([decision records](../docs/decisions.md)) |
| Indexes updated in the same PR: the docs index, the section index, the decision index, `CHANGELOG.md` | the prototype-removal commit did this. **An index that lags is worse than no index**, because it is trusted |
| Filenames kebab-case; numbered series sequential, zero-padded, never reused | [document lifecycle](../docs/document-lifecycle.md) |
| Every identifier that must match something else still matches it | the mismatch resolves to nothing, silently, on somebody else's machine |
| The leak test on any core claim: *could this be false in the other market?* | it has already caught one shipped defect |
| **Nothing that is part of this change is left unstaged** | *"a deletion sitting unstaged in the working tree is the only state that loses information"* — the intent is recorded nowhere. Commit it or revert it; do not leave it ambiguous |
| The `How to review this` route actually works when followed | if you cannot follow your own route, nobody can |

---

## Merge policy

| | Rule | Reason |
|---|---|---|
| Method | **Merge commit** or **rebase-merge** when the branch's commits were split deliberately; squash only when the intermediate commits are noise ("wip", "typo") | squashing the docs-tree and prototype-removal commits together would destroy the reversibility split that was the reason for splitting them |
| History | never force-push or rewrite a branch after review has started | the review comments detach from the code and the thread becomes unreadable |
| Branch | delete after merge | the branch name is not history — see [branch naming](../git/branch-naming.md) |
| Issue | close manually if the commits used `Refs`, or edit the PR body to `Closes #N` before merging | see Linking, above |
| Merged ≠ built | a merged decision record records a decision, not an implementation | implementation status lives wherever the project tracks it — a current-state inventory, a roadmap — see [document lifecycle](../docs/document-lifecycle.md) |

### Draft PRs

**Open a draft as soon as the branch is pushed, not when the work is finished.** Where the PR thread is the project's discussion venue, a discussion that opens at the end of the work is a review, not a discussion. A draft costs nothing and gives findings somewhere to land while they are still cheap to act on.

A draft is expected to be incomplete. It is not exempt from stating what it does not yet include.

### When the PR outgrows its issue

If the PR delivers something outside the issue's `Scope of work`, pick one — and pick it explicitly:

| Option | When |
|---|---|
| **Trim the PR** back to scope | the extra work is separable and not urgent |
| **Amend the issue** with a new numbered scope item, plus a comment saying why it was added rather than opened separately | the extra work genuinely belongs to the same unit of work |
| **Open a second issue** and add a second `Refs #M` | the extra work has its own definition of done |

**Do not let the PR silently redefine the issue.** The issue is what every status comment reports against; scope that moves without a note makes every earlier comment retroactively wrong. Appending a numbered item is cheap; renumbering or rewriting the scope is not — see [writing issues](issues.md) and [status updates](issue-updates.md).

---

## Template

```markdown
<!-- Title: Conventional Commits grammar, same as the commit.
     docs(strategy): re-derive the product definition against the new model -->

## Summary

What changed and why, in prose. Two to five sentences. The commit bodies
carry the detail — do not restate them here.

## How to review this

<!-- Include only the lines this change's cost drivers earn — volume, blast
     radius, irreversibility, unfalsifiable assertion. A three-line fix earns
     none of them; say so and stop. -->

**Time:** ~N minutes.
**Start here:** `path/to/file.md` §N.
**The load-bearing claim:** <the one thing that, if wrong, invalidates the rest>.
**Argue with:** <what genuinely needs a second opinion>.
**Skim:** <what is bulk, and where its findings are summarized>.
**Do not review:** <moved, generated, archived, or verbatim files>.

## Commits

| Commit | Contents | Why separate |
|---|---|---|
| commit subject | ... | reversibility / type / mechanical |

## Not included

- <what a reviewer might expect and will not find, and why>

## Known defects shipped knowingly

- <defect, and its mechanism> — tracked in <roadmap phase / known-defect entry / issue #N>

## Outward-facing changes

- <anything named from outside, on a resolution path, or drawn from a shared
  budget. What that catches in this project is listed in its CONTRIBUTING.md.
  Or "none".>

## Evidence

<Whatever this project requires for the class of artifact touched — evals,
benchmarks, a migration dry-run. Otherwise "n/a".>

## Review received

<Second reviewer, or: self-review pass + three-day hold. Say which.>

## Links

Refs #N
<!-- Closes #N only if this merge completes the issue's WHOLE scope of work,
     including any open decisions. If using Refs, say who closes the issue. -->

Decision status changes in this PR: ADR-NNNN `proposed` → `accepted`
```

---

## References

Relative markdown links, not wikilinks: this repo is read on GitHub, which renders `[[foo]]` as literal text.

- [Document lifecycle](../docs/document-lifecycle.md) — frontmatter, naming, the SemVer boundary, and *merged ≠ built*.
- [Decision records](../docs/decisions.md) — status semantics; why a status change deserves its own PR line.
- [Research conventions](../docs/research.md) — *state what the pass could not reach*, which is what a research PR's review route points at first.
- [Writing reports](../docs/reports.md) — the one artifact class that leaves the repository, and what that costs its review.
- [Writing issues](issues.md) — issue anatomy, scope numbering, and definition of done.
- [Status updates on issues](issue-updates.md) — the status comment that should precede the PR.
- [Commit messages](../git/commit-messages.md) — commit grammar, bodies, and the `refs #N` subject reference.
- [Branch naming](../git/branch-naming.md) — branch prefixes and post-merge deletion.
- The adopting project's own `CONTRIBUTING.md` — the recurring change shapes and their routes, what the outward-facing tests catch here, what counts as cross-cutting, and the evidence each class of change owes.
