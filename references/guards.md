# Guards

A sweep fixes today. A guard is what stops the same claim drifting again.

Ordered by strength. Prefer the highest one the situation allows, because each
step down adds a copy that a future one-sided fix can miss.

---

## 1. Derive it, so there is no second copy

The strongest guard is that the value exists once and everything else computes
from it.

If a version appears in five surfaces, four of them should read the fifth. If a
date is "when this last changed", take it from the version control system
rather than typing it. If a count is "how many of these there are", count them.

Applies to more than numbers: route lists, supported platforms, and feature
tables are all derivable from the thing they describe. A hand-written list of
routes does not fail when it falls behind; it just silently covers less.

## 2. If it must be duplicated, make one copy assert the other

Some copies are unavoidable. A published registry page cannot import from your
repository. A generated report cannot import from a sibling project.

Then the guard is a check that reads both and fails on disagreement. This is
weaker than deriving, because it can be deleted, but it is loud.

The failure mode to design against: **independent re-derivation does not catch
a shared definition error.** Two places that compute the same wrong formula
agree perfectly. If the copies are two implementations of one definition, pin
the *output* as a literal in one place, so the check compares against something
that cannot drift in step with the thing it is checking.

## 3. Make the empty harvest a failure

Any check that finds its subject by searching must fail when it finds nothing.

    if (!matches.length) fail("found no X to check; the format changed or the subject moved");

A check reporting "0 problems" and a check that examined nothing produce
identical output unless you write the difference in. This single rule catches
more silent breakage than any other, because subjects get renamed and moved far
more often than they get broken.

The same rule applies to generators, not only to checks: a generator that
produces an empty result and reports success will overwrite a real artifact
with an empty one.

## 4. Match on shape, not on one phrasing

A check keyed to an exact sentence is defeated by a rewrite, silently, and the
rewrite is usually innocent.

Prefer matching the *shape* of the claim: a grade-like token, a number followed
by a unit, any line containing both the subject and a digit. Then assert
something about the match. If you must match a phrase, assert that the phrase
still occurs at all, so a rewrite fails loudly and gets a deliberate decision
rather than silence.

## 5. Prove the guard can fail, before trusting it

Break the input on purpose. Watch the check go red. Put it back.

A guard that has never failed has not been shown to work. This costs about a
minute and is the only step that distinguishes a guard from a decoration.

Two specific traps:

- **Check the exit code of the thing you ran**, not of a pipeline. In a shell,
  `tool | tail` reports `tail`'s status, so a failing tool reads as success.
- **A guard can pass for the wrong reason.** If the check went green because
  the sabotage did not actually reach it, you have proven nothing. Confirm the
  failure message names the thing you broke.

## 6. Put the guard where the claim lives

A guard in the repository that owns the data does not watch the repository that
publishes it. If copy in project A quotes a measurement from project B, the
check has to be able to see both, which means it belongs wherever that is
possible, even if that is neither of them.

Ask: which artifacts does this guard need to read at once? Put it where they
are all visible. A guard that cannot see the carrier cannot watch it.

---

## Choosing

Derive when you can. Cross-check when you cannot. Always make empty a failure.
Always prove it can fail. And write down which guard covers which claim, so the
next sweep starts from a list instead of from memory.
