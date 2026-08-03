# The carriers

Every place a claim can be repeated, with how to search each one. Work the list
in order. The categories are ordered by how often they are missed, not by how
often they occur: the first two are always checked and the rest usually are not.

Throughout, `OLD` is the value being retired and `NEW` is its replacement.

---

## 1. Source of truth

The code, config, or data file that produces the fact.

There is usually exactly one, and finding it is the point: if you cannot name
the single place the value comes from, the claim is not derived anywhere and
every other carrier is a hand-typed copy. That is itself the finding, and
`guards.md` is about fixing it.

    grep -rn "OLD" --include='*.json' --include='*.yaml' --include='*.toml' .

## 2. Derived renderings

README, docs, website copy, `--help` output, error messages, changelog,
onboarding text, marketing pages, slide decks, the repository description
field, GitHub topics, the package description.

    grep -rniI "OLD" . | grep -v node_modules

Two traps:

- **Rounded and reformatted forms.** `1632` may appear as `1,632`, `~1.6k`,
  `about 1,600`. Search the digits without separators too.
- **Spelled-out numbers.** "thirteen tools" will not match `13`. Any
  digit-scanning check is structurally blind to these, so search words for
  small integers: `grep -rniE "\b(one|two|three|four|five|six|seven|eight|nine|ten|eleven|twelve)\b"`
  near the subject.

## 3. Captured output

Screenshots, recorded terminal sessions, architecture diagrams, social preview
cards, README images, video thumbnails, GIFs, badge images.

**Text search cannot see any of these.** They are the longest-surviving
carriers of a retired number, and they sit at the top of the README where they
are the first thing a reader sees.

- SVG is text: `grep -rn "OLD" --include='*.svg' .` works and is worth running
  first. (`-- '*.svg'` does not: it passes a literal filename, not a filter.)
- Raster images are not. List them and open them:

      find . -name '*.png' -o -name '*.jpg' -o -name '*.webp' | grep -v node_modules

- If an image was rendered from a source (SVG, HTML, a capture script), fix the
  source and **re-render**, then confirm the new file's dimensions match the one
  it replaced. A re-render at the wrong scale is a different defect arriving
  with the fix.
- Check whether the capture is still *representative*, not just whether the one
  number changed. Output formats gain lines. An old capture can be wrong by
  omission while every visible figure in it is correct.

## 4. Published artifacts

The released package, container image, binary, or hosted build.

**What is installed is a different fact from what is on the main branch**, and
public documentation is read against what is installed. If the branch is ahead
of the release, every doc describing branch behaviour is wrong for every reader
who follows it.

Unpack the published artifact and read it, rather than reasoning about it:

    npm pack <pkg>@<version> && tar -xzf <pkg>-<version>.tgz
    grep -rn "OLD" package/

    docker create --name tmp <image>:<tag> && docker export tmp | tar -t

Then answer explicitly: does the released artifact still assert OLD? If yes,
either the release ships or the docs must say which version they describe.
Writing "unreleased" beside the affected lines is a valid, honest fix.

## 5. Tests and fixtures

An assertion that encodes OLD is not a safety net. It is a second copy of the
bug, and it makes the code and the test agree, so the suite stays green while
both are wrong.

    grep -rn "OLD" test/ tests/ spec/ __tests__/

Two specific shapes to look for:

- A test asserting a **computed result** that only holds because of the old
  behaviour. Changing the fact makes this test fail, and the tempting fix is to
  update the expected number without asking why it moved. Ask why it moved.
- A **fixture built to exercise a rule that never fired.** If a fixture
  contains an input designed to trigger something, assert the trigger by name,
  not only by its effect on a total. A total is satisfied by any combination
  that sums correctly, including one where the thing you care about is absent.

## 6. Instruments

The gates, monitors, CI checks, alerts, and scheduled jobs that were watching
the claim.

When a claim is retired, an instrument watching it does not go red. It goes
**vacuously green**: it searches for a sentence that no longer exists, finds
nothing, and reports zero problems. Zero problems and nothing checked are
indistinguishable unless the instrument was written to tell them apart.

For each instrument that mentions OLD:

- Does it now assert NEW, or does it assert nothing?
- What does it do on an empty harvest? If the answer is "passes", that is a
  defect independent of this sweep. **An empty harvest should be a failure.**
- Does it match a literal phrasing that a rewrite would walk around? A check
  keyed to one wording is defeated by a paraphrase, silently.

Then break NEW on purpose and confirm the instrument goes red. Until you have
watched it fail, you have not established that it works.

## 7. Carriers you do not control

The ones you cannot fix by editing your repository.

- **Package registries** - the rendered README on the registry page is a
  snapshot taken at publish time. Editing the repository README does not change
  it. Only publishing does.
- **Aggregators and directories** - listing sites scrape once and re-scrape on
  their own schedule. Expect a stale version number, a stale date, and a
  description assembled from an old README.
- **Search indexes** - a retired page can rank for your own name for a long
  time. If you moved domains, check that old URLs redirect to something that
  exists: a path-preserving redirect into a 404 destroys the old page's standing
  instead of transferring it.
- **Link preview caches** - social platforms and chat apps cache the card. Most
  have a debugger that forces a re-fetch, and most require an interactive login.
- **Mirrors, forks, and vendored copies** of your docs.
- **Other people's writing** - posts, tutorials, answers. Usually not worth
  chasing, but worth knowing about before someone quotes it back at you.

For each: record the URL, what it currently says, and who can change it. This
is the category to *report* rather than fix, and reporting it is the deliverable.

## 8. Comments and commit messages

Prose describing behaviour that has changed.

    grep -rn "OLD" --include='*.*' . | grep -E ':[0-9]+:\s*(//|#|\*|<!--)'

(The second grep anchors after `path:line:`, because `grep -rn` prefixes every
line with them; anchoring on `^` there harvests nothing, silently.)

A comment that describes a fixed bug has become the bug. A comment asserting a
measurement ("this is 44px wider", "there are ten of these") is a claim like any
other and drifts like any other. Comments explaining *why* age well; comments
stating *how many* do not.

---

## Finishing

Write down, for each carrier category: checked / not applicable / out of my
control. A sweep with an unexamined category is not finished, and "not
applicable" is a decision worth recording so the next person does not re-derive
it.
