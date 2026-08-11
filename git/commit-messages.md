---
type: process
status: living
version: 2.0
updated: 2026-07-29
tags: [contribution, git, commits, conventional-commits, process]
aliases: [commit messages, commit conventions, conventional commits]
---

# Commit messages

Detail behind the one line about commits in the adopting project's own `CONTRIBUTING.md`. A commit message is the only record of a change that travels with the change. PR threads live on a server, issues get closed, and an ADR records a decision rather than a diff — but `git log` is in every clone, offline, forever.

---

## 1. The form

[Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/):

```
<type>[optional scope][!]: <issue reference> <description>

[optional body]

[optional footer(s)]
```

Blank line between each part. That blank line is structural, not cosmetic — git treats the first line as the subject and everything after the first blank line as the body, and every tool that reads commits relies on it.

**The issue reference sits in the subject, not in a footer.** This is a deliberate departure from the common practice of trailing `Refs #7` at the bottom, and the reason is the view people actually use:

```
$ git log --oneline
0b283b3 docs: refs #7 forbid citing SHAs from unmerged branches
dd10dae docs: refs #7 add reports, and the convention that governs them
```

`git log --oneline` prints **subjects only**. A footer is invisible there, and in `git shortlog`, in most blame views, and in the forge's branch list. An issue reference that can only be found by opening each commit in full is a reference nobody follows — which defeats the point of writing it. Put it where the scanning eye already is. See §6.

### The version mapping

| Marker | Bump |
|---|---|
| `fix:` | PATCH |
| `feat:` | MINOR |
| `!` after the type/scope, or a `BREAKING CHANGE:` footer | MAJOR |

**This is the one list in this guide with no local generating rule, deliberately** — it is fixed by Conventional Commits and SemVer, and changes when those standards change, not when a project decides. A locally extended bump mapping would make the changelog unreadable by every tool that reads other repos'.

**The mapping governs exactly the files that carry a version — in most projects one manifest for the released artifact, and nothing else.** [Document lifecycle](../docs/document-lifecycle.md) draws that boundary: only the released artifact gets SemVer. Strategy documents are living and carry status only; decisions use status and supersession; research carries `researched:` — see [research conventions](../docs/research.md). So a `docs(strategy):` commit never moves a version number, no matter how large it is, and the source project's docs-tree commit — forty files, a rewritten product definition — was correctly a `docs:` commit that bumped nothing.

Two consequences worth stating:

- **`feat(strategy):` is a category error.** Features live in the released artifact. A change to a docs section is `docs:` even when it proposes a new capability.
- **Below 1.0, `!` bumps MINOR, not MAJOR.** While the artifact is in `0.y.z`, SemVer §4 makes anything in it changeable at any time. Mark the break anyway — the marker is what makes the changelog entry correct later, and the changelog entry belongs in the same PR as the bump.

---

## 2. Types

### The rule

**A type names what the change does to the released artifact** — one word, read by a person deciding whether to open the diff and by tooling deciding whether to move a version. A candidate qualifies only if all three hold:

| Test | Reason |
|---|---|
| **It names an effect no existing type names** | Two types a reader cannot tell apart are one type with a spelling problem. `feat` and `fix` differ by whether the behaviour is new or was already owed |
| **Its version consequence is stated** | The type is the input to §1's mapping. Without one, every changelog entry becomes a judgement call |
| **The work it names exists here now** | A type coined for anticipated work decays into a synonym for `chore` |

Draw from the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) vocabulary unless an ADR says otherwise — a non-standard type is invisible to every tool that parses a log.

### Current values — as of 2026-07-29, revisable

| Type | Use |
|---|---|
| `feat` | new capability in the released artifact — a component, a field it writes, an output contract |
| `fix` | a defect in shipped behaviour |
| `docs` | anything under `docs/`, plus root `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md` |
| `refactor` | structure changes, behaviour does not |
| `chore` | maintenance that is none of the above: deletions, manifest bumps, index sweeps |

