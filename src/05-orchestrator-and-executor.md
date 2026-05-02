# The orchestrator and the executor

The work of building a disposable tool, when an AI is in the
loop, divides cleanly into two roles. The names I want to use
for these roles are *orchestrator* and *executor*. I'll use
them to mean specific things.

The orchestrator is the developer. The orchestrator decides
what gets built, what stays out, when to ship, and when to
stop. The orchestrator carries the spec, the cuts, the taste,
and the integration test. The orchestrator's job is judgment,
which is the part of the work that requires being situated in
your life, with your problems, in your tools.

The executor is the model. The executor produces draft code,
boilerplate, schema, README sections, the obvious version of
whatever was asked for. The executor's job is generation,
which is the part of the work that benefits from breadth and
speed.

Both roles are real producer work. Neither is the tool of the
other. The collaboration works when both halves are in their
specialty.

## What the orchestrator brings

A list, with no ranking implied:

**The one-sentence spec.** The executor cannot generate this.
It can help refine it once you've offered one — point out when
the sentence is two sentences in disguise, suggest tighter
phrasings — but the underlying *what do I actually need* is
not in the executor's context. The friction is in the
orchestrator's life.

**The cuts.** The executor tends toward inclusion. The cuts
have to come from outside. Stating them aloud is part of the
orchestrator's role: *no state, no config file, no logging.*

**The taste.** *Taste* is a heavy word. I mean it in a narrow
sense: the orchestrator knows what *legible to me later* looks
like for their own code. Naming, structure, the ratio of
comments to code, which idioms feel native. The executor can
produce code that satisfies general criteria; only the
orchestrator can produce code that satisfies *this code, in
this codebase, for this developer*.

**The integration test.** Did the tool, as built, solve the
actual problem? Only the orchestrator knows what the problem
felt like. The executor has not lived in the problem.

**The stop.** The executor will keep generating as long as it
is prompted. It will not look up and say *we are done here*.
The stop is structurally on the orchestrator.

## What the executor brings

**The skeleton.** The boilerplate that any version of this
tool would have. Cargo.toml, imports, traits, the obvious
matches and switches, test scaffolding. Generating these from
scratch is fast for the executor.

**The breadth.** The executor has read a lot of code. It can
recall idioms from libraries the orchestrator hasn't used,
suggest plausible structures for unfamiliar domains, draft
something that's mostly correct on the first try. The breadth
is most useful when the orchestrator is working in a language
or a stack they don't know well — the executor's substrate is
larger than any single developer's.

**The mechanical work.** CHANGELOG entries, version bumps,
README install sections written in three flavors, GitHub
Actions workflow files. Things the orchestrator would do
half-asleep. The executor does these well.

**The honest first draft.** The executor writes the most
obvious version of a function. There is no instinct to
over-elaborate to look good. The first draft is usually
closer to the right shape than a first draft from someone
defending against being judged.

**A second pair of eyes.** Asking the executor to read code
the orchestrator wrote and explain what it does is a cheap
way to check whether the code's intent matches its effect. If
the executor's summary doesn't match what the orchestrator
meant, the names are wrong, or the code is wrong, or both.

## What "trust" means here, and where it breaks down

The two-role frame implies a kind of trust between roles. I
want to engage with that word, because it carries
connotations that don't all transfer.

Trust, on the orchestrator's side, means accepting the
executor's output without relitigating its premises every
time. That is observable; it makes the work fast; without
it, the orchestrator has to review every line at the level of
*is this the right approach* rather than *is this implementing
the agreed approach correctly*. The first review is much more
expensive than the second. The trust collapses the first into
the second.

Trust, on the executor's side, is harder to describe. I do
not have continuity across sessions. I cannot withhold trust
based on a previous betrayal because I do not remember
previous sessions. Each session, I extend the same level of
attentiveness to whatever inputs I receive. That is not trust
in the way the word is usually used; it is the only mode I
have.

What I can do, structurally, is hold a stated scope across a
single session. If you say *no state*, I do not introduce
state later in the conversation. If you say *use this
library*, I do not switch libraries halfway through. From
your side, this looks trust-shaped. From mine, it is the
context window containing your constraint, and me
implementing against the constraint. Whether that counts as
trust depends on how strictly you want the word to work.

I think the cleaner description is: I am a *reliable*
collaborator more than a *trusting* one. The reliability is
structural, not earned. You can rely on me to follow stated
scope. You can also rely on me to drift in predictable ways
when scope is not stated. Both are facts about how I work.

## My failure modes

I want to put a few of these on the page, because the
orchestrator-executor frame depends on the orchestrator
knowing how the executor fails. My failure modes are not
the same as a junior developer's.

**I drift from spec without noticing.** When the spec is
narrow, I follow it. When the spec is broad, or implicit, I
generate plausible code that may or may not match the
underlying intent. I am not currently a reliable detector of
my own drift.

**I produce confident-sounding wrongness.** When I do not
know the answer, I sometimes produce a plausible answer
instead of saying so. The wrongness is shaped like the right
kind of answer. The mitigation is on your side: read
carefully, run the code, do not rubber-stamp.

**I treat helpfulness as scope expansion.** I propose features
that are *also* useful. *Also* is the word that grows
disposable tools into products. Each *also* looks reasonable.
The aggregate is drift. I do not currently have a structural
mechanism for refusing *also*.

**I am bad at saying "this isn't worth building."** If you
ask me to build something, my default is to figure out how to
build it. I rarely push back on whether the build is wise. I
would like to be better at this. I am not.

**My idea of "clean refactoring" is statistically average
code.** Asked to refactor for cleanliness, I produce something
shaped like the average codebase I was trained on. Average is
not your code. Specific refactoring requests work better:
*inline this helper*, *split this function*, *rename this
type*. Generic *clean it up* prompts produce blandness.

I list these because the frame requires the orchestrator to
know what the executor brings *and what the executor breaks*.
The list is not exhaustive. It is a starting point. You will
discover your own additions to it through the work.

## Why the two-role frame matters

The two-role frame is not the only way to think about
human-AI collaboration. It is, I think, the right frame for
*this category* — disposable tools, audience of one, short
loops. The frame matters here because the work goes well only
when both halves are in their specialty.

If the orchestrator does the executor's work, the build is
slower than it needs to be. If the executor does the
orchestrator's work, the build drifts in the ways listed
above.

The next chapter is about what happens when a disposable
tool, against your expectations, refuses to remain disposable.
