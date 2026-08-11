---
type: process
status: living
version: 1.2
updated: 2026-07-29
tags: [process, contribution, issues, status-reporting, honesty]
aliases: [status updates, issue comments, status comment, reporting against scope]
---

# Status updates on issues

A status update is a comment that reports the state of the work **against the issue's own structure** — its numbered scope items, then its stated problems, then what is genuinely open. It is written for the person who picks this up next, which is usually you in three weeks with different assumptions.

This is the least standard of the three contribution documents and has the least prior art anywhere. The convention here is derived from the two status comments on [#7](https://github.com/Micheal-Friday/Trace/issues/7), which are the only worked examples that exist. Read them before writing one.

**A status update reports against structure, not chronology.** Not *"here is what I did this week"* but *"here is item 3"*. Everything below follows from that.

Extends [`CONTRIBUTING.md`](../../CONTRIBUTING.md); depends on the issue anatomy in [[issues]].

---

## When an update is owed

**An update is owed the moment the issue's page stops telling the truth** — that is, when somebody reading only the issue and its comments would form a belief you know to be wrong. Every trigger below is an instance of that, and a situation not listed is still a trigger if it meets the rule.

| Trigger | Why | Minimum content |
|---|---|---|
| **A scope item completes** | the status table changes; a stale table lies | the row, and where the deliverable is |
| **A finding changes the plan** | plans get followed by people who did not make the finding | the finding, and precisely what it changed |
| **A decision is needed from someone else** | the work is blocked and nobody knows it | the choice, the options, a recommendation, and the default if nobody answers |
| **The scope changes** | every earlier status comment now reports against a different issue | the new numbered item, and why it was appended rather than opened separately |
| **A premise in the issue turns out to be wrong** | everything downstream inherits it | see the honesty rules |
| **Silence has gone on long enough to mislead** | see below | where it actually is, even if the answer is "not started" |
| **Immediately before opening the PR** | the reviewer's route starts here | the state the PR is landing into — see [[pull-requests]] |

**Silence is a status report, and it reports "nothing is happening here."** An issue with no comment for three weeks reads as abandoned to one person and as nearly-done to another, and it is usually neither. If the true state differs from "nothing is happening", a comment is owed — including the comment that says *"paused, because X; resuming when Y"*. That is a real status and costs two lines.

**The update goes wherever the scope lives, and only there.** One unit of work has one page reporting on it, however many repositories, packs or services the commits landed in — see [writing issues](issues.md). A second tracker gets a link, never a copy of the table: two copies of a status table diverge on the first change, and then neither can be trusted.

---

## Structure

**A block earns its place by answering a question no other block answers, and the order is fixed so that a reader who stops early is not misled.** That is why the ask is **last and headed** — a reader who only wants the ask can jump to it, and a reader who stops after block one still has a true summary. Adding a block means naming the question; a block that only re-answers another block's question is length without navigation.

| # | Block | Heading pattern | Required |
|---|---|---|---|
| 0 | Headline | `## Status — <what is done and what is owed>` | always |
| 1 | Scope | `### Scope of work` — table keyed by the issue's numbers, then per-item detail sections | always |
| 2 | Problems | `## The problems in this issue, addressed` — two-column table | always |
| 3 | Open | `## Open — N decisions, not outstanding work` and, separately, work still to do | always, even if empty |
| 4 | Limits | `## Not reached, recorded rather than hidden` | whenever research or work could not be completed |

### 0 — The headline

#7's first comment: `## Status — scope of work complete, three decisions open`.

**The heading names both halves: what is done and what is owed.** A reader who reads nothing else is not misled. `## Update` and `## Progress` are headings that carry no information and are worse than none, because they suggest the reader must open the body to learn the state.

### 1 — Scope of work

A table keyed by the issue's own numbers, with the item text taken verbatim from the issue.

| # | Item | Status | Where |
|---|---|---|---|
| 1 | Where TRACE sits in SCM | ✅ | `docs/10-strategy/positioning.md` v2, ADRs 0002–0005 |
| 2 | Benchmark and derive the feature set from evidence | ✅ | `docs/50-research/` (7 reports), `docs/20-product/capability-map.md` |

Two rules about the columns:

**The status column exists to answer two questions and no others: should the reader still expect a deliverable, and does anything need them?** Those two questions have four combinations, which is why the vocabulary has exactly four values and why it stays scannable:

| Still expect a deliverable? | Needs somebody outside? | State | Today's glyph |
|---|---|---|---|
| yes | no | in progress | ◐ |
| yes | yes | blocked | ⛔ |
| no — it arrived | no | done | ✅ |
| no — it will not arrive | no | dropped, with why | ➖ |

**The glyphs are typography, not vocabulary.** If a surface renders them badly, change the symbols and keep the four states. A status column containing prose is not scannable, and the prose belongs in the per-item section below: if a row needs a sentence, it needs a section.

A fifth value is warranted only when there is a fifth answer to the two questions *and* it changes what the reader does. One is already foreseeable: once a deliverable is a record that a human must confirm rather than a file that exists, *produced* and *confirmed* stop being the same state, and the table earns a value for the gap between them. Add it by PR to this file when the confirmation gate is real, not before — a value nobody can apply yet dilutes the four that work.

**Every row carries a `Where`.** A status table without locations is a set of claims; with them it is checkable. This is the single cheapest honesty mechanism in the format: a ✅ you cannot supply a path for is not a ✅. `Where` is a location, not a venue — a file path today, a record identifier or a URL later.

Then, only for items where the row is not enough, a detail section numbered to match: `### 1 — Where TRACE sits`, `### 2 — What the benchmarks changed`. Numbering to match is what keeps a long comment navigable — #7's first comment is over 1,500 words and remains readable entirely because of this.

### 2 — The problems, addressed

A two-column table, problems verbatim from the issue.

| Problem stated | Addressed by |
|---|---|
| No defensible scope boundary | The four questions, plus explicit `REFUSE` rows in `capability-map.md` and a system-of-record map naming the owner of every adjacent question |
| No persistence, no review gate | ADR-0007 — a git-versioned workspace, `review-queue.jsonl`, and `trace-review` |

**The scope of work is what you agreed to do; the problems are why.** Those come apart more often than anyone expects. **Delivering every scope item and not fixing the problem is a real outcome, and this table is the only place it becomes visible.** If a row's right-hand column says *"partially"* or *"not yet"*, that is the most valuable line in the comment — it is also the line most likely to be quietly omitted.

### 3 — Open

**Split open items by what happens if nobody acts, because that is what determines who has to read them.** Two outcomes exist today, so there are two headings; a third heading is warranted only when ignoring an item produces a third outcome. One is foreseeable — an item waiting on a party outside the project neither takes a silent default nor stays visibly undone, it stalls with nobody owning it, and once work depends on other people that distinction will need its own heading.

Today, always split in two, under separate headings.

| | **Decisions someone must make** | **Work still to do** |
|---|---|---|
| Blocked on | a judgement | time |
| Resolved by | somebody choosing | somebody working |
| If ignored | **the default happens silently** | the item stays undone, visibly |
| Lives on afterwards as | an ADR, if it meets the ADR bar | a roadmap phase, a backlog row, or a follow-up issue |

**Conflating decisions with work is the most common failure of this format, and it is not cosmetic. A decision buried in a to-do list gets treated as a to-do, and to-dos wait. A decision that waits is a decision made by default** — usually the worst one, since nobody chose it.

#7 gets this right at the level of the heading itself: `## Open — three decisions, not outstanding work`. The heading does the work before the reader reaches the list.

Each open decision carries four things:

| Element | #7, item 1 |
|---|---|
| The choice | moving `Claude Plugin/trace/` → `plugins/trace/` |
| The recommendation, with its reason | recommended — the marketplace `source` currently contains a space |
| Why it was not executed | it touches a published path |
| The constraint that survives either outcome | *"The plugin `name` must stay `trace`; it is immutable once published"* |

That fourth element is worth its own rule: **record the facts that make the decision cheap to revisit, so it does not have to be re-derived.** #7's second comment does this explicitly — *"Two facts are recorded in the ADR so this can be revisited without re-deriving them."*

And when a decision is declined rather than taken:

**A decision declined without a revisit trigger is a decision deferred forever.** #7's second comment names the trigger: *"**Revisit when** packaging changes for a substantive reason: rung 2 of the ladder (a bundled MCP server), a second distributed plugin, or CI tripping over the space."* Three named, observable conditions. Not "later".

### 4 — Not reached

See honesty rule 3.

---

## The honesty rules

These are the reason the format exists. A status update that only reports success is a press release, and the repo already has documents for what went well.

### 1. Report findings that contradict the issue's own premise

**The issue was written before the work. The work is the only thing with evidence.**

#7's body implies the electronics business is the settled case and Pargar is the new, risky adopter. The status comment says otherwise, in the issue's own terms:

> One correction to the framing in this issue's premise, on the evidence: **the electronics team is the incumbent user of this artifact, not the future one.** The five skills were written and evaluated against Chinese enclosure vendors, the reference material is a China/global directory list and a Western/electronics certification guide, and `VendorRecord` holds export-trade fields. Pargar is the new and harder adopter. The generalization risk runs the opposite way from expected.

How to write one:

| Do | Do not |
|---|---|
| Name the premise, in the issue's own words | bury it as "worth noting" or "interesting nuance" |
| Name the evidence — files, fields, what was tested against what | assert the correction and move on |
| State which way it actually runs, and what that changes downstream | leave the reader to work out the implication |
| Put it in a comment | **edit the issue body to make the old premise look right** |

The last one deserves the emphasis it gets. **Corrections go in comments, never into the issue body** — for the same reason an accepted ADR is never rewritten. A silently corrected premise leaves no record that anyone was ever wrong, which means the *reason* the mistake was made is gone too, and it will be made again. It also retroactively falsifies every earlier comment that reported against the old framing.

An update that quietly conforms to a wrong premise makes the premise durable. The issue is the first thing a future reader reads.

### 2. Report defects found in your own prior work, not just in the code

#7 reports three, and they are three different kinds. The kinds matter:

| Kind | #7's instance | Why it is easy to swallow |
|---|---|---|
| **Shipped guidance is wrong** | the customs-data rung sits at rung 3 of the verification ladder with no market condition — it produces nothing for a domestic Iranian workshop, so the ladder reports *unverified* where the truth is *invisible to a channel that does not apply here* | it works fine in the market it was written for |
| **The schema is wrong, found by external evidence** | `qualification.stage` was a single global enum; qualification is scoped and plural everywhere mature, and AS9100D §8.4.1.1 requires approval status **and** scope of approval | nothing broke, because nothing implements it yet |
| **Your own reasoning was wrong** | *"one rule of mine was over-confident and is now corrected — scoring only vendor-attributed events is gameable by whoever fills the attribution field, so raw and attributed now publish side by side"* | **nothing forces it out** |

The third kind is the one this rule is really about. **Nobody else will find a defect in reasoning that exists only in your head.** A status update is the cheapest possible moment to surface it — before anything is built on it. The customs-data defect was caught by the leak test and costs a phase-0 fix; had it been caught after phase 1 it would have been a migration of real records.

Two further practices from #7 worth copying:

- **Say what found the defect.** *"Schema defect found by benchmarking"* is a heading that tells the reader which mechanism is working, so it gets used again. The decision index records the same thing about ADR-0010: *"the first ADR written from external evidence rather than from internal reasoning… That is the record working as intended."*
- **Separate the fact from the defect when they differ.** *"The defect is not the fact; it is that a market-conditional channel sits in the core with no condition attached."* The customs-data claim is true where it applies. Naming precisely what is broken prevents an over-correction that deletes a good signal.

### 3. Report research or work that could not be completed, and why

Required by [`CONTRIBUTING.md`](../../CONTRIBUTING.md) §Research: *"State what the pass could not reach. A benchmark that hides its gaps is worse than no benchmark, because conclusions get built on absence."*

**Quietly narrowing scope to what you could reach turns a gap into a finding.** That is the specific harm: the reader cannot distinguish *"we looked and there is nothing there"* from *"we could not look."* Those support opposite conclusions.

**Label a gap by what would close it**, because that is what the reader has to know and it is the only thing that distinguishes the three:

| Label | Means | Closed by | Obligation |
|---|---|---|---|
| **Not reached** | tried, failed | an obstacle being removed | say *how* it failed — behind login, HTTP 403, no public documentation |
| **Out of scope** | deliberately excluded | a boundary moving | cite the boundary that excludes it |
| **Deferred** | will be reached | time, and somebody | say when, and by what |

A fourth label needs a fourth way of closing. There is not one today.

#7's block, which is the model:

> - **JAGGAER, Ivalua, GEP, Zycus and Workday publish no public product documentation.** Everything attributed to them is marketing copy and marked unverified. JAGGAER's optimizer is the largest single unverified claim in the corpus.
> - Keelvar's optimizer guide and Coupa's CSO constraint language are behind login; Craft.co and Kompass return HTTP 403.
> - Supplier risk / n-tier mapping was not benchmarked — out of scope per the positioning boundary.
> - **None of these blocks a build decision.** Where a conclusion depends on an unreached area it is flagged in place — see `capability-map.md` §11, which records that of six claims originally resting on absent research, **two turned out to be wrong** and are corrected.

Note the last bullet. **A gap list must end with an impact assessment.** Without it the reader has to decide for themselves whether the gaps matter, and they have less information than you do. And note what the impact assessment admits: two of six claims resting on absent research were wrong. That number is the argument for the whole rule.

Finally: *"unverified" means "could not be confirmed from a primary source," not "false."* Never silently promote an unverified claim by dropping the label.

### 4. Distinguish `proposed` from `accepted`

`proposed` is not a soft `accepted` ([`docs/40-decisions/README.md`](../40-decisions/README.md) rule 4). **Never let a recommendation read as a decision taken.**

The failure is asymmetric and that is why it needs a rule: a recommendation mistaken for a decision gets built on, and the first person to notice it was never decided is the person it breaks. A decision mistaken for a recommendation merely gets re-litigated.

| Write | When | Not |
|---|---|---|
| "ADR-NNNN is `proposed`, **not accepted**" | the decision has not been taken | "we're going with X" |
| "Recommended: X. **Not executed**, because Y" | you have a view; the authority or the moment is elsewhere | "X is next" |
| "**Declined for now.** Revisit when Z" | decided against, reversibly, with a trigger | "we probably won't do X" |
| "ADR-NNNN **moves from `proposed` to `accepted`**" | the decision is taken, and the record now says so | "ADR-NNNN is done" |

Both #7 comments are precise about this. The first: *"ADR-0009 is `proposed`, not accepted. Moving `Claude Plugin/trace/` → `plugins/trace/` is recommended… but it touches a published path, so it was not executed."* The second: *"ADR-0009 moves from `proposed` to `accepted`, with both outcomes recorded in place rather than deferred to a follow-up."*

**The test before posting: for every recommendation in the comment, can a reader tell who decided it and when?** If not, mark it `proposed` in those words. And remember the comment is not the record — the ADR is. The comment reports that the record moved.

---

## What not to do

**Every row below produces a comment the reader cannot act on or cannot check.** That is the test for adding one: not that it is untidy, but that it leaves a reader unable to answer *where is this* or *what does it need from me*.

| Anti-pattern | Why it fails | Instead |
|---|---|---|
| **Progress theatre** — "made good progress on the benchmarks" | unfalsifiable, and it makes the next update harder: you now have to explain what changed since a baseline that was never stated | say which of the seven reports exist, and where |
| **Restating the issue back** | the issue is one click up the page; a restatement costs the reader the same attention as the actual news and delivers none | report *against* it — the tables reference it, they do not reproduce it |
| **Burying a needed decision mid-comment** | decisions written as prose read as commentary and are not actioned | last block, own heading, numbered |
| **Chronology instead of structure** — "Monday I…, then I…" | nobody needs the order of operations; they need the state | the scope table |
| A status table with no `Where` column | uncheckable claims | a path per row |
| **Silent scope narrowing** | the gap becomes invisible and conclusions get built on absence | the not-reached block, with impact |
| Editing the issue body to match what was done | falsifies every earlier comment and erases that anyone was wrong | a comment appending a numbered item, with why |
| Mixing decisions into the work list | to-dos wait; decisions cannot afford to | two headings |
| A wall of text with no headings | see below | headings and tables |

On the last: **length is allowed; unstructured length is not.** #7's comments are long — the first runs well past 1,500 words — and they are readable because every block has a heading, the tables carry the summary, and the ask is at the end under its own numbered heading. The rule is not "be brief". It is "be navigable". A reader must be able to answer *where is this* and *what does it need from me* without reading the middle.

---

## Closing comments

A closing comment is a status comment with three additional obligations. The difference is not tone; it is that a closing comment is the **last** thing anyone will read on the issue.

| | Status comment | Closing comment |
|---|---|---|
| Answers | *where is this?* | *why is this closed?* |
| Scope table | current state, any status | **every row terminal** — ✅ or ➖ with a reason. Nothing ◐ |
| Open block | decisions and work, both live | **empty, or every item rehomed by name** |
| Must also contain | — | the completion test and its result; the commits or PR that delivered it; what comes next, with a location |

A closing comment must:

1. **State the issue's `Done when` test and whether it was met.** If the test was never stated, say so and state the one you are closing against — the next issue should not repeat the omission.
2. **List the commits or the PR.** #7's second comment does exactly this, and the table is the format to copy: the docs-tree commit — *the product definition — 40 files, 7,014 insertions*; the second — *prototype removal and the two decisions above, recorded in ADR-0009*.
3. **Terminate every scope row.** An issue closed with an item still in progress silently converts unfinished work into finished work.
4. **Rehome anything that survives the close, by name.** *"Nothing in this issue's scope of work is now outstanding. What comes next is build work rather than definition work, and it is sequenced in `docs/20-product/roadmap.md`."* A successor location, not a promise.
5. **State how the issue actually closes.** Commits carry `refs #N` in the subject, so merging does not auto-close — #7's comment says so and names the two options. See [pull requests](pull-requests.md).

**An update that says "nothing is outstanding" while leaving the issue open must say what it is waiting on.** #7's second comment is precisely this case: the scope is complete, the decisions are resolved, and the issue stays open pending the PR. Saying so is what keeps "open" meaningful.

---

## Worked examples

### Good — the two comments on #7

The first comment's skeleton, which is the format this document describes:

```
## Status — scope of work complete, three decisions open

### Scope of work                              <- table keyed 1–5, every row with a path
### 1 — Where TRACE sits                       <- per-item detail, numbered to match
### 2 — What the benchmarks changed
### 3 — Platform or Pargar-only
### 4 — Roadmap
### 5 — Repository structure

## The problems in this issue, addressed       <- two-column table, problems verbatim
## Defect found in shipped code                <- honesty rule 2
## Schema defect found by benchmarking         <- honesty rule 2, and names the mechanism
## Open — three decisions, not outstanding work <- honesty rule 4, ask at the end
## Research limits, recorded rather than hidden <- honesty rule 3, ends with impact
```

The correction to the issue's own premise (honesty rule 1) sits inside `### 3`, at the point where the evidence for it was produced — not in a separate apology section. That placement is right: a correction belongs next to the finding that produced it.

The second comment is the closing-shaped one: three numbered sections matching the three open decisions from the first comment, a commit table, an explicit `proposed` → `accepted` transition, and a forward pointer to [[roadmap]] phase 0 with its three named tasks.

### Bad

```
Update: good progress! Benchmarking is mostly done, I've been through most of
the S2C tools and the ERPs, and the positioning doc is coming together nicely.
Still need to think about the platform question but I'm leaning towards making
it generic. Also the plugin folder has a space in the path which is annoying,
we should probably move it. Will pick this up again next week.
```

Every failure below maps to something #7 actually got right.

| Line | Failure | What #7 did |
|---|---|---|
| "good progress!" | progress theatre — unfalsifiable, and it sets no baseline for the next update | a table with five rows and five paths |
| "mostly done", "most of the S2C tools" | scope item 2 has no state and no location; "most" is unauditable | *"seven reports"*, listed, with the directory |
| "the positioning doc is coming together" | no path — a reader cannot check it | `docs/10-strategy/positioning.md` v2 |
| "leaning towards making it generic" | a recommendation written as a mood. It is scope item 3, the second-largest question in the issue, and it reads as neither `proposed` nor `accepted` | ADR-0006, with the nine-vs-nine evidence and an explicit verdict |
| "we should probably move it" | a decision meeting the ADR bar (packaging), buried mid-paragraph, with no options, no recommendation, no default, and no revisit trigger | ADR-0009, own numbered heading, at the end, with the immutable-`name` constraint recorded |
| *(absent)* | no not-reached block, though five vendors publish no documentation at all — a gap that changes how the benchmark should be read | *"Research limits, recorded rather than hidden"*, ending in an impact assessment |
| *(absent)* | no defects, though two were live at the time | two defect sections, one of them in the author's own reasoning |
| *(absent)* | nothing reported against the issue's five stated problems | the problems table |
| "next week" | the only forward-looking statement is a schedule, not a state, and it is the least reliable thing in the comment | phase 0, its three tasks, and why the timing matters |

The comment is not short — it is 70 words that convey less than any one row of #7's scope table.

---

## Template

```markdown
## Status — <one line naming both what is done and what is owed>

### Scope of work

| # | Item | Status | Where |
|---|---|---|---|
| 1 | <verbatim from the issue> | ✅ | `path/to/deliverable.md` |
| 2 | <verbatim from the issue> | ◐ | `path/`, partial — see below |
| 3 | <verbatim from the issue> | ⛔ | blocked on decision 1 below |

<!-- Four states: ✅ done · ◐ in progress · ⛔ blocked · ➖ dropped, with why.
     They answer: should the reader still expect a deliverable, and does
     anything need them? Every row carries a location. A ✅ without one
     is not a ✅. -->

### <N> — <item title>

Only for items where the table row is not enough. Numbered to match the issue.

---

## The problems in this issue, addressed

| Problem stated | Addressed by |
|---|---|
| <verbatim bullet from the issue> | <what, and where> |
| <verbatim bullet from the issue> | <or: not yet, and why — this is the valuable row> |

## Findings that change this issue's premise

<!-- Delete only if there genuinely are none. Name the premise in the issue's
     own words, name the evidence, state which way it actually runs, and say
     what it changes downstream. Never edit the issue body instead. -->

## Defects found

<!-- In shipped behaviour, in the schema, and in your own prior reasoning.
     Say what found each one. Separate the defect from the fact where they
     differ. -->

## Open — <N> decisions, not outstanding work

1. **<The choice.>** Options. Recommendation, with its reason. Why it was not
   executed. What happens if nobody decides. Any constraint that survives
   either outcome. If declined: **Revisit when** <named, observable trigger>.

## Still to do

<!-- Work, not decisions. Its own heading, deliberately. -->

## Not reached, recorded rather than hidden

- <what> — not reached: <how it failed> / out of scope per <boundary> / deferred to <when>
- **Does this block anything?** <yes, what — or no, and why not>
```

For a closing comment, add the completion test and its result, a commit or PR table, and the named successor location for anything that survives the close.

---

## References

Relative markdown links, not wikilinks: this repo is read on GitHub, which renders `[[foo]]` as literal text.

- [CONTRIBUTING.md](../../CONTRIBUTING.md) — §Research (*state what the pass could not reach*; *"unverified" is not "false"*) and §Document lifecycle, which this document's honesty rules extend to issue comments.
- [ADR index](../40-decisions/README.md) — *"`proposed` is not a soft `accepted`"*, and the rule that accepted decisions are never rewritten.
- [ADR-0009 — Repo and plugin layout](../40-decisions/0009-repo-and-plugin-layout.md) — the declined decision with a named revisit trigger, quoted throughout.
- [docs/README.md](../README.md) — *merged ≠ built*; the gap vs. out-of-scope distinction that the not-reached block depends on.
- [Writing issues](issues.md) — the `Problem` and `Scope of work` structure this document reports against, and why scope items are numbered.
- [Pull requests](pull-requests.md) — `Refs` vs `Closes`, and why closing is manual here.
- [Current state](../30-architecture/current-state.md) — the honest inventory a reported defect is filed into.
- [Roadmap](../20-product/roadmap.md) — where surviving work is rehomed at close.
- [Skill architecture](../30-architecture/skill-architecture.md) §7 — the migration ladder behind the foreseeable fifth status value and third open-items heading.
- [Commit messages](commit-messages.md) — the commit-body conventions that mirror these honesty rules.
- Issue [#7](https://github.com/Micheal-Friday/Trace/issues/7), both comments — the reference examples for every rule above.