`test`, `build`, `ci`, and `perf` are valid Conventional Commits types held in reserve. **They stay unused for as long as the thing they name does not exist** — a repo with no test suite and no CI has no use for `test` or `ci`. They fail test three, not test one, so they become available the day that thing exists, with no further decision.

### How the list changes

| Event | Procedure |
|---|---|
| Adopting a type from the Conventional Commits set | PR to this guide, landing with the first commit that needs it. No ADR — the vocabulary and its bump are already standard |
| Adding a type outside that set | ADR. It changes how the log is parsed downstream, which [the ADR bar](../docs/decisions.md) treats as a packaging decision |
| Retiring a type | PR to this guide, naming what carries the work now. **History is never rewritten to match** — old commits keep the retired type, because it was correct when written |

The type is the branch type too — [branch-naming.md](branch-naming.md) §2 — but a branch may carry several: `chore/remove-supplier-quest` could reasonably carry a `docs(decisions):` commit alongside its `chore:` one.

---

## 3. Scopes

### The rule

**A scope names the smallest part of the system that ships, is reviewed, and is retired as one unit.** Three tests, all of which must pass:

| Test | Reason |
|---|---|
| **It has a boundary you can point at** — a directory, a manifest, a deployable | The name is checkable rather than invented, and `git log --grep` keeps working against something that exists |
| **It can be reviewed on its own** — a change inside it is judged without opening a second part | A scope is a claim about blast radius. If the reviewer must look elsewhere anyway, the scope lied |
| **It will still be a name in a year** | The scope is written into permanent history. A name that tracks the current packaging expires with it, leaving the log describing a shape the repo no longer has |

The negative is where the mistakes are: **a scope is not a layer, an area, or an activity.** `frontend`, `skills`, `refactor` all fail test two. Same fault as `skill/` in a branch name — [branch-naming.md](branch-naming.md) §2. Note that the rule names no artifact, no component and no docs section; it merely yields those today.

### Current values — per project

The values this rule yields are the adopting project's own, and they belong in its `CONTRIBUTING.md` rather than here. The shape is stable even where the names are not: one value for the released artifact, taken from its manifest; one per component that ships and is reviewed on its own — a package, a service, a skill directory; one per docs section that is superseded on its own. Scope names are the directory or manifest name exactly — `vendor-search`, not `search` or `vendorSearch` — and section scopes drop any numeric prefix: `strategy`, not `10-strategy`.

### Choosing one

**The narrowest scope that contains the whole change.** A scope that does not contain the whole change is worse than no scope, because it tells the reader the diff is smaller than it is.

| Change touches | Scope |
|---|---|
| one component | that component — `fix(vendor-search):` |
| two components, a shared reference file, or the artifact manifest | the released artifact |
| one docs section | that section — `docs(strategy):` |
| two or more docs sections | omit |
| docs and the artifact together | omit |

**Omit the scope when the change genuinely spans the repo.** Both of the source project's substantial commits do: the docs-tree commit touched every docs section, and the prototype-removal commit spanned a deletion, an ADR, two indexes, and the changelog. An invented umbrella scope would have been a lie about the blast radius.

### How the list changes

| Event | Procedure |
|---|---|
| A new value falls out of the rule — a component, a docs section, an extension pack | It is a scope the moment its directory or manifest lands. Add the row in that PR. No ADR: nothing was decided, the rule already covered it |
| A new *family* appears, or one stops shipping independently | ADR — see [the ADR bar](../docs/decisions.md). Anything changing how the product is packaged, distributed, or extended qualifies |
| A value stops matching the rule | Retire it in a PR, saying what replaced it. **History keeps the old scope**; it was true when written |

### The test the values have to pass

Run the rule against the packaging the project expects to have next, not only the one it has today. That is how this rule was tested, and a rule that only answers correctly at the current rung is exactly the flaw it was written to fix.

The generic form of the move: a project shipping as a single distributable yields one scope for the artifact plus one per component inside it. Give it independently owned extensions and each is a scope the moment it ships without core approval — test two passing exactly. Split the artifact into a client and a server that build, version, and deploy separately, and the rule yields one scope per deployable without being edited — while the original umbrella scope is *retired*, because it named the whole product only for as long as the product was one thing.

