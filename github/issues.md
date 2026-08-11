---
type: process
status: living
version: 1.1
updated: 2026-07-29
tags: [process, contribution, issues, github]
aliases: [issue conventions, writing issues, issue anatomy]
---

# Writing issues

An issue is a **unit of work**: something a person will do, with a finish line somebody else can check. This document says when to open one instead of writing an ADR or a backlog row, what goes in it, and how it should be shaped so that the status comments written against it later — see [[issue-updates]] — have something to report against.

The reference example throughout is [#7](https://github.com/Micheal-Friday/Trace/issues/7), *"Revise TRACE product definition, feature set, and roadmap against the Pargar ODM model"*. It is the largest issue this repo has run and the only one with a full reporting cycle. Where this document says "like #7", go and read it.

Extends [`CONTRIBUTING.md`](../../CONTRIBUTING.md). Where the two disagree, `CONTRIBUTING.md` wins and this file is the bug.

---

## Issue, ADR, or backlog row

**Assign a piece of work by the question it answers, not by the tool you happen to have open.** Three questions, and they do not overlap:

| The question | The thing it produces | Lifecycle | Ends by |
|---|---|---|---|
| *Who is doing what, and when is it finished?* | a **unit of work** | open → closed | being delivered |
| *What did we choose, and why?* | a **decision** | `proposed` → `accepted` \| `rejected` → `superseded` | being superseded — never edited, never deleted |
| *What do we want, and in what order?* | a **ranked intention** | a verdict and a priority, re-triaged | being promoted to a unit of work, or re-triaged |

The three are confused constantly because a single piece of thinking usually produces all three, and producing all three is not duplication — it is the point. #7 was one unit of work; it produced ten ADRs and a re-triaged backlog of 21 items. Nothing was written twice: the issue tracked the doing, the ADRs recorded the choosing, and the backlog recorded the ordering.

Each question implies its own storage. A unit of work needs a finish line somebody else can check. A decision needs to be findable by whoever hits the constraint next, which means durable, linkable, and never rewritten. A ranked intention needs to sit next to its competitors, because the rank *is* the content.

### Where each lives — current, 2026-07-29

| Produces | Venue today | Owner |
|---|---|---|
| Unit of work | a GitHub issue | an assignee |
| Decision | [`docs/40-decisions/`](../40-decisions/README.md) | the decision record |
| Ranked intention | [`docs/20-product/feature-backlog.md`](../20-product/feature-backlog.md) | the triage table |

**Venues are revisable; the three questions are not.** This repo plans to become other things — a git-versioned workspace, an MCP server, a hosted application ([skill architecture §7](../30-architecture/skill-architecture.md)), and per-business-unit packs that may not live here ([platform strategy](../10-strategy/platform-strategy.md)). When that happens the venue column changes and nothing above it does. Three consequences worth stating before they arrive:

| Situation | What holds | What changes |
|---|---|---|
| Work spans two repositories | one unit of work, one issue, opened in the repo that owns the **deliverable** | the other repo gets a link, never a copy. Two trackers reporting one piece of work diverge, and then both are wrong |
| There is no ranking file | the ranked intention still exists and still needs a rank | it goes wherever ranks are kept. If nothing keeps ranks, that absence is the problem — **do not compensate by opening issues nobody is starting** |
| There is no `docs/` tree to land a decision in | the decision still has to outlive the thread that produced it | any durable, linkable, append-only location will do. A comment thread is none of the three |

Changing a venue is a decision, not a preference: it moves a system of record, so it needs an ADR. Adding a *label*, a status value, or a review mode is a PR to this guide — see each section below.

### Which one to open

| If | Open |
|---|---|
| Somebody is going to do something, and there is a state in which it is finished | an **issue** |
| The output is a choice that would be expensive to reverse and confusing to rediscover — see the ADR bar in [`CONTRIBUTING.md`](../../CONTRIBUTING.md) | an **ADR**, `proposed` |
| It is a capability we want, nobody is starting it, and its rank against the other 20 is the actual question | a **backlog row** |
| A decision has been taken and files now have to move as a result | an **ADR** *and* an **issue** — the ADR carries the reasoning, the issue carries the labour |

**A GitHub issue is not a system of record.** Comment threads are not indexed, not linked from `docs/`, and not readable in Obsidian. Anything that must still be findable in a year goes into `docs/` and the issue links to it. This is why #7's second comment reports that *"ADR-0009 moves from `proposed` to `accepted`"* rather than deciding it in the comment: the comment reports the move, the ADR is the move.

### The two mistakes worth naming

**"Decide whether X" opened as an issue.** If the deliverable is a judgement and no files change, that is an ADR in `proposed`, not an issue — opening it as an issue puts the alternatives and the rationale in a comment thread that nothing cites. The legitimate hybrid is #7's scope item 3, *"Answer whether TRACE can be a company-wide platform"*: the work of answering was real work with a deliverable, and the answer landed in ADR-0006. The issue tracked the work; the ADR is the answer.

**An issue per backlog row.** The backlog has 21 rows. Twenty-one issues is a tracker full of things nobody is doing, which trains everyone to ignore the tracker. **Promote a backlog row to an issue when work on it starts, not when it is agreed.** The backlog already carries `Verdict`, `Priority` and `Blocked by`; that is the queue.

---

## Issue anatomy

Three parts, in this order. #7 has all three and nothing else.

| Part | Heading | Job |
|---|---|---|
| Context | *(none — the opening paragraph)* | what the situation was, what changed, and why it matters **now** |
| Problem | `Problem` | concrete bullets, each one falsifiable |
| Scope | `Scope of work` | a numbered list of deliverables that later comments report against |

### The context paragraph

Three moves: what it was, what changed, why that breaks something now. #7:

> TRACE was built as five Claude Code skills for finding and vetting Chinese electronics vendors. It is now expected to serve Pargar's ODM orchestration business — an asset-light manufacturing intermediary that outsources 100% of production to job shops.
>
> That is a structurally different business, not just a different market, and it breaks parts of the existing product definition.

**If you cannot say why now, you have a backlog row, not an issue.** "The docs are out of date" is a permanent condition and generates no urgency; "a second business unit is now expected to use this and the definition does not survive it" is an event. The reason this test matters is that an issue with no *now* never gets picked up, sits open for months, and eventually gets closed unread — which costs more than never opening it.

Keep it to one or two short paragraphs. The detail belongs in `docs/`; the issue points at it.

### The `Problem` section

Bullets, each one **falsifiable** — a statement a reader could disprove. Compare:

| Vague | Falsifiable (#7) |
|---|---|
| The product is too China-focused | *"Reference material is China/electronics-specific, `VendorRecord` encodes export-trade assumptions, and the skills were evaluated only against Chinese enclosure vendors. This blocks any second business unit."* |
| We should write things down | *"Decisions existed only in prose, with no way to tell settled from provisional."* |
| Nothing is saved between sessions | *"Claims are pasted between skill invocations and die with the session, which makes the human-confirmation gate — TRACE's central trust claim — unenforceable."* |

Two reasons the falsifiable form is worth the extra sentence, and the second is the one people miss:

1. **A status update has to report against each problem.** [[issue-updates]] requires a *"problems addressed"* table. A vague problem produces a vague row, and a vague row is unarguable — nobody can say the problem is still there.
2. **A concrete premise can be found wrong, and that is a feature.** #7's framing of which business unit was the incumbent turned out to be backwards on the evidence, and the status comment said so in those words. That correction was only possible because the premise was stated concretely enough to contradict. **A premise too vague to be wrong is also too vague to be corrected.**

Name the file, the field, or the behaviour. Say what it blocks, not just that it is untidy — *"This blocks any second business unit"* is the part that makes the bullet actionable.

### The `Scope of work` section

A **numbered** list. Each item is a verb phrase with a deliverable, and where possible the test it is defended against. #7:

> 1. Determine where TRACE sits in SCM, defended against SCOR and against the Pargar operating model.
> 2. Benchmark the platforms that matter — on-demand manufacturing orchestration, supplier discovery/intelligence, job-shop ERP/MES, e-sourcing, and supplier performance management — and derive the feature set from that evidence rather than from opinion.
> 3. Answer whether TRACE can be a company-wide platform (electronics team and others) or must focus on the Pargar model.
> 4. Produce a roadmap and implementation approach.
> 5. Restructure the repository so the product has a durable record.

**The numbers matter because they become the row keys of the status table.** #7's first status comment opens with a five-row table keyed `1`–`5`, then five detail sections headed `### 1 — Where TRACE sits`, `### 2 — What the benchmarks changed`, and so on. That mapping is free only because the scope was numbered. Bullets force the update's author to re-describe the work in their own words, and re-description is where drift starts: three comments later nobody can tell whether item 2 shrank or the reporter just got tired of typing it out.

Consequences of "the numbers are row keys":

| Rule | Reason |
|---|---|
| **Never renumber.** Append 6, 7, 8 | earlier comments reference the old numbers and become wrong |
| **Never delete an item.** Mark it dropped and say why | same reason ADR numbers are never reused ([`CONTRIBUTING.md`](../../CONTRIBUTING.md)) — a missing number reads as an error, a dropped one reads as a decision |
| Add scope in a comment, not silently in the body | see [[issue-updates]]; a body edit retroactively falsifies every earlier status comment |
| One deliverable per item | an item covering two deliverables gets a half-status, and half-statuses are what status tables exist to prevent |

---

## Titles

**Imperative, specific, no type prefix.** The label carries the type.

| | Example |
|---|---|
| ✅ | `Revise TRACE product definition, feature set, and roadmap against the Pargar ODM model` |
| ❌ | `docs: product definition` — prefix duplicates the `documentation` label, and it is not a sentence |
| ❌ | `Product definition` — no verb; nothing is being asked for |
| ❌ | `Fix the docs` — verb, no object; unfilterable and unactionable |
| ❌ | `Pargar??` — a mood |

Two reasons for each half of the rule:

**Imperative and specific**, because the title is read in a list with no body next to it. It has to be a sentence you could act on. #7's title is 85 characters and the last clause — *"against the Pargar ODM model"* — is doing real work: it names the thing the revision is defended against. Do not trim a qualifier that carries meaning to hit a character count.

**No type prefix**, because that job is already done twice over. Conventional Commits prefixes exist on the *commit* (see [[commit-messages]]) where tooling parses them for SemVer; issue type is a *label*, which is filterable, colour-coded, and costs zero title characters. A `docs:` prefix in an issue title is a third copy that nothing reads. **The title carries what a filter cannot; the label carries what the title cannot.** That division is the rule the next section is built on.

---

## Labels

**A label answers a question a person filtering the issue list needs answered, and that the title cannot answer.** That rule is what keeps the set finite, and everything below is derived from it.

A candidate label passes all three tests or it does not exist:

| Test | Fails when | Reason |
|---|---|---|
| **Somebody runs the filter** | nobody has ever wanted the list it produces | an empty convention is a convention nobody follows, and it teaches people to ignore the labels that do mean something |
| **The title cannot carry it** | the title already says it, or could at no cost | a duplicated answer is a third copy that nothing reads — this is why titles carry no type prefix |
| **It is decidable when the issue is opened** | you need the work done before you can apply it | that is a *status*, and status belongs in the issue's state and its comments, not in a label |

Labels come in **axes**: one question each, answered by mutually exclusive values. Within an axis exactly one value applies; across axes they combine freely. **If two values from the same axis keep getting applied together, the axis is drawn wrong** — that is evidence to redraw it, not licence to apply both.

### Current values — 2026-07-29, revisable

The set is still GitHub's default and has **not** been curated. Four of the nine are in real use; five have never been applied to anything. That is recorded rather than tidied, because an unused label costs nothing and a wrongly-invented one costs a convention nobody follows.

| Axis — the question it answers | Values today | Means here | Status |
|---|---|---|---|
| **Is the behaviour wrong, or merely absent?** | `bug` | shipped behaviour is wrong, including guidance that is wrong | in use in spirit — the customs-data rung defect is a `bug` |
| | `enhancement` | a new or changed capability | in use — #7 |
| **What kind of artifact is the deliverable?** | `documentation` | a document in `docs/`, a root-level `.md`, or a `SKILL.md` | in use — #7 |
| **Can this be scoped at all yet?** | `question` | needs an answer before the work can be scoped | in use |
| **Why was it closed?** | `duplicate`, `invalid`, `wontfix` | applied *at* close, alongside a comment saying which and why | never applied |
| **Who could pick this up?** | `good first issue`, `help wanted` | inherited; meaningless on a single-maintainer repo | never applied — the first candidates for pruning |

The axis model generates the combination rules rather than listing them. `documentation` + `enhancement` is **#7 and the most common pairing here**, because most work in this repo is still definition work — two axes, so they combine. `documentation` + `bug` is shipped guidance that is wrong: the customs-data rung sits at rung 3 of the verification ladder in `sourcing-directories.md` with no market condition attached. `question` alone means scoping is blocked, and it should not stay open long.

**Never `bug` + `enhancement`, because they are one axis.** If you cannot tell whether the behaviour is *wrong* or merely *absent*, the `Problem` section is not concrete enough yet — go back and write it falsifiably. The two have different bars: a bug is fixed because it is wrong, an enhancement is built because it is ranked.

One genuine ambiguity, since it will come up: **in this repo a skill is a markdown file, so the deliverable axis and the wrong-or-absent axis do not look independent.** Apply `bug` when the *behaviour* is wrong and `documentation` when the *record* is wrong; a skill defect is usually both, and both labels is the right answer — different axes. Note this is a property of the current rung, not of the taxonomy: at rung 2 and above ([skill architecture §7](../30-architecture/skill-architecture.md)) part of the product stops being markdown and the two axes separate on their own.

### How the set changes

| Change | Evidence required | Recorded by |
|---|---|---|
| **Add a value** | a filter somebody wanted to run and could not, the three tests pass, and you can name the axis it belongs to | a PR to this file, in the same change that creates the label |
| **Retire a value** | it has never been applied, or its question is now answered by another axis | the same PR, leaving one line saying it was retired and when — a label that silently vanishes reads as data loss |
| **Add or redraw an axis** | a question filterers keep asking that no axis covers, or two values of one axis routinely co-applied | a PR; an ADR only if it changes how work is *assigned*, not how it is filtered |

**Pruning is as legitimate as adding, and here it is the likelier next move.** Five of nine labels have never touched an issue.

Under the futures this repo has already named, the axes behave predictably:

| If | The set |
|---|---|
| packs, the workspace, or a service move to their own repos | a *which surface* axis may earn its place — but only once somebody needs a filter they cannot get by looking at the repo they are already in |
| the product reaches a shared, multi-user rung | a *severity* or *who is affected* axis becomes runnable, because for the first time somebody else's work is blocked by the answer |
| the maintainer set grows past one | `good first issue` and `help wanted` stop being meaningless and should be un-retired rather than reinvented |

---

## Sizing

**An issue you cannot report on in a single status table is too big.** That is the operative test, and it is deliberately about the *reporting*, not the effort. #7 delivered 36 documents, 10 ADRs and 7 benchmark reports across five scope items and still fits one table, because all five items answer one question: *what is TRACE now?*

The signals below are the ones this repo has actually hit; the list is open. **Anything that predicts a status table nobody could write honestly belongs here** — add a row when you meet one, with the case that produced it.

| Signal | Action | Reason |
|---|---|---|
| The scope items would be done by different people at different times | **Split** | one status table cannot honestly report two independent workstreams |
| The scope items share no definition of done | **Split** | the issue has no closing condition, so it will not close |
| One item is blocked and the rest are not | **Sub-issue** for the blocked one | the parent keeps moving; the block stays visible instead of dragging the parent's status down |
| Splitting would produce issues whose conclusions depend on each other | **Keep together** | #7's items 1–3 are mutually dependent; five issues would have deadlocked |
| Scope grew during the work | **New issue**, or an appended numbered item **with a comment saying why** | see [[issue-updates]] |
| Fewer than three scope items and one commit | **No issue** — a branch, a commit and a PR are enough | the counts are the symptom; the rule is that tracker overhead must not exceed the work |

### Sub-issues

A sub-issue is warranted when the child has **its own scope of work and its own status reporting**. That is the whole test. A checklist item inside the parent's body is cheaper and better for anything smaller — a sub-issue that never receives a comment is a checkbox with extra ceremony.

Concretely: had the ADR-0009 plugin move been executed, it would have been a sub-issue of #7 — it has its own scope (move the directory, re-point the marketplace source, verify install paths, update local `--plugin-dir` users), its own risk, and its own report. It was instead declined; see [[issue-updates]] on how a declined decision is written up.

---

## Definition of done

Every issue states its own completion test, in a `Done when` block at the end of the body.

**State done as an observable event, not as an artifact existing.** The model is the roadmap's phase test, quoted in #7's own status comment:

> a phase is done when **a real piece of Pargar work went through it and the output was kept** — not when the code exists.

| Weak | Strong |
|---|---|
| The docs are written | Every scope item has a named file in `docs/`, and `docs/README.md` links all of them |
| `trace-compare` is implemented | One real multi-quote comparison ran through `trace-compare` and the normalized output was kept in the workspace |
| The ADR is merged | *(not a done test — merged ≠ built; see [`CONTRIBUTING.md`](../../CONTRIBUTING.md))* |

Two reasons this is worth a separate block rather than being implied by the scope list:

1. **The person closing the issue should not have to reconstruct the test.** Often that person is the author three weeks later, with different assumptions.
2. **It forecloses the argument at close time.** Without a stated test, "is this done?" becomes a matter of opinion exactly when everyone is tired of the issue and motivated to say yes.

Closing is not automatic. Commits carry `refs #N` in the subject and the `Closes #N` decision lives in the PR body — see [pull requests](pull-requests.md). Every commit on `docs/product-definition` used `refs #7` deliberately, because none of them completed the scope on its own.

---

## Worked examples

### Good — issue #7

| Element | What it does | Why it works |
|---|---|---|
| Title: *Revise TRACE product definition, feature set, and roadmap against the Pargar ODM model* | imperative, names the object and the frame | actionable read cold, in a list |
| *"It is now expected to serve Pargar's ODM orchestration business"* | states the change | supplies the **now** |
| *"That is a structurally different business, not just a different market"* | states why the change breaks something | pre-empts "can't we just add a market?" |
| *"`VendorRecord` encodes export-trade assumptions"* | names the field | falsifiable; a reader can open the file and check |
| *"This blocks any second business unit"* | names the consequence | makes the bullet rankable against other problems |
| Five numbered scope items | each a verb + deliverable + test | became a five-row status table verbatim |
| *"defended against SCOR and against the Pargar operating model"* | names the adversary | the item cannot be satisfied by assertion |

### Bad — the same issue, stripped

```
Title: Update docs

The docs are out of date now that we're working with Pargar.
Need to figure out what TRACE actually is and fix the roadmap.
```

| Failure | Consequence |
|---|---|
| Title has no object and no verb worth acting on | invisible in a list; nobody can filter or prioritise it |
| "out of date" is a permanent condition, not an event | no **now**; this is a backlog row |
| "out of date" is not falsifiable | the status comment can only answer "less out of date now", which is not an answer |
| No numbered scope | no status table is possible, so every update is prose and the updates drift |
| "figure out" has no deliverable | there is no state in which this issue is finished |
| No `Done when` | it will be closed by exhaustion, not by completion |

### Bad — an ADR wearing an issue costume

```
Title: Decide whether to move Claude Plugin/trace to plugins/trace
```

The decision is ADR-0009. Opened as an issue, the alternatives, the rationale, and the constraint that survives either outcome — *the plugin `name` must stay `trace`, because it is immutable once published* — all end up in a comment thread that no document cites, and the next person to touch packaging rediscovers them. **The ADR carries the decision; an issue exists only if somebody has to move the files.** In the event the move was declined with a named revisit trigger, and that fact lives in the ADR where packaging work will find it.

---

## Template

```markdown
<!-- Title: imperative, specific, no type prefix. Labels carry the type. -->

One or two paragraphs: what the situation was, what changed, and why that
matters now. If there is no "now", this is a backlog row, not an issue.

## Problem

- A concrete, falsifiable statement. Name the file, the field, or the
  behaviour. Say what it blocks.
- Another one. A reader should be able to prove it wrong.
- If a bullet could be true of any repo on any day, cut it or sharpen it.

## Scope of work

1. First deliverable — verb, object, and the thing it is defended against.
2. Second deliverable.
3. Third deliverable.

<!-- Numbered, because status comments report against these numbers.
     Never renumber. Never delete an item — mark it dropped, with why. -->

## Done when

<An observable event. Not "the PR is merged" — merged is not built.>

## Links

<!-- Real paths, not wikilinks: an issue body is rendered by GitHub, which
     displays [[foo]] literally and does not link it. -->

- Decisions this will produce or change: ADR-NNNN — docs/40-decisions/NNNN-*.md
- Ranked intentions this draws from: docs/20-product/feature-backlog.md #NN
- Related issues: #N
```

---

## References

Relative markdown links, not wikilinks: this repo is read on GitHub, which renders `[[foo]]` as literal text.

- [CONTRIBUTING.md](../../CONTRIBUTING.md) — naming, document lifecycle, the ADR bar, review norms. This document extends it.
- [docs/README.md](../README.md) — section layout and the linking convention.
- [ADR index](../40-decisions/README.md) — the ADR rules, including *"`proposed` is not a soft `accepted`"*.
- [ADR-0001 — Record architecture decisions](../40-decisions/0001-record-architecture-decisions.md) — why decisions get their own venue at all.
- [Feature backlog](../20-product/feature-backlog.md) — the triage table and priority queue that units of work are promoted from.
- [Roadmap](../20-product/roadmap.md) — phase sequencing and the "done when real work went through it" test.
- [Skill architecture](../30-architecture/skill-architecture.md) §7 — the migration ladder the forecasts above are drawn from.
- [Platform strategy](../10-strategy/platform-strategy.md) — core plus per-business-unit packs, and where they may live.
- [Status updates on issues](issue-updates.md) — how to report against the `Scope of work` and `Problem` sections defined here.
- [Pull requests](pull-requests.md) — `Refs` vs `Closes`, and how an issue is actually closed.
- [Commit messages](commit-messages.md) — Conventional Commits grammar and the subject-line issue reference.
- [Branch naming](branch-naming.md) — branch prefixes and their relationship to the issue.
- Issue [#7](https://github.com/Micheal-Friday/Trace/issues/7) — the in-house reference example for every section above.
