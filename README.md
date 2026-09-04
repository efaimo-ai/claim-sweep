# claim-sweep

[![npm](https://img.shields.io/npm/v/claim-sweep?color=0b7285&label=npm)](https://www.npmjs.com/package/claim-sweep)
[![license](https://img.shields.io/badge/license-Apache--2.0-0b7285)](LICENSE)
[![grade](https://img.shields.io/badge/efaimo%20check--skill-A%20(100)-0b7285)](https://efaimo.ai/skills)
[![house-style](https://github.com/efaimo-ai/claim-sweep/actions/workflows/house-style.yml/badge.svg)](https://github.com/efaimo-ai/claim-sweep/actions/workflows/house-style.yml)

An Agent Skill for the moment a fact about your project changes and you have to
find everything that still says the old one.

Not the code change. The **sweep**: the README, the docs, the site copy, the
screenshot at the top of the README with the old number rendered into it, the
package you published last week, the test that asserts the old value, the check
you wrote to watch the claim and which now watches nothing, and the listing on
somebody else's site that scraped you in July.

<!-- generated:install -->

## Install

```sh
npx claim-sweep                 # into ./.claude/skills/claim-sweep/
npx claim-sweep --global        # into ~/.claude/skills/claim-sweep/
npx claim-sweep --check         # installed, and current?
```

The package is the skill: `SKILL.md` and its `references/`, nothing else. The
installer copies them, reads every byte back, and fails if what landed is not
what it wrote. It refuses to overwrite a directory whose contents differ unless
you pass `--force`, and installing the same version twice is a success rather
than a conflict.

Or take it by hand. It is markdown; `npx claim-sweep --print` writes `SKILL.md` to
stdout, and the repository is the whole thing.

<!-- /generated:install -->

## The shape of a sweep

```mermaid
flowchart TD
    F["one fact changes<br/><i>a version, price, grade, name, endpoint</i>"]
    F --> C["the code change<br/><b>this part is easy</b>"]
    F --> S["every artifact that still<br/>asserts the old value"]
    S --> R["README"]
    S --> D["docs and site copy"]
    S --> P["the published package,<br/>frozen at publish time"]
    S --> H["captured screenshots<br/>and recorded output"]
    S --> T["tests that pin the old value"]
    S --> G["the checkers that went<br/>vacuously green"]
    S --> L["third-party listings"]
    classDef easy fill:#0b728522,stroke:#0b7285;
    classDef hard fill:#c9282822,stroke:#c92828;
    class C easy;
    class R,D,P,H,T,G,L hard;
```

The hardest carriers are the last two: a checker that was watching the old value
now passes over nothing, and a listing you do not own keeps serving the old claim
to people who never see your repository.

## The problem

Retiring a claim is easy to do and hard to finish. Changing the fact takes a
minute; enumerating what was carrying it is the part that gets skipped, and each
survivor looks authoritative because the file it lives in was correct when it
was written.

Grepping for the old value is the obvious move and it finds the easy half. It
cannot see a number baked into a screenshot, a value inside a published tarball,
a count spelled as a word, a test that agrees with the bug, a checker that has
gone quiet, or a directory site that holds its own copy.

So this skill leads with a carrier list and searches each carrier on its own
terms.

## Install

Claude Code and other agents that read `SKILL.md` from a skills directory:

```bash
git clone --depth 1 https://github.com/efaimo-ai/claim-sweep \
  ~/.claude/skills/claim-sweep
```

Or vendor the directory anywhere your agent loads skills from. There is nothing
to build and no dependencies.

## What it contains

| file | what it is |
|---|---|
| `SKILL.md` | the procedure, and the carrier list in one line each |
| `references/carriers.md` | all eight carrier categories with the search that finds each |
| `references/guards.md` | how to make the next drift loud instead of silent |

`SKILL.md` is small on purpose. It is what an agent loads at trigger time; the
references load only when the sweep actually needs them.

## When it fires

A version, number, grade, price, date, default, endpoint, or name changes. Two
places disagree about the same figure. Someone asks whether a published claim is
still true. You are about to launch, release, rename, or announce a migration.

## The part worth reading even if you never install it

When you retire a claim, the tooling that was watching it does not start
failing. It starts **succeeding vacuously**: it searches for a sentence you
deleted, finds nothing, and reports zero problems. "Zero problems" and "nothing
was checked" produce identical output unless somebody wrote the difference in.

Make an empty harvest a failure. It is one line, and it catches more silent
breakage than anything else in `guards.md`, because subjects get renamed and
moved far more often than they get broken.

## Scope

This is a procedure, not a linter. There is nothing to run.

It tells you where a claim is **repeated**. It does not tell you whether the new
value is **correct** - deciding that is your job. What it makes sure of is that
the old one stops being published.


<!-- generated:pipeline -->

## What installing it does to a session

A skill is not free just because it is markdown. Its frontmatter is loaded at
the start of every session for every skill you have installed, whether or not it
ever fires.

```mermaid
flowchart LR
    N["npx claim-sweep"] --> D[/".claude/skills/claim-sweep/"/]
    D --> M["frontmatter<br/><b>every session, always</b>"]
    D --> B["SKILL.md body<br/><i>only when it triggers</i>"]
    D --> R["references/<br/><i>only if the agent reads them</i>"]
    M --> S(["your context window"])
    B -.->|"on trigger"| S
    R -.->|"on demand"| S
    classDef always fill:#c9282822,stroke:#c92828,stroke-width:1px;
    classDef lazy fill:#0b728522,stroke:#0b7285,stroke-width:1px;
    class M always;
    class B,R lazy;
```

In this skill's case, measured by [efaimo](https://github.com/efaimo-ai/efaimo) `weigh` (v0.5.0, 2026-09-04):
**92 tokens always resident**, 1,657 when it triggers, 2,647 across 2 reference files if the agent reads to the end.

<!-- /generated:pipeline -->

<!-- generated:set -->

## The set

Every skill in this set is about a report that was true about the wrong thing.

| skill | something reported | what the report was really about |
|---|---|---|
| [`red-before-green`](https://github.com/efaimo-ai/red-before-green) | a check said clean | whether it ran at all |
| [`denominator`](https://github.com/efaimo-ai/denominator) | a check said clean | how much of the world it saw |
| [`read-back`](https://github.com/efaimo-ai/read-back) | a write said done | whether it applied |
| **`claim-sweep`** | a change said done | everything else still asserting the old value |
| [`unreleased-guard`](https://github.com/efaimo-ai/unreleased-guard) | a document said true | which version it is true of |
| [`honest-chart`](https://github.com/efaimo-ai/honest-chart) | a picture said the data | whether its geometry is proportional |
| [`mcp-stateless-migration`](https://github.com/efaimo-ai/mcp-stateless-migration) | a server said ok | which revision it speaks |
| [`efaimo`](https://github.com/efaimo-ai/efaimo) | a tool said A(100) | what a grade certifies, and what it costs |

```mermaid
graph TD
    red_before_green["red-before-green"]
    denominator["denominator"]
    read_back["read-back"]
    claim_sweep["claim-sweep"]
    unreleased_guard["unreleased-guard"]
    honest_chart["honest-chart"]
    mcp_stateless_migration["mcp-stateless-migration"]
    efaimo["efaimo"]
    red_before_green --- denominator
    red_before_green --- read_back
    denominator --- claim_sweep
    read_back --- claim_sweep
    claim_sweep --- red_before_green
    claim_sweep --- unreleased_guard
    unreleased_guard --- red_before_green
    honest_chart --- red_before_green
    honest_chart --- read_back
    mcp_stateless_migration --- unreleased_guard
    mcp_stateless_migration --- red_before_green
    efaimo --- denominator
    efaimo --- mcp_stateless_migration
    classDef self fill:#0b728533,stroke:#0b7285,stroke-width:2px;
    class claim_sweep self;
```

Each edge is a real handoff, not a category: the reason one skill points at
another is written into it at [efaimo.ai/skills](https://efaimo.ai/skills), and
in the `Siblings` section of every `SKILL.md`. All of them are graded and
weighed by [`efaimo`](https://github.com/efaimo-ai/efaimo), the CLI that measures
what an agent loads.

<!-- /generated:set -->

## License

Apache-2.0. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).
