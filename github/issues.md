---
type: process
status: living
version: 2.0
updated: 2026-07-29
tags: [process, contribution, issues, github]
aliases: [issue conventions, writing issues, issue anatomy]
---

# Writing issues

An issue is a **unit of work**: something a person will do, with a finish line somebody else can check. This document says when to open one instead of writing a decision record or a backlog row, what goes in it, and how it should be shaped so that the status comments written against it later — see [status updates](issue-updates.md) — have something to report against.

The reference example throughout is the source project's tracking issue, *"Revise the product definition, feature set, and roadmap against a second business unit's operating model"*. It was the largest issue that project ran and the only one with a full reporting cycle. It is not public, so where this document says "the tracking issue" it reproduces the shape rather than pointing at it — the shape is the part that transfers.

These rules sit under the adopting project's own `CONTRIBUTING.md`. Where the two disagree, that file wins and this one is the bug.

---

## Issue, decision record, or backlog row

**Assign a piece of work by the question it answers, not by the tool you happen to have open.** Three questions, and they do not overlap:

| The question | The thing it produces | Lifecycle | Ends by |
|---|---|---|---|
| *Who is doing what, and when is it finished?* | a **unit of work** | open → closed | being delivered |
| *What did we choose, and why?* | a **decision** | `proposed` → `accepted` \| `rejected` → `superseded` | being superseded — never edited, never deleted |
| *What do we want, and in what order?* | a **ranked intention** | a verdict and a priority, re-triaged | being promoted to a unit of work, or re-triaged |

The three are confused constantly because a single piece of thinking usually produces all three, and producing all three is not duplication — it is the point. The tracking issue was one unit of work; it produced ten decision records and a re-triaged backlog of 21 items. Nothing was written twice: the issue tracked the doing, the records recorded the choosing, and the backlog recorded the ordering.

Each question implies its own storage. A unit of work needs a finish line somebody else can check. A decision needs to be findable by whoever hits the constraint next, which means durable, linkable, and never rewritten. A ranked intention needs to sit next to its competitors, because the rank *is* the content.

### Where each lives

**Venues are per-project; the three questions are not.** Record the mapping once, in the adopting project's own `CONTRIBUTING.md`, dated and marked revisable. The usual shape: units of work in the issue tracker, decisions in a numbered series under `docs/`, ranked intentions in a single backlog file whose triage table carries the rank. Three consequences worth stating before they arrive, because they hold whatever the venues are:

| Situation | What holds | What changes |
|---|---|---|
| Work spans two repositories | one unit of work, one issue, opened in the repo that owns the **deliverable** | the other repo gets a link, never a copy. Two trackers reporting one piece of work diverge, and then both are wrong |
| There is no ranking file | the ranked intention still exists and still needs a rank | it goes wherever ranks are kept. If nothing keeps ranks, that absence is the problem — **do not compensate by opening issues nobody is starting** |
| There is no `docs/` tree to land a decision in | the decision still has to outlive the thread that produced it | any durable, linkable, append-only location will do. A comment thread is none of the three |

Changing a venue is a decision, not a preference: it moves a system of record, so it needs a decision record. Adding a *label*, a status value, or a review mode is a change request against this guide — see each section below.

### Which one to open

| If | Open |
|---|---|
| Somebody is going to do something, and there is a state in which it is finished | an **issue** |
| The output is a choice that would be expensive to reverse and confusing to rediscover — see the bar in [decision records](../docs/decisions.md) | a **decision record**, `proposed` |
| It is a capability we want, nobody is starting it, and its rank against the other 20 is the actual question | a **backlog row** |
| A decision has been taken and files now have to move as a result | a **decision record** *and* an **issue** — the record carries the reasoning, the issue carries the labour |

**A GitHub issue is not a system of record.** Comment threads are not indexed, not linked from `docs/`, and not readable in an editor. Anything that must still be findable in a year goes into `docs/` and the issue links to it. This is why the tracking issue's second comment reports that *"ADR-0009 moves from `proposed` to `accepted`"* rather than deciding it in the comment: the comment reports the move, the record is the move.

### The two mistakes worth naming

**"Decide whether X" opened as an issue.** If the deliverable is a judgement and no files change, that is a decision record in `proposed`, not an issue — opening it as an issue puts the alternatives and the rationale in a comment thread that nothing cites. The legitimate hybrid is the tracking issue's scope item 3, *"Answer whether the product can serve the whole company"*: the work of answering was real work with a deliverable, and the answer landed in a decision record. The issue tracked the work; the record is the answer.

