---
name: claim-sweep
description: Use after a fact about a project has changed, to find every place that still states the old value - a version, price, limit, count, supported platform, default, endpoint, command or product name, left behind in docs and site copy, help text, tests and fixtures, screenshots and diagrams, the published package, CI checks that watched the old value, and listings you do not control. Also use when two project documents disagree about the same value, or before announcing a rename, migration or domain move. Not for dependency bumps or general fact-checking.
license: Apache-2.0
metadata:
  version: "0.2.0"
  homepage: "https://efaimo.com"
  verified_against: "2026-09-03"
---

# claim-sweep

A claim is any statement a project makes about itself that could stop being
true: a version, a benchmark number, a supported platform, a price, a grade, a
spec date, a default, a command, a hostname.

Changing the fact is easy. The failure is never the change. **The failure is
that nobody enumerates what was carrying the old value**, so the fact changes
in one place and stays changed in nine others, and each survivor looks
authoritative because it is sitting in a file that was correct when it was
written.

Retiring a claim is a sweep, not an edit.

## Why the obvious approach misses

Grepping for the old value finds the carriers that store it as text. It misses
every carrier that stores it some other way, and those are the ones that
survive longest:

- a number rendered into a **screenshot** or a diagram is not text
- a value baked into a **published package** is not in the working tree
- a number spelled as a **word** ("thirteen") does not match a digit search
- a **test** asserting the old value makes the code and the test agree, so
  nothing goes red
- a **checker** written to watch the claim goes blind the moment the claim
  flips, and reports green about a sentence that no longer exists
- a **third-party listing** is not in your repository at all

So: enumerate carriers first, then search each one on its own terms.

## Procedure

1. **State the claim in one sentence, old value and new.** "The free plan
   allows 1,000 requests a day" becomes "it allows 2,000". Vague claims
   cannot be swept.

2. **Enumerate the carriers before searching.** Walk the list in
   `references/carriers.md`. Write down every carrier that exists for *this*
   project, including the ones you do not control. Do not skip a category
   because you doubt it applies; deciding it does not apply is the finding.

3. **Search each carrier on its own terms.** Text search for text. Open the
   images. Unpack the published artifact. Read the tests. Check the
   spelled-out forms and the rounded forms, not just the exact string.

4. **Ask what was watching this claim.** Any check, gate, test, or monitor
   written to assert the old fact is now either failing for the right reason
   or passing for the wrong one. Both need attention, and the second is
   silent. This step is skipped almost every time.

5. **Fix, then prove the fix could have failed.** Break the new value on
   purpose and confirm something goes red. A guard you have never seen fail is
   not a guard.

6. **Propose the guard that would make the next drift loud**, per
   `references/guards.md`, and add it when the user wants it. If a value is
   derivable, derive it. If it must be duplicated, make one copy assert the
   other.

7. **Report what you did not fix.** Third-party listings, caches, and other
   people's copies are usually out of your control. Name them and say who has
   to act.

## Report

End with one line per carrier category, so a skipped category is visible:

    source of truth      checked    config/plans.json:4
    derived renderings   fixed      README.md:31, docs/limits.md:12 ("1k/day")
    captured output      manual     public/og-card.png shows 1,000; re-render
    published artifacts  checked    npm 3.2.0 README still says 1,000
    tests and fixtures   fixed      test/plans.test.js:9
    instruments          re-aimed   scripts/check-docs.mjs matched the old sentence
    not controlled       reported   partner pricing page (owner: partner)
    comments             checked    none

If you cannot search the files yourself (no shell, no repository), give the
user this list with the search to run for each line.

## The carriers, in one line each

Full detail with commands in `references/carriers.md`.

1. **Source of truth** - the code or data that produces the fact.
2. **Derived renderings** - README, docs, site copy, help text, error strings.
3. **Captured output** - screenshots, terminal captures, diagrams, social
   cards. Text search cannot see these.
4. **Published artifacts** - the released package, image, or binary. What is
   installed is a different fact from what is on the main branch.
5. **Tests and fixtures** - an assertion encoding the old value makes the bug
   and the test agree.
6. **Instruments** - the gates and monitors that were watching the claim.
7. **Carriers you do not control** - registries, aggregators, search indexes,
   preview caches, mirrors, other people's posts.
8. **Comments and commit messages** - prose describing behaviour that changed.

## The step everyone skips

When you retire a claim, the tooling that watched it does not start failing.
It starts **succeeding vacuously**. A check that greps for a sentence you
deleted finds nothing, and "nothing found" is usually written as a pass.

After any claim flip, list the instruments that mention the old value and
decide, for each one: does it now check the new claim, or does it check
nothing? An instrument that harvests zero matches should be red, not green.
That rule is worth applying to every checker you own, not just the ones
involved in this sweep.

## When nothing was ever named

If the canonical page names a kind of thing ("we will pin the dependency")
instead of a value, there is no old string, and a sweep comes back clean.
Check that first: `references/never-named.md`.

## Scope

A sweep finds where a claim is repeated. Whether the new value is right is a
separate decision: when two sources disagree, settle which one is canonical
first, or ask.

<!-- generated:siblings -->

## Siblings

Every skill in this set is about a report that was true about the wrong thing. The set: https://efaimo.com/skills

- `red-before-green` - step 5, and the reason a guard you have never watched fail is not a guard.
- `unreleased-guard` - when the fact that changed is a version, and the gap between the tree and the registry is what made the copy wrong.
- `denominator` - for the instruments in step 4: a checker that went vacuously green is one whose denominator fell to zero.

<!-- /generated:siblings -->
