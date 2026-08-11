# Contributing

This repository holds the shared conventions, and **follows them**. A conventions repository whose own log breaks its rules is hard to argue with anyone about.

Read the guides for the rules — start at [`README.md`](README.md). This file records only what is specific to **this** repository.

**Pinned to:** n/a — this *is* the source. Adopting projects pin to `main` or to a tag.

---

## This project's values

### Commit scopes

A scope names the smallest part that ships, is reviewed, and is retired as one unit. Here that is a **directory**, not an individual guide: the guides cross-reference heavily, and a change to one is routinely reviewed alongside its neighbour — which fails the "reviewable on its own" test at file granularity.

| Family | Values |
|---|---|
| A guide directory | `git`, `github`, `docs` |
| The adoption surface | `templates` |

Omit the scope when a change spans the repository — a rule change that touches a guide, a template and the index has no honest narrower scope.

### Commit types in use

| Type | Means here |
|---|---|
| `docs` | the normal case. These documents *are* the artifact, so writing and editing them is `docs:` |
| `fix` | a rule that was **wrong**, not merely improvable — a correction someone following the old text would have got bitten by |
| `chore` | deletions, moves, index sweeps |

`feat` is deliberately unused. A new guide is still a document, and calling it `feat:` would imply a version bump that nothing here consumes.

### Branch types in use

`docs/` · `fix/` · `chore/`. No numbered record series yet, so no `adr/`.

### Labels

**None in use.** GitHub's defaults are present and unpruned; they will be curated the first time the issue list is long enough that filtering it is a real task. Adding a label before anyone needs to filter is how a taxonomy fills with values nobody applies.

### Where work is recorded

| Question it answers | Where |
|---|---|
| who is doing what, and when is it finished | GitHub issues |
| what was decided, and what it cost | **nowhere yet** — add `docs/decisions/` the first time a convention change has alternatives worth recording |
| what we want, and in what order | nowhere. There is no backlog; the issue list is short enough to be one |

---

## This project's exceptions

**The guides carry a `version:` in their frontmatter, which [document-lifecycle](docs/document-lifecycle.md) says documents should not.**

That rule exists because a version number on a living document is a lie about how it changes. Here the guides are **consumed by other repositories**, which may pin to a tag and need to know whether the text moved under them. That makes them closer to a shipped artifact than to an internal document.

The honest reading: this is the one place where the document/artifact line genuinely blurs, and the exception is recorded rather than quietly taken. If it turns out nobody ever pins, the versions should go.

---

## What this project cannot honestly enforce

| Convention | Reality here |
|---|---|
| **Author ≠ reviewer** | Aspirational at one maintainer. The substitute is a self-review pass plus a cooling-off period, and the change request says which it got |
| **Hold cross-cutting changes three business days** | Same. It applies once there is a second reviewer; until then it is a cooling-off convention, not a gate |
| **"Yes, if" review norm** | Adoptable now — it is a way of writing feedback, not a headcount requirement |

**Revisit this table whenever the team size changes.** Each row is a rule waiting for a precondition, not a rule that was rejected.

---

## Prior art that is not the model

Conventions here start on **2026-07-29**, which is also this repository's first commit — so unusually, **the whole log complies**. That will not be true of most projects adopting these, which is why the stub has this section.

The guides' worked examples come from the project these were extracted from, and its history predates the rules. Where an example is held up as *bad*, that is why.

---

## Changing a rule

1. **A rule change is a change to every project that follows it.** Say in the change request which projects it affects and whether any need to pin before it lands.
2. **Never edit a rule silently.** If the old text was wrong, `fix:` it and say what someone following it would have got wrong — that sentence is the whole value of the commit.
3. **Values do not belong here.** If a change adds a specific scope, label or path, it belongs in a project's own `CONTRIBUTING.md`. The test: could this sentence be false in another project? Then it is a value, not a rule.
4. **Keep the examples real.** Placeholders make a document portable and useless. An example drawn from a real history, labelled as such, is evidence the rule survived contact with something.
