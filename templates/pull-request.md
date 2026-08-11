<!--
  Pull request template. Full rules: github/pull-requests.md

  Title: same Conventional Commits grammar as a commit, including the issue
  reference — e.g.  docs: refs #12 restructure the decision record
-->

## Summary

<!-- What changed and why, in prose. Two to five sentences for a small change;
     two short paragraphs for a large one. The commit bodies carry the detail —
     do not restate them here. -->

## How to review this

<!--
  Include only the lines this change's cost drivers earn — volume, blast
  radius, irreversibility, unfalsifiable assertion. A three-line fix earns
  none of them: say "the diff is the review" and stop.

  A reviewer given no route reviews what is easy to review, and misses the
  argument.
-->

**Time:** ~N minutes.
**Start here:** `<path>` §N.
**The load-bearing claim:** <the one thing that, if wrong, invalidates the rest>.
**Argue with:** <what genuinely needs a second opinion>.
**Skim:** <what is bulk, and where its findings are summarised>.
**Do not review:** <moved, generated, or verbatim files>.

## Commits

<!-- Multi-commit changes only. Key on the commit SUBJECT, not the hash — a
     rebase rewrites every hash and preserves every message. -->

| Commit | Contents | Why separate |
|---|---|---|
| `<subject>` | | reversibility / type / mechanical |

## Not included

- <what a reviewer might reasonably expect and will not find, and why>

## Known defects shipped knowingly

- <the defect, its mechanism, and where it is tracked>

## Outward-facing changes

<!-- Anything named from outside, on a resolution path, or drawn from a shared
     budget. Or "none". -->

## Review received

<!-- A second reviewer, or: self-review pass plus a cooling-off period.
     Say which. Do not imply a review that did not happen. -->

## Links

Refs #N
<!-- `Closes #N` ONLY if this merge completes the issue's whole scope,
     including any open decisions. If using Refs, say who closes the issue. -->

<!-- Decision-record status changes in this change: ADR-NNNN proposed → accepted -->
