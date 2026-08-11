---
type: index
status: living
version: 1.1
updated: 2026-07-29
tags: [contribution, process, index]
aliases: [contribution guides, workflow conventions]
---

# Contribution — the workflow record

How work enters this repository: what a branch is called, what a commit says, what an issue contains, what a PR asks for, and what a status update owes the reader.

These documents are **not style pedantry**. Each one exists because the artifact it governs is the only durable record of something. A commit message is the sole record of a change that travels with the change — `git log` is in every clone, offline, forever, while PR threads live on a server someone else owns. An issue's numbered scope items are the row keys of every status table written against it later. A branch name lands in a merge commit and is permanent: `Merge pull request #6 from Micheal-Friday/skill/inquiry` is in this history for good, and `skill/inquiry` does not say what it did.

---

## The six

| Document | Governs | The rule that carries it |
|---|---|---|
| [Branch naming](branch-naming.md) | `<type>/<short-kebab-description>` | **A branch name outlives the branch.** Issue numbers stay out — the platform cross-references from commit trailers, not refs — but ADR numbers stay in, because an ADR has no automatic backlink |
| [Commit messages](commit-messages.md) | Conventional Commits 1.0.0, plus this repo's structured-body pattern | **The issue reference goes in the subject, not a footer** — it is the only position `git log --oneline` shows. `refs #N` while any of the issue's scope is open; `closes #N` only when the merge completes all of it |
| [Writing issues](issues.md) | Context → **Problem** bullets → numbered **Scope of work** | **Numbered scope items are row keys.** Never renumber, never delete — mark dropped, for the same reason ADR numbers are never reused |
| [Pull requests](pull-requests.md) | Title, summary, and a **"how to review this"** section | A change request is a request for a *specific kind of attention*, and different changes need different attention |
| [Status updates](issue-updates.md) | Comments that report against the issue's own structure | **Report findings that contradict the issue's premise, and defects in your own prior work.** Separate *decisions someone must make* from *work still to do* |
| [Writing reports](reports.md) | `R<NNN>-<subject-slug>-<YYYY-MM-DD>.md`, and the mandatory section structure | **A report is a document that leaves the repository.** So it carries no repo-relative links, cites sources as plain text, and must be readable with no access to this repo |

---

## How these documents are written

Every one of them defines at least one **enumerated list** — commit types, scopes, branch types, labels, review modes, status vocabulary. A list is the easy thing to write and the wrong thing to leave behind, because **a list without its generating rule cannot be extended correctly.** The next person either guesses, or bolts on a value that breaks the pattern.

So each list in this folder is written in three layers, and any new one must be too:

| Layer | What it is | Why |
|---|---|---|
| **1. The rule** | What qualifies something as a member, stated without naming any current artifact | This is the durable part. It must survive the project being repackaged |
| **2. Current values** | A table of what the rule yields today, dated and marked revisable | Concrete enough to use immediately, honest about being a snapshot |
| **3. How it changes** | Who adds or retires a value, on what evidence, recorded where | Without this, layer 2 silently becomes the definition again |

**The test for layer 1: does the rule still produce a sensible answer if the project is packaged completely differently?** This one is not hypothetical. [`skill-architecture.md`](../30-architecture/skill-architecture.md) §7 already plans five rungs — a plugin today, then a git workspace, then a bundled MCP server over local SQLite, then a remote server over a shared database, then an application built on the Agent SDK. [`platform-strategy.md`](../10-strategy/platform-strategy.md) adds per-business-unit packs on top. A commit-scope taxonomy that names *skills* and *the plugin* answers correctly at rung 0 and nowhere else.

**Examples stay concrete; only definitions become general.** Every worked example in these guides is a real commit, issue, or PR from this repository. Replacing them with placeholders would make the documents portable and useless.

---

## What is platform-specific, and what is not

Worth separating, because the two decay at different rates:

| Layer | Examples | Survives a platform move? |
|---|---|---|
| **Git** | Branch names, commit messages, trailers, merge structure | **Yes** — these are properties of the repository, not of where it is hosted |
| **Ours** | ADRs, the docs tree, the numbered-scope convention, the honesty rules | **Yes** — they are files in the repo |
| **Platform** | Issues, pull requests, labels, review threads, `Closes #N` autolinking | **No** — a move to another forge takes the mechanism and leaves the discipline |
| **Neither** | **Reports** | They are stored here and read elsewhere. A forge move does not touch them, because they were never allowed to depend on the forge in the first place |

If the forge ever changes, [Branch naming](branch-naming.md) and [Commit messages](commit-messages.md) carry over unchanged, and so do [Reports](reports.md). The other three need their *mechanism* remapped and keep their *structure*: an issue is still context → problems → numbered scope, whatever the tool calls it.

---

## How they fit together

```
issue  ──▶  branch  ──▶  commits  ──▶  change request  ──▶  merge  ──▶  report
  │                          │                │                          │
  └──── status updates ──────┘                └──▶ Closes, or not        └──▶ leaves the repo
```

A report is the only artifact here that is read outside the repository, which is why it is the only one forbidden from linking into it.