That last move is the point. **The rule survives; the values do not.** Which is why this section defines a rule and dates its list rather than shipping the list alone.

---

## 4. The subject line

| Rule | Reason |
|---|---|
| **The issue reference comes first, after the colon** | `refs #7` or `closes #7`, then the description. It is the only position visible in `git log --oneline` — see §1 and §6 |
| **Imperative mood** | It matches what git itself writes — `Merge pull request #6`, `Revert "…"`. A log that mixes moods reads as two authors |
| No trailing period | It is a title, not a sentence. The period costs a character in a line that is already truncated by every UI that displays it |
| Lower case throughout the reference and after it | `refs #7 re-derive`, not `Refs #7 Re-derive`. Proper nouns keep their case — `docs: refs #7 adopt Conventional Commits for the log` is correct |
| **≤ 72 characters including type, scope and reference** | `git log --oneline`, the forge's branch view, and every terminal at 80 columns truncate past that. The reference costs about nine characters, so descriptions are correspondingly tighter |
| One change per subject | A subject that needs a comma-separated list is usually a commit that needs splitting — see §9 |

The test — **run it on the description, with the reference removed**:

> **If applied, this commit will _______.**

`docs: refs #7 re-derive product definition, feature set, and roadmap` → *"If applied, this commit will re-derive product definition, feature set, and roadmap."* Reads. `docs: refs #7 updated the roadmap.` → *"If applied, this commit will updated the roadmap."* Does not.

**The one cost, stated because it is real:** a changelog generator that reads Conventional Commits will render `refs #7` as part of the description text. Where nothing generates a changelog automatically — `CHANGELOG.md` written by hand, as in the source project — the cost is zero. If that changes, the alternative is the scope slot, `docs(#7): re-derive …`, which keeps the description clean and stays visible in `--oneline`. It is a swap, not a rewrite.

---

## 5. The body

| Body | When |
|---|---|
| Not needed | typo fixes, broken links, a date correction, a mechanical rename where the subject is the whole story |
| One paragraph | the subject is clear but the *reason* is not obvious from the diff |
| **Structured, with named sections** | any change a reviewer would need more than a minute to reconstruct — a new component, a schema change, an ADR resolution, a multi-section docs change |

Wrap at **72–76 columns**. `git log` indents the body by four spaces, and 80-column terminals still exist.

### The house pattern

The distinguishing convention here. In the source project, the docs-tree commit uses the sections *Positioning* / *Benchmarks* / *Platform question* / *Record*; its prototype-removal commit uses *Removed* / *Plugin path*. The skeleton:

```
<type>(<scope>): <subject>

<Opening paragraph, prose. What was wrong, or what changed in the world,
and why this is happening now. Not a summary of the diff — the diff is
right there.>

<Section name>
- <what changed, and the reason it changed that way>
- <what changed, and the reason it changed that way>

<Section name>
- <...>

<Closing paragraph. What this deliberately does not include, or what it
leaves open, and where that is tracked.>
```

Rules for the sections, each with its reason:

| Rule | Reason |
|---|---|
| **Bare capitalised phrases on their own line.** No `#`, no `**`, no trailing colon | `git log` renders plain text. Markdown syntax in a terminal is noise, and half the tools that display commits do not render it |
| Name them after their subject matter | *Positioning*, *Benchmarks*, *Plugin path* are things a reader can be looking for. *Changes*, *Details*, *Notes* name nothing and can be deleted without loss |
| Two to five sections | More than five and you are describing several commits. Go to §9 |
| Every bullet carries its reason | *"Removed `Supplier Management App/` — a throwaway exploration, and the capability it sketched belongs in the product proper"* survives. *"Removed `Supplier Management App/`"* is already in the diff |
| **Close by stating what is not included** | The single most useful line in either of those two commits. The docs-tree commit's closing sentence, quoted in full in §10, is how its reader knows the boundary was chosen rather than missed |

Cite, do not restate. The prototype-removal commit says *"ADR-0009 proposed moving … Declined for now"* and gives the reason in three lines; the options analysis stays in the ADR. See §8.

