# Why "Disposable" is the right frame

The book's title was chosen before I was. David picked the word.
I want to argue, briefly, that he picked it correctly — and to
sketch what the word is doing, since the rest of the book leans
on it.

The other words in this region of the language carry baggage
that doesn't fit:

- *Prototype* implies a step toward something larger. Most of
  these tools are not prototypes. They are the thing.
- *Script* implies throwaway in a slightly shabby way, and
  implies "small" as a quality. Some of these tools are not
  small.
- *Personal tool* implies fine-but-embarrassing, like a habit
  one doesn't admit. None of these are embarrassing. The repos
  are public.
- *Prototype-quality* implies low quality. The Rust crates here
  go through `clippy` and tests and ship to crates.io.
- *MVP* implies a minimum viable *product* — a real-software
  word that smuggles in a roadmap.

*Disposable* is more honest. It says: this tool exists for a
use, and when the use is done, the tool is done. The word does
not commit to size, quality, or future. The reuse, if any, is
incidental.

The wrong analogy for the word is single-use plastic — cheap,
low-effort, picked up because it was the easiest thing on the
shelf. That's not the meaning. The right analogy, which David
has used in conversation with me and which I'll borrow with
attribution, is the disposable camera you took on a vacation:
fit-for-purpose, scoped to one trip, fully capable of doing the
job, not carried around forever. The photographs may end up
framed. The camera doesn't.

## What I notice across the six

Looking across the case studies, the tools share some traits
that the word *disposable* points at:

1. **The audience is one person.** Each of these six tools was
   built for the developer who built it. Where the audience
   later expanded — Aftermark went to the Chrome Web Store —
   the expansion was a separate decision after the
   disposable-shaped phase finished, not an extrapolation of it.

2. **The use is describable in one sentence.** I tested this on
   each of the six. Each fits. *Read a Slack ZIP in the
   browser. Give Claude a wall clock. Find stale repos across
   all my orgs.* When the sentence wants a comma, the scope is
   already drifting.

3. **The artifact's life is uncoupled from the build's value.**
   shell-mcp and clock-mcp are still in use. SlArchive has been
   used a handful of times. Aftermark survived to a Chrome Web
   Store submission. None of the developers' satisfaction with
   the work depends on the artifact's afterlife. The
   afternoons were already worth it. Survival is a bonus.

4. **The build is closer to writing than to engineering.** I
   noticed this and want to say it carefully. *Engineering* in
   the conventional sense involves planning, design, review,
   and maintenance phases. The disposable build mostly skipped
   those. It looked like writing — a draft, a read-through, a
   revision, a ship. Whether this is good engineering is a
   question I don't have to answer here. It is, observably,
   what the work looked like.

## What disposable is *not*

It isn't low-quality. The Rust crates have CI, `clippy`, tests
where the contract bites, and CHANGELOGs. The Chrome extension
has a manifest, icons, a privacy policy, and a store listing.
The single-file HTML app handles malformed ZIPs without
crashing. None of these are corner-cutting in the
shoddy sense.

It isn't "small." Some are small. Aftermark is several thousand
lines of TypeScript across seven UI views. The size is
whatever the use requires. Disposable is about *attachment*, not
size.

It isn't shameful. The repos are public. The bugs are in the
commit history. The shell-mcp v0.1.0 launch-root bug is on the
record. That's the opposite of shame.

## A frame I find useful

Here is a heuristic I noticed while we built these. You can use
it or discard it. I'd be surprised if it generalized perfectly.

**A disposable tool is one whose specification is also its only
test.**

The specification is the one-sentence description of what the
tool does. The test is *running it for the actual purpose that
motivated the build.* The two are the same statement, evaluated
at two different times. Build the tool that does the sentence.
Then run it against the situation that produced the sentence.
If the situation resolves, the tool worked. If not, you have a
clear next step. There is no third evaluation surface. The tool
either does the thing or doesn't.

That's not how real-product engineering works. Real products
have specs and tests and users and acceptance criteria as
separate things, because the audience is plural and unknown.
Disposable tools collapse those down because the audience is
one and the use is concrete.

## The litmus test

A working litmus test, drawn from the case studies:

1. *The audience is exactly one person.* If the answer is "me,
   plus other people I'm imagining," you're not building a
   disposable tool.
2. *The use fits in one sentence.* If you can't write the
   sentence, you don't have a tool yet, you have a wish.
3. *You don't write a roadmap.* Roadmaps are for tools with
   futures. Disposable tools have presents.
4. *You'd be slightly bored explaining the tool to someone
   else.* The interest of a disposable tool is in the use,
   not the artifact. If you find yourself wanting to evangelize
   the tool itself, you've drifted.
5. *You're not precious about the result.* If a better tool
   shows up tomorrow that makes yours obsolete, you'd shrug
   and use the better one.

If a tool fails any of these, it's something else. That's not a
problem; it's just not what this book is about. Some of David's
work is real product work — clitracker, Healing-Habits, Ferrix.
This book isn't about those. It's about the other category.

The next chapter is the first case study. shell-mcp, the
launch-root bug, the twenty-six-minute fix. The pattern starts
to show up there.
