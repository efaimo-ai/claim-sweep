# The one a sweep cannot find: the value that was never named

Everything in the procedure assumes a fact changed and left copies of its old
value behind. There is a sibling failure with the same symptom and no old value
to search for, so a sweep runs clean while the problem sits in the open.

It happens when the authoritative page names a **category** instead of a
**value**. "We will pin the dependency." "We will use a queue." "The retention
window is configurable." Each reads like a settled decision and none of them
decides anything. Downstream, two documents each pick a value, each written by
someone who believed the question was closed upstream, and neither cites the
other. Both are internally consistent. Nothing contradicts the canonical page,
because the canonical page said nothing.

You cannot grep for this. There is no stale string; there is an absent one. It
surfaces only when someone reads two downstream documents in the same sitting,
which can take months, or when one of them is executed.

**The tell** is a decision sentence whose object is a kind of thing rather than
a thing. Read your canonical page and, for each decision, ask whether you could
act on it without asking a follow-up question. "Pin the dependency" fails,
because the next question is "to what version". "Pin lodash to 4.17.21" passes.

**The fix** is a rule about decision records rather than a search: a decision
records the value, not the category. If the value is genuinely undecided, that
is fine and it belongs in the open-questions list rather than in the decided
one, where its presence is what makes two readers disagree in confidence.

**Before you sweep**, confirm the value was ever named. Sweeping for a changed
value when nothing was ever decided finds zero carriers and reports success,
which is the same vacuous green the procedure's instruments step warns about,
arriving from the other direction.