---

## 6. Footers

Conventional Commits footers are git trailers: `Token: value`. The token replaces spaces with `-` — `Co-authored-by` — with `BREAKING CHANGE` as the one exception. One footer per line, blank line before the block, no trailing period.

**The rule: a footer exists only where something keys on it** — a forge that autolinks it, a changelog generator that reads it, an attribution system that acts on it. A trailer nothing reads is prose in a costume; one pointing at a private or expiring resource is worse than none. That single test generates the current values below (2026-07-29, revisable) and rules out everything in §7. **To add one:** name what reads it, in the PR that introduces it. If nothing reads it, it is a sentence in the body.

**The issue reference is the one thing that is deliberately not a footer here.** It is well-formed as a trailer and it works — but a trailer is invisible in `git log --oneline`, which is where people actually look. It lives in the subject instead; see §1 and below.

### `refs` and `closes` — in the subject

| Reference | Meaning | Effect on the forge |
|---|---|---|
| `refs #7` | this commit is part of the work on #7 | cross-references. Leaves the issue open |
| `closes #4` | this commit completes #4 | closes the issue on merge to the default branch |

**Use `refs` while any part of the issue's scope is still open. Use `closes` only when the merge genuinely completes it.**

In the source project, every commit on the product-definition PR uses `refs #7`: each shipped a real part of the work and none completed the scope, so none was entitled to close it. the prototype-import commit and the tone-modes commit used `closes #4` and `Closes #2` correctly — those issues were done.

The asymmetry is deliberate. An issue closed early stops collecting the work still owed to it, and nobody re-reads a closed issue. An issue closed late costs one click.

**Both work identically in the subject.** The forge autolinks `#N` anywhere in a commit message, and it honours closing keywords in the subject exactly as it does in a footer. Nothing is lost by moving them up, and the reference becomes visible in the one view that gets used most.

**Every commit still needs one.** A commit with no issue reference is a commit nobody can place six months later. If the work genuinely belongs to no issue, that is usually a sign the issue was never opened — see [Writing issues](../github/issues.md).

### Citing another commit

**Never cite a commit SHA from an unmerged branch** — in a commit body, a doc, an ADR, or a PR. A SHA is a stable identifier only once it is on `main`. Before then the commit can be amended, rebased, or squashed out of existence, and a rebase-merge rewrites every SHA on the branch even where a fast-forward was possible.

Cite instead, in order of preference: a **PR or issue number** — `#8` — which is permanent from the moment it is opened; a **SHA already on `main`**; or a **description** — *the docs-tree commit*, *the prototype-removal commit*. These guides learned it the hard way: they cited two unmerged commits as worked examples in forty-two places across four files, and unpicking that was the price of being free to choose a merge strategy at all.

The rule is about *when*, not *whether*. Inside its own repository a merged SHA is the most precise citation there is, and a project's own `CONTRIBUTING.md` should use them freely.

**These guides cannot, and the reason is the second half of the same rule: a SHA resolves only where its history is.** The worked examples below come from the project this was extracted from, so they are named — *the spec-class-gate fix*, *the prototype-import commit* — rather than hashed. A hash here would be a citation nobody reading this repository could follow, which is the exact failure the rule exists to prevent, arrived at from the other direction.

The general form: **cite a SHA only where the reader has the history.** Within a repository, that is anything merged. Across repositories, it is nothing.

**When you must identify a specific commit on a live branch, use its subject line.** A PR body's `Commits` table is the case that forces this: it describes an unmerged branch by definition, and a reviewer needs to know which commit is which. **A rebase rewrites every hash and preserves every message**, so the subject survives exactly the operation that destroys the hash. That is why the template in [Pull requests](../github/pull-requests.md) keys its table on the subject rather than on a `sha` column.

The honest cost, stated once: a merged SHA lets a reader run `git show` and get the whole commit in one step. `#8` plus a description takes two. That is the price of the rule, and it is worth paying — a citation that takes two steps beats one that resolves to nothing.

### `BREAKING CHANGE:`

