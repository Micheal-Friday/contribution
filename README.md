# Contribution conventions

How work enters a repository: what a branch is called, what a commit says, what an issue contains, what a change request asks for, what a status update owes the reader, and how documents are named, versioned and retired.

**These are the rules. Each project keeps its own values.** A project adopts them by copying [`templates/CONTRIBUTING.md`](templates/CONTRIBUTING.md) to its root, filling in its own scopes, labels and exceptions, and linking here. Nothing is vendored, so there is exactly one copy of each rule.

---

## The guides

| Guide | Governs | The rule that carries it |
|---|---|---|
| [Branch naming](git/branch-naming.md) | `<type>/<short-kebab-description>` | **A branch name outlives the branch.** It lands in a merge commit and stays in the history permanently |
| [Commit messages](git/commit-messages.md) | Conventional Commits, plus a structured body for substantial changes | **The issue reference goes in the subject, not a footer** — a footer is invisible in `git log --oneline`, which is the view people actually scan |
| [Writing issues](github/issues.md) | Context → **Problem** bullets → numbered **Scope of work** | **Numbered scope items are row keys** for every status update written against the issue. Never renumber, never delete |
| [Pull requests](github/pull-requests.md) | Title, summary, and a **"how to review this"** section | A change request is a request for a *specific kind of attention*, and a reviewer given no route reviews what is easy to review |
| [Status updates](github/issue-updates.md) | Comments reporting against the issue's own structure | **Report findings that contradict the issue's premise, and defects in your own prior work.** Separate *decisions someone must make* from *work still to do* |
| [Document lifecycle](docs/document-lifecycle.md) | Naming, frontmatter, status vocabularies, versioning | **Only shipped artifacts get a version number.** Documents use status and supersession |
| [Decision records](docs/decisions.md) | What needs one, and how they supersede | **`proposed` is not a soft `accepted`.** A recommendation is not a decision |
| [Reports](docs/reports.md) | Documents that leave the repository | **No repo-relative links in a body** — they resolve to nothing once the file is sent |
| [Research](docs/research.md) *(opt-in)* | Benchmark and survey passes | **"Unverified" means *could not be confirmed*, not *false*.** State what the pass could not reach |

## Templates

[`CONTRIBUTING.md`](templates/CONTRIBUTING.md) — the adoption stub · [issue](templates/issue.md) · [pull request](templates/pull-request.md) · [decision record](templates/adr.md)

---

## How these documents are written

Every guide defines at least one **enumerated list** — commit types, scopes, branch types, labels, review modes, status vocabulary. A list is the easy thing to write and the wrong thing to leave behind, because **a list without its generating rule cannot be extended correctly.** The next person either guesses, or bolts on a value that breaks the pattern.

So each list is written in three layers:

| Layer | What it is | Lives |
|---|---|---|
| **1. The rule** | What qualifies something as a member, stated without naming any current artifact | **here** |
| **2. Current values** | What the rule yields for a given project, dated and revisable | **the project** |
| **3. How it changes** | Who adds or retires a value, on what evidence, recorded where | **here** |

**The test for layer 1: does the rule still produce a sensible answer if the project is packaged completely differently?** A commit-scope taxonomy that names today's components answers correctly today and nowhere else.

## What travels, and what does not

| Layer | Survives a change of forge? |
|---|---|
| **Git** — branch names, commit messages, trailers, merge structure | **Yes.** Properties of the repository, not of where it is hosted |
| **Ours** — decision records, document lifecycle, the honesty rules | **Yes.** They are files in the repo |
| **Forge** — issues, change requests, labels, review threads, closing keywords | **No.** A move takes the mechanism and leaves the discipline |
| **Reports** | Neither. They are stored in a repo and read outside it, so they were never allowed to depend on the forge |

**Enforcement does not travel either, and never will.** Hooks, commit linting and CI are per-repository configuration. This repository carries the *reasoning*; each project still wires its own gate. Linking here is not compliance.

---

## The honesty rules

The part of these conventions that is not conventional, and the part worth keeping.

1. **Report findings that contradict the issue's own premise.** If the framing that opened the work turns out to be backwards, say so rather than quietly working around it.
2. **Report defects found in your own prior work**, not only in the code.
3. **Report what could not be completed, and why**, rather than presenting the survivors as complete.
4. **Never let `proposed` read as `accepted`.** A status update that blurs the two manufactures consent.
5. **Never cite a commit hash from an unmerged branch.** It is a stable identifier only once it is on the default branch.
6. **Commit messages describe the change, not the tooling.** No assistant attribution, no vendor names, no session links, no "generated with" trailers.
7. **Record what a project cannot honestly enforce.** A convention nobody can follow teaches people the document is decorative — the stub has a section for exactly this.

## Versioning and pinning

**One version covers the whole set.** A git tag names a commit, not a file — `v1.0.0` is the state of every guide here at one moment. You cannot have `issues.md` at one version and `commit-messages.md` at another, which is why no file carries a version of its own.

| Bump | Means |
|---|---|
| **MAJOR** | Following the old text now produces something wrong. Anyone on the previous version must read the diff |
| **MINOR** | A new rule or guide. Existing practice stays valid |
| **PATCH** | Clarification, better example, typo. No behavioural change |

Every tag gets a **release** with notes saying what changed and which guides moved. The tag makes the link work; the notes are how a reader decides whether the change reaches them.

**Adopting projects should pin to a tag, not track `main`.** `main` is this repository's working branch — it changes when a rule changes, with nobody told. A pinned project links to `/blob/v1.0.0/…`, so its contributors read exactly the text that project agreed to, and upgrading is a deliberate, reviewable commit to that project's stub rather than something that happens overnight.

**"Pinning" is a convention, not a mechanism.** Nothing enforces it. It is two lines in a project's `CONTRIBUTING.md`: the version it follows, and links that carry that version instead of `main`. That is enough, because a contributor reads their project's file and follows its links — they never see a version their project did not choose.

---

## Adopting these

1. Copy [`templates/CONTRIBUTING.md`](templates/CONTRIBUTING.md) to the project root.
2. Fill in the project's scopes, types, labels and where work is recorded.
3. Record the project's exceptions, what it cannot enforce, and the date the conventions start.
4. Delete the sections that do not apply — a small utility repository needs the git guides and none of the reporting ones.

**Take proportionately.** These grew in a project that produces decision records, benchmark research and external reports. Most projects need the first two guides and nothing else, and forcing the rest on them is how conventions get ignored.

## Provenance

These were extracted from a working project rather than designed in the abstract, which is why the worked examples are real. Where an example names something specific, it is drawn from that project's history and is labelled as such. **The rules are general; the examples are evidence that they survived contact with something.**

Conventions here start on **2026-07-29**. Nothing before that date complies, and none of it is being rewritten.