**An issue per backlog row.** That project's backlog had 21 rows. Twenty-one issues is a tracker full of things nobody is doing, which trains everyone to ignore the tracker. **Promote a backlog row to an issue when work on it starts, not when it is agreed.** A backlog that already carries `Verdict`, `Priority` and `Blocked by` is the queue.

---

## Issue anatomy

Three parts, in this order. The tracking issue has all three and nothing else.

| Part | Heading | Job |
|---|---|---|
| Context | *(none — the opening paragraph)* | what the situation was, what changed, and why it matters **now** |
| Problem | `Problem` | concrete bullets, each one falsifiable |
| Scope | `Scope of work` | a numbered list of deliverables that later comments report against |

### The context paragraph

Three moves: what it was, what changed, why that breaks something now. In the source project:

> The product was built as five skills for finding and vetting suppliers in one market. It is now expected to serve a second business unit whose production is 100% outsourced.
>
> That is a structurally different business, not just a different market, and it breaks parts of the existing product definition.

**If you cannot say why now, you have a backlog row, not an issue.** "The docs are out of date" is a permanent condition and generates no urgency; "a second business unit is now expected to use this and the definition does not survive it" is an event. The reason this test matters is that an issue with no *now* never gets picked up, sits open for months, and eventually gets closed unread — which costs more than never opening it.

Keep it to one or two short paragraphs. The detail belongs in `docs/`; the issue points at it.

### The `Problem` section

Bullets, each one **falsifiable** — a statement a reader could disprove. Compare:

| Vague | Falsifiable |
|---|---|
| The product is too focused on one market | *"Reference material is specific to the first market, the core record type encodes its trade assumptions, and the skills were evaluated only against its suppliers. This blocks any second business unit."* |
| We should write things down | *"Decisions existed only in prose, with no way to tell settled from provisional."* |
| Nothing is saved between sessions | *"Claims are pasted between invocations and die with the session, which makes the human-confirmation gate — the product's central trust claim — unenforceable."* |

Two reasons the falsifiable form is worth the extra sentence, and the second is the one people miss:

1. **A status update has to report against each problem.** [Status updates](issue-updates.md) require a *"problems addressed"* table. A vague problem produces a vague row, and a vague row is unarguable — nobody can say the problem is still there.
2. **A concrete premise can be found wrong, and that is a feature.** The tracking issue's framing of which business unit was the incumbent turned out to be backwards on the evidence, and the status comment said so in those words. That correction was only possible because the premise was stated concretely enough to contradict. **A premise too vague to be wrong is also too vague to be corrected.**

Name the file, the field, or the behaviour. Say what it blocks, not just that it is untidy — *"This blocks any second business unit"* is the part that makes the bullet actionable.

### The `Scope of work` section

A **numbered** list. Each item is a verb phrase with a deliverable, and where possible the test it is defended against. In the source project:

> 1. Determine where the product sits in the wider process model, defended against the industry reference model and against the second unit's operating model.
> 2. Benchmark the platforms that matter and derive the feature set from that evidence rather than from opinion.
> 3. Answer whether the product can serve the whole company or must focus on one unit's model.
> 4. Produce a roadmap and implementation approach.
> 5. Restructure the repository so the product has a durable record.

**The numbers matter because they become the row keys of the status table.** That issue's first status comment opens with a five-row table keyed `1`–`5`, then five detail sections headed `### 1 — Where the product sits`, `### 2 — What the benchmarks changed`, and so on. That mapping is free only because the scope was numbered. Bullets force the update's author to re-describe the work in their own words, and re-description is where drift starts: three comments later nobody can tell whether item 2 shrank or the reporter just got tired of typing it out.

Consequences of "the numbers are row keys":

| Rule | Reason |
|---|---|
| **Never renumber.** Append 6, 7, 8 | earlier comments reference the old numbers and become wrong |
| **Never delete an item.** Mark it dropped and say why | same reason numbers in a series are never reused ([document lifecycle](../docs/document-lifecycle.md)) — a missing number reads as an error, a dropped one reads as a decision |
| Add scope in a comment, not silently in the body | see [status updates](issue-updates.md); a body edit retroactively falsifies every earlier status comment |
| One deliverable per item | an item covering two deliverables gets a half-status, and half-statuses are what status tables exist to prevent |

---

## Titles

**Imperative, specific, no type prefix.** The label carries the type.

| | Example |
|---|---|
| ✅ | `Revise the product definition, feature set, and roadmap against the second unit's operating model` |
| ❌ | `docs: product definition` — prefix duplicates the `documentation` label, and it is not a sentence |
| ❌ | `Product definition` — no verb; nothing is being asked for |
| ❌ | `Fix the docs` — verb, no object; unfilterable and unactionable |
| ❌ | `Roadmap??` — a mood |