```
BREAKING CHANGE: `qualification.stage` is replaced by `relationship.stage`
and a plural `qualifications[]`. No persisted data exists to migrate.
```

Uppercase, `: ` separator, and it says both **what breaks and what to do about it**. Pair it with `!` in the subject so the break is visible in `--oneline`. Released artifact only — see §1.

### `Co-authored-by:`

```
Co-authored-by: Name <email@example.com>
```

**Real human co-authors only** — someone who wrote part of the change or pair-authored it. The forge attributes the commit to both people, which is the entire point, and which is why attributing it to anything that is not a person is wrong.

---

## 7. The record is of the change, not the tooling

**No AI assistant attribution, no vendor names, no model names, no session links, no "generated with" trailers.**

This is not a style preference. One commit in the source project's history carries trailers of this shape:

```
Co-Authored-By: <Assistant Name Model-Version> <noreply@vendor.example>
<Vendor>-Session: https://…/session_<opaque-id>
```

Each of these three fails the §6 rule — nothing reads them — and each is sufficient on its own:

- **The session URL resolves for exactly one person**, and only until it does not. A permanent record pointing at a private, expiring resource looks like a lead and is not one.
- **It dates the record.** A model version will read like `IE6` within a year, on a commit whose actual content — the spec-class gating rule — is still correct and still load-bearing.
- **It answers a question nobody asks of `git blame`.** The question is always *why is this line like this*. What editor, keyboard, or assistant produced it has never been part of the answer.

the spec-class-gate fix's body is otherwise a good example of the house style; the trailers are the only thing wrong with it. They are retired by this rule going forward, and **the commit is not being rewritten.**

The same rule applies to noise in the other direction: no `[skip ci]` theatre, no ticket-tracker URLs where a `refs #N` will do, no emoji.

---

## 8. Commit vs ADR vs PR

The most duplicated writing in any of these guides. Three artifacts, three questions, three lifespans:

| | Answers | Lives | Read by |
|---|---|---|---|
| **Commit** | *what changed, and why now* | forever, immutable, in every clone | someone running `git blame` or `git log -S` on a line that surprised them |
| **ADR** | *what was decided, what was rejected, and what it costs* | forever, superseded rather than edited | someone asking "why is it like this" before proposing to change it |
| **PR** | *how to review this* | until merge | one reviewer, this week |

The rules that fall out:

| Rule | Reason |
|---|---|
| **The commit cites the ADR; it does not re-argue it** | The prototype-removal commit gives the packaging decision in three lines and the reason in one, and lets the ADR carry the options analysis. Re-arguing it means two records that can disagree |
| **The ADR does not describe the diff** | [Decision records](../docs/decisions.md) §2: *ADRs cite the strategy, product, and research documents; they do not restate them.* Same principle downward. A diff described in an ADR is stale the first time the code moves |
| **Nothing that must survive goes only in the PR body** | PR bodies are not in the git object store. A clone with no network has the commits and the docs and nothing else. If a rationale matters in a year, it is in the commit or the ADR |
| **The PR body is for the reviewer** | What to look at first, what you are unsure about, what you tested, which of the two markets you exercised. See [pull requests](../github/pull-requests.md) |
| **"Why now" belongs to the commit alone** | The ADR records the decision, not the timing. The docs-tree commit's opening paragraph — the business had changed shape, not just market — is why *that week*, and it appears nowhere else |

Applied to the prototype-removal commit: it says the prototype is gone and why it was deleted rather than restored; the layout ADR §*Prototypes* says the `prototypes/` directory is withdrawn and what the general lesson is; the PR said which files to check. No sentence appears twice.

---

## 9. When to split

Split when any of these is true:

| Signal | Reason |
|---|---|
| **You would not want to revert the whole thing** | A commit is the unit of revert. If half of it is safe and half is not, it is two commits |
| **Something in it is not decided yet** | The real case, below |
| A rename or move travels with a content change | Git detects renames by similarity. Change the content in the same commit and the diff stops showing a rename and starts showing a delete plus an add |
| A schema change travels with the component that uses it | **Schema before skill**, where the adopting project's own `CONTRIBUTING.md` says so: the schema and its ADR land first, so the change can be reviewed against a settled contract |
| The subject needs commas to list what it did | See §4 |