Four boundaries decide where a given sentence belongs, and getting them wrong means writing the same thing three times:

| Artifact | Answers |
|---|---|
| **Commit message** | What changed, and why now |
| **ADR** | What was decided, what was rejected, and what it costs |
| **Change-request body** | How to review this, and what is deliberately not in it |
| **Issue** | What work exists, and how anyone can tell it is finished |

The commit and the ADR are in the git object store; the issue and the change request are on somebody else's server. **When something must survive the platform, it goes in a commit, an ADR, or `docs/`** — not in a comment thread.

---

## The honesty rules

These are the part of this folder that is not conventional, and they are the part worth keeping. Each is drawn from something that actually happened here rather than from a template.

1. **Report findings that contradict the issue's own premise.** Issue #7 framed one business unit as the incumbent and the other as a future adopter. The evidence said the reverse. The status update said so rather than quietly working around it.
2. **Report defects found in your own prior work.** `CONTRIBUTING.md` shipped an exemplary commit message for a plugin move that was subsequently declined — the example instructed people to do the opposite of the decision. That was found while writing these guides and corrected in the same pass.
3. **Report what could not be completed, and why.** Seven research agents died on a session limit. The research index records exactly which areas that left uncovered rather than presenting the survivors as complete.
4. **Never let `proposed` read as `accepted`.** ADR-0009 sat at `proposed` for a day with a clear recommendation attached. A recommendation is not a decision, and a status update that blurs the two manufactures consent.
5. **Commit messages describe the change, not the tooling.** No assistant attribution, no vendor names, no "generated with" trailers. Commit `73fa6e0` predates this rule and carries such a trailer; **it is not being rewritten.** A history edited to look tidy is worth less than one that is accurate.
6. **Never cite a commit SHA from an unmerged branch.** A SHA is a stable identifier only once it is on `main` — before then the commit can be amended, rebased, or squashed out of existence, and a rebase-merge rewrites every SHA on a branch even when a fast-forward was possible. Cite a pull request or issue number, a SHA already on `main`, or a description of the commit. **This one is written from being caught by it:** these guides cited two unmerged commits as worked examples in 42 places across four files, which quietly made the repository's merge strategy hostage to its own documentation. Full statement in [Commit messages](commit-messages.md).

---

## What this repo cannot honestly enforce

Recorded because a convention nobody can follow is worse than none — it teaches people the document is decorative.

| Convention | Reality here |
|---|---|
| **Author ≠ reviewer** | Aspirational at one maintainer. The honest substitute is a **self-review pass plus a cooling-off period**, and a change request that states which of the two it actually got |
| **Hold cross-cutting changes three business days** | Same. It applies once there is a second reviewer; until then it is a cooling-off convention, not a gate |
| **"Yes, if" review norm** | Genuinely adoptable now — it is a way of writing feedback, not a headcount requirement |

**Revisit this table whenever the team size changes.** Each row is a rule waiting for a precondition, not a rule that was rejected.

## Prior art that is not the model

These conventions start from **2026-07-29**. Earlier history does not comply and is not being rewritten:

- Five of ten commits predate Conventional Commits — `first commit`, and `trace-inquiry:` used as a pseudo-type.
- PRs #1 and #6 use prose titles rather than the commit grammar.
- Branch `skill/inquiry` uses a type that is not in the list.
- Commit `73fa6e0` carries assistant attribution trailers.

None of it is wrong in retrospect; it is simply from before the rule. **Cite these documents for what to do next, never as a description of what the log already looks like.**

---

## Scope

These documents govern the **workflow**. [`CONTRIBUTING.md`](../../CONTRIBUTING.md) at repo root governs the **content** — file naming, the document lifecycle, what needs an ADR, the rules for changing a skill, the leak test, and research conventions. It links here rather than restating any of it, so there is one copy of each rule.

Templates for issues and change requests live inside [Writing issues](issues.md) and [Pull requests](pull-requests.md) as fenced blocks. They are **not** installed into `.github/` — doing that changes what everyone sees when they open an issue in the platform UI, which is a decision rather than a formatting choice. Adopting them is a copy-paste away when that decision is made.

### A note on links

**Wikilinks do not render on GitHub.** `[[foo]]` displays as literal text and is not clickable, and it fails silently — nothing errors, the link simply stops being one. Since this repo is read on GitHub as well as in an editor:

- In `## References` sections and any other list of links, use **relative markdown links with real paths** — they work in both.
- Inline in prose, wikilinks are acceptable; they degrade to readable text.
- **Never write a wikilink followed by a markdown link** (`[[foo]](path)`). That is malformed and renders badly everywhere.

---

## References

- [`CONTRIBUTING.md`](../../CONTRIBUTING.md) — content conventions
- [ADR-0001 — Record architecture decisions](../40-decisions/0001-record-architecture-decisions.md) — why decisions live in ADRs rather than in issues
- [`skill-architecture.md`](../30-architecture/skill-architecture.md) §7 — the five packaging states these conventions must survive
- [`platform-strategy.md`](../10-strategy/platform-strategy.md) — core and per-business-unit packs
- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) · [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/) · [SemVer 2.0.0](https://semver.org/)