Two reasons for each half of the rule:

**Imperative and specific**, because the title is read in a list with no body next to it. It has to be a sentence you could act on. The title above is long, and its last clause — *"against the second unit's operating model"* — is doing real work: it names the thing the revision is defended against. Do not trim a qualifier that carries meaning to hit a character count.

**No type prefix**, because that job is already done twice over. Conventional Commits prefixes exist on the *commit* (see [commit messages](../git/commit-messages.md)) where tooling parses them for SemVer; issue type is a *label*, which is filterable, colour-coded, and costs zero title characters. A `docs:` prefix in an issue title is a third copy that nothing reads. **The title carries what a filter cannot; the label carries what the title cannot.** That division is the rule the next section is built on.

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

### Current values

**The values are per-project.** Keep them in the adopting project's own `CONTRIBUTING.md`, dated and marked revisable, with which ones are actually in use — an unused label costs nothing and a wrongly-invented one costs a convention nobody follows. Most forges ship a default set that has never been curated, and recording that honestly beats tidying it.

The axes generate the combination rules rather than listing them. A typical set answers four questions: *is the behaviour wrong, or merely absent?* (`bug` | `enhancement`), *what kind of artifact is the deliverable?* (`documentation`), *can this be scoped at all yet?* (`question`), and *why was it closed?* (`duplicate` | `invalid` | `wontfix`, applied *at* close alongside a comment saying which and why). So `documentation` + `enhancement` is the common pairing wherever most of the work is still definition work — two axes, so they combine. `documentation` + `bug` is shipped guidance that is wrong. `question` alone means scoping is blocked, and it should not stay open long.

**Never `bug` + `enhancement`, because they are one axis.** If you cannot tell whether the behaviour is *wrong* or merely *absent*, the `Problem` section is not concrete enough yet — go back and write it falsifiably. The two have different bars: a bug is fixed because it is wrong, an enhancement is built because it is ranked.

One genuine ambiguity, since it will come up: **where the product itself is markdown, the deliverable axis and the wrong-or-absent axis do not look independent.** Apply `bug` when the *behaviour* is wrong and `documentation` when the *record* is wrong; such a defect is usually both, and both labels is the right answer — different axes. Note this is a property of the packaging, not of the taxonomy: once part of the product stops being markdown, the two axes separate on their own.

### How the set changes

| Change | Evidence required | Recorded by |
|---|---|---|
| **Add a value** | a filter somebody wanted to run and could not, the three tests pass, and you can name the axis it belongs to | a change request against the project's own list, in the same change that creates the label |
| **Retire a value** | it has never been applied, or its question is now answered by another axis | the same change request, leaving one line saying it was retired and when — a label that silently vanishes reads as data loss |
| **Add or redraw an axis** | a question filterers keep asking that no axis covers, or two values of one axis routinely co-applied | a change request; a decision record only if it changes how work is *assigned*, not how it is filtered |

**Pruning is as legitimate as adding, and on an uncurated set it is the likelier next move.** An axis also becomes runnable only once somebody else's work depends on the answer: a *severity* or *who is affected* axis is noise on a single-maintainer project and load-bearing on a shared one.

---

## Sizing

**An issue you cannot report on in a single status table is too big.** That is the operative test, and it is deliberately about the *reporting*, not the effort. The tracking issue delivered 36 documents, 10 decision records and 7 benchmark reports across five scope items and still fits one table, because all five items answer one question: *what is this product now?*

The signals below are the ones that project actually hit; the list is open. **Anything that predicts a status table nobody could write honestly belongs here** — add a row when you meet one, with the case that produced it.

| Signal | Action | Reason |
|---|---|---|
| The scope items would be done by different people at different times | **Split** | one status table cannot honestly report two independent workstreams |
| The scope items share no definition of done | **Split** | the issue has no closing condition, so it will not close |
| One item is blocked and the rest are not | **Sub-issue** for the blocked one | the parent keeps moving; the block stays visible instead of dragging the parent's status down |
| Splitting would produce issues whose conclusions depend on each other | **Keep together** | the tracking issue's items 1–3 are mutually dependent; five issues would have deadlocked |
| Scope grew during the work | **New issue**, or an appended numbered item **with a comment saying why** | see [status updates](issue-updates.md) |
| Fewer than three scope items and one commit | **No issue** — a branch, a commit and a change request are enough | the counts are the symptom; the rule is that tracker overhead must not exceed the work |

### Sub-issues

A sub-issue is warranted when the child has **its own scope of work and its own status reporting**. That is the whole test. A checklist item inside the parent's body is cheaper and better for anything smaller — a sub-issue that never receives a comment is a checkbox with extra ceremony.