**The real case.** In the source project, the docs tree (forty files) and the prototype deletion were deliberately kept apart. The docs-tree commit's closing paragraph says so: *"the deletion of `Supplier Management App/` is left unstaged pending a decision."*

The reason is the one worth internalising: **an undecided deletion bundled into a forty-file docs diff is an invisible deletion.** No reviewer would have caught a removed prototype among thirty-six new markdown files, and the deletion needed its own argument — throwaway exploration, capability belongs in the product proper, recoverable from history. That argument is the whole body of the prototype-removal commit; bundled, it would have been one bullet in a wall of text.

The layout ADR §*Prototypes* states the general form: **a deletion sitting unstaged in the working tree is the only state that loses information**, because the intent is recorded nowhere. Commit the removal with its reasoning, or restore the files.

Do not split past the point where a commit stands on its own. A commit that leaves `docs/README.md` pointing at a file that does not exist yet is not a smaller unit — it is a broken one.

---

## 10. Worked examples

All four are from the source project, abridged, with its own names left in place.

### Good

**One — a structured body with a stated boundary** (the docs-tree commit):

```
docs: re-derive product definition, feature set, and roadmap

The product was five skills for finding and vetting electronics vendors in
one market, and is now expected to serve an asset-light manufacturing
intermediary that outsources all production to job shops. That is a
structurally different business, not just a different market, and it broke
parts of the existing product definition.

Positioning
- The product is the supply-side intelligence layer of the Source pillar.
  It answers four questions and refuses the rest: who could make this, what
  should it cost from the market, what do we know about them and on whose
  word, and did they deliver what they said.

Benchmarks
- Across eight source-to-contract suites and seven e-sourcing tools, nobody
  closes the loop between a stated requirement and a received answer.

The packaging move proposed in ADR-0009 is not included here, and the
deletion of Supplier Management App/ is left unstaged pending a decision.
```

No scope, because the change spans every section. Named sections a reader can navigate. A closing paragraph that marks the boundary as chosen. `refs`, not `closes`, because the issue was not finished.

**Two — a decision recorded alongside the change it caused** (the prototype-removal commit):

```
chore: remove Supplier Quest prototype and close out layout decisions

Resolves the two decisions left open by the product-definition work.

Removed
- Supplier Management App/ — a single-file Kanban CRM added in 38c8f75 as a
  test app. Deleted deliberately rather than restored: it was a throwaway
  exploration, and the supplier-management capability it sketched belongs in
  the product proper rather than in a parallel HTML prototype. Recoverable
  from history if ever wanted.

Packaging path — left as is, knowingly
- ADR-0009 proposed moving the plugin directory because the marketplace
  source path contains a space. Declined for now: the packaging is expected
  to change substantively at the next rung, and re-organising a directory
  that is already scheduled for re-organisation is churn.

ADR-0009 moves from proposed to accepted, with both outcomes recorded in
place rather than as a follow-up. Indexes and changelog updated to match.
```

The deletion carries its own justification and its recovery path. The declined move cites the ADR rather than re-arguing it.

**Three — a defect fix** (the spec-class-gate fix, with a type added and the tooling trailers removed per §7):

```
fix(vendor-search): gate vendors on spec class, not just product

Searches like "small gear manufacturer" were returning large-gear makers
because the capability gate only checked product-vs-process, treating any
gear maker as a match. Small and large gears are effectively different
products made by different factories.

Extend the capability gate to two parts: Part A product match (as before),
Part B spec-class match against the vendor's verified production envelope
(size/dimensional range, precision/quality grade, material class, volume
model). A right-product-wrong-class vendor is gated to Capability <=2 and
capped at Conditional, the same as a process generalist.

- Principle 2 now covers defining specs, not just process
- Phase 3 requires verifying the production envelope, not just the category
```

Symptom, then cause, then the rule that replaces it. The scope is the one component that changed.