Concretely: had the proposed move of a published directory path been executed, it would have been a sub-issue of the tracking issue — it has its own scope (move the directory, re-point the catalog that resolves it, verify install paths, update anyone pinned to the old one), its own risk, and its own report. It was instead declined; see [status updates](issue-updates.md) on how a declined decision is written up.

---

## Definition of done

Every issue states its own completion test, in a `Done when` block at the end of the body.

**State done as an observable event, not as an artifact existing.** The model is the roadmap's phase test, quoted in the tracking issue's own status comment:

> a phase is done when **a real piece of the business's work went through it and the output was kept** — not when the code exists.

| Weak | Strong |
|---|---|
| The docs are written | Every scope item has a named file in `docs/`, and the docs index links all of them |
| The comparison tool is implemented | One real comparison ran through it and the normalized output was kept in the workspace |
| The decision record is merged | *(not a done test — merged ≠ built; see [document lifecycle](../docs/document-lifecycle.md))* |

Two reasons this is worth a separate block rather than being implied by the scope list:

1. **The person closing the issue should not have to reconstruct the test.** Often that person is the author three weeks later, with different assumptions.
2. **It forecloses the argument at close time.** Without a stated test, "is this done?" becomes a matter of opinion exactly when everyone is tired of the issue and motivated to say yes.

Closing is not automatic. Commits carry `refs #N` in the subject and the `Closes #N` decision lives in the change-request body — see [pull requests](pull-requests.md). Every commit on the source project's docs branch used `refs` deliberately, because none of them completed the scope on its own.

---

## Worked examples

### Good — the tracking issue

| Element | What it does | Why it works |
|---|---|---|
| Title: *Revise the product definition, feature set, and roadmap against the second unit's operating model* | imperative, names the object and the frame | actionable read cold, in a list |
| *"It is now expected to serve a second business unit"* | states the change | supplies the **now** |
| *"That is a structurally different business, not just a different market"* | states why the change breaks something | pre-empts "can't we just add a market?" |
| *"the core record type encodes the first market's trade assumptions"* | names the field | falsifiable; a reader can open the file and check |
| *"This blocks any second business unit"* | names the consequence | makes the bullet rankable against other problems |
| Five numbered scope items | each a verb + deliverable + test | became a five-row status table verbatim |
| *"defended against the reference model and against the second unit's operating model"* | names the adversary | the item cannot be satisfied by assertion |

### Bad — the same issue, stripped

```
Title: Update docs

The docs are out of date now that we're working with a second business unit.
Need to figure out what this product actually is and fix the roadmap.
```

| Failure | Consequence |
|---|---|
| Title has no object and no verb worth acting on | invisible in a list; nobody can filter or prioritise it |
| "out of date" is a permanent condition, not an event | no **now**; this is a backlog row |
| "out of date" is not falsifiable | the status comment can only answer "less out of date now", which is not an answer |
| No numbered scope | no status table is possible, so every update is prose and the updates drift |
| "figure out" has no deliverable | there is no state in which this issue is finished |
| No `Done when` | it will be closed by exhaustion, not by completion |

### Bad — a decision record wearing an issue costume

```
Title: Decide whether to move the published plugin directory
```

That decision was a decision record. Opened as an issue, the alternatives, the rationale, and the constraint that survives either outcome — *the published package name must not change, because it is immutable once anyone has installed it* — all end up in a comment thread that no document cites, and the next person to touch packaging rediscovers them. **The record carries the decision; an issue exists only if somebody has to move the files.** In the event the move was declined with a named revisit trigger, and that fact lives in the record where packaging work will find it.

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

- Decisions this will produce or change: ADR-NNNN — docs/decisions/NNNN-*.md
- Ranked intentions this draws from: the backlog file, row NN
- Related issues: #N
```

---

## References

Relative markdown links, not wikilinks: this repo is read on GitHub, which renders `[[foo]]` as literal text.

- [Document lifecycle](../docs/document-lifecycle.md) — naming, numbered series, statuses, and *merged ≠ built*.
- [Decision records](../docs/decisions.md) — the bar a decision has to clear, and *"`proposed` is not a soft `accepted`"*.
- [Status updates on issues](issue-updates.md) — how to report against the `Scope of work` and `Problem` sections defined here.
- [Pull requests](pull-requests.md) — `Refs` vs `Closes`, and how an issue is actually closed.
- [Commit messages](../git/commit-messages.md) — Conventional Commits grammar and the subject-line issue reference.
- [Branch naming](../git/branch-naming.md) — branch prefixes and their relationship to the issue.
- The adopting project's own `CONTRIBUTING.md` — the label values in force, the venue for each of the three questions, and the content conventions this document assumes.