### Bad

| Message | Why it fails |
|---|---|
| `Add Supplier Quest: single-file Kanban CRM for supplier management #4` (the prototype-import commit) | Three faults: no type; the bare `#4` trails the subject as decoration rather than sitting after the colon as a reference, so it reads as part of the title; and `closes #4` is buried in the middle of the body, where it works by luck rather than by convention. The number being *in* the subject is right — everything about how it got there is wrong |
| `vendor-inquiry: synthesize loose phrases, formal/friendly modes, prefer prose over lists` (the three-changes-in-one commit) | 86 characters. No type — `vendor-inquiry` is a scope wearing the type's position. Three changes comma-separated in one subject, which is the §9 signal to split. And the body is empty, on a change that plainly needed one |
| `docs: reflect vendor-inquiry synthesis + formal/friendly modes in README` (the mirror-edit commit) | Type and scope are fine; the problem is that it exists. A README edit whose only purpose is to mirror the commit before it is not an independent revertible unit — it belonged in that commit |
| `first commit` (the initial commit) | No type, no information, no body. The one message everybody forgives and nobody should repeat |
| `fix: fixed the customs bug.` | Past tense, trailing period, and no referent — the project's defect list names five known defects and this names none of them |
| `docs(strategy): bump the artifact to 0.2.0 and revise positioning` | Two faults: a version bump is not `docs`, and the scope contains neither half of the change. Split it — the bump is a `feat` on the artifact scope with a changelog entry, the revision is `docs(strategy):` |
| `feat(skills): add vendor-compare` | `skills` is a layer, not a unit that ships or is retired on its own — §3 test two. The scope is the released artifact, or `vendor-compare` once its directory exists |

---

## 11. Before you commit

- [ ] Subject is `<type>[(scope)][!]: <description>`, imperative, lower case after the colon, no trailing period, ≤ 72 characters.
- [ ] *"If applied, this commit will …"* completes cleanly.
- [ ] Type is in the §2 table — or you added it there in this PR, having run it past the three tests.
- [ ] Scope is one your project's `CONTRIBUTING.md` lists and is the narrowest one containing the **whole** change — or omitted because the change spans the repo.
- [ ] `feat`/`fix`/`!` used only where the released artifact actually changed, and its manifest plus `CHANGELOG.md` move in the same PR if it did.
- [ ] Body present if the reason is not obvious from the diff; structured with named sections if the change is substantial.
- [ ] Body wraps at 72–76 columns and states what is **not** included.
- [ ] Anything that needed a decision has an ADR, and the commit cites it rather than re-arguing it.
- [ ] Subject carries `refs #N` right after the colon — or `closes #N` only if this merge genuinely finishes the issue.
- [ ] Any commit cited by SHA is already on `main`; anything still on a branch is cited as `#N` or described.
- [ ] `BREAKING CHANGE:` says what breaks and what to do.
- [ ] No tooling attribution, session links, vendor names, or generated-with trailers.
- [ ] `git diff --cached --stat` matches the message. Nothing rode along; nothing decided-but-undecided is hiding in a large diff.

---

## References

- The adopting project's own `CONTRIBUTING.md` — the base convention, its scope values, and the rules for changing a skill
- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) · [Semantic Versioning 2.0.0](https://semver.org/) · [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/) — the external standards that close §1's mapping
- [Document lifecycle](../docs/document-lifecycle.md) — the SemVer boundary §1 depends on, §3, and the naming rules behind §3's scope names
- [Decision records](../docs/decisions.md) — the ADR bar §2 and §3 escalate to, and rules 2 and 3, which set the commit/ADR boundary in §8
- [Research conventions](../docs/research.md) — the dated status field research carries instead of a version
- [Branch naming](branch-naming.md) — the same type vocabulary, applied to refs
- [Pull requests](../github/pull-requests.md) — what the PR body carries that the commit does not
- [Writing issues](../github/issues.md) · [Status updates](../github/issue-updates.md) — where `#N` comes from, and when an issue is finished
- The repository index — the three-layer contract every list in these guides is written to
