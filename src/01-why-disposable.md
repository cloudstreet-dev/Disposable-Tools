# Why "Disposable" is the right frame

The word does work. Pick a different one and the whole posture
changes.

Call them *prototypes* and you imply they're a step toward something
larger — a working version, a productized version, the real thing
later. Call them *scripts* and you imply they're throwaway in a
shabby way, the kind of thing you don't admit to writing. Call them
*personal tools* and you imply they're fine but small and a bit
embarrassing, like a toothbrush you'd rather not show people. None
of these is right.

*Disposable* is right. Disposable says: this tool exists for a use,
and when the use is done, the tool is done. There's no shame in the
ending. The tool was never trying to be anything other than what it
is. If it gets reused later, fine — but the reuse is a bonus, not
the design. If it gets thrown away, also fine. The work was the use.

You'll hear the word "disposable" and reach for the wrong analogy.
The wrong analogy is single-use plastic — cheap, low quality, here
because it was the easiest thing on the shelf. That's not what we
mean. The right analogy is the disposable camera you took on a
vacation: deliberate, fit-for-purpose, scoped to one trip, capable
of doing exactly the job, and you don't carry it around forever.
Some of the photographs on it might end up framed. The camera does
not.

## The question that changed

For most of my career, the question I asked before building a tool
was: **is this worth building?** That question carried hidden
assumptions. It assumed building cost a weekend. It assumed I was
trading writing-the-tool against the rest of my life. It assumed I'd
have to maintain whatever I shipped. It assumed someone might find
it later and judge me by it.

The question now is different. The question is: **what was stopping
me?**

That's not a rhetorical question. I have to actually answer it,
because most of the time the honest answer turns out to be small.
Friction. A vague feeling that this isn't a real project. An
internalized sense that bespoke tooling is for senior people at
real companies, not me, sitting at my kitchen table on a Saturday
morning with a cup of coffee and a Slack export I want to read.

When the cost of building drops below the cost of putting up with
the absence, the calculation flips. You can run that calculation
several times a week now. Most days I run it at least once.

## What disposable is *not*

It's not low quality. The Rust binaries in this book go through
`clippy`, run their tests, and ship under MIT or proprietary licenses.
shell-mcp publishes to crates.io. clock-mcp publishes to crates.io.
Aftermark went through eight tagged versions before submitting to the
Chrome Web Store. Disposable doesn't mean sloppy. Disposable means
unattached.

It's not a synonym for "small." The size of a disposable tool is
whatever it needs to be. shell-mcp is two binaries, an integration
test suite, and a config-file walker. Aftermark is a TypeScript build
with seven UI views and an IndexedDB-backed local index. Both ship
in a single afternoon. Both are disposable. They're disposable not
because they're small, but because I built them for a job and I am
not married to them.

It's not "throwaway in shame." The repos are public. The bugs are in
the commit history. Future-me — or you, or anyone — can read the
work and see what was thought, what was tried, and what was wrong.
That's the opposite of shame. That's keeping the receipts.

## The litmus test

Here is how to tell if a tool you're considering is a disposable
tool, or whether it's something else dressed up as one:

1. **Audience is exactly one person.** That person is you. If you
   start thinking about a "user," stop. The user in this book is
   the developer building the tool. There's no other user.

2. **You can describe the use in one sentence.** "I want to ask
   Claude what time it is and have it actually know." "I want to
   read this Slack export without writing a converter." "I want
   to find every stale repo across all my GitHub orgs."

3. **The tool ends when the use ends.** Or it doesn't, and survives.
   Either is fine. The point is you don't *plan* for survival. You
   plan for use.

4. **You'd be slightly bored explaining it to someone else.** This
   is a real signal. Disposable tools are not interesting in the
   abstract. They're interesting only in the use. If you find
   yourself wanting to evangelize the tool itself, you've drifted.

5. **You don't write a roadmap.** A roadmap is for tools with a
   future. Disposable tools have a present.

If a tool fails any of these, it's something else. That's not bad —
it's just not this. Some of the work in your life is going to be
real product work, with users and a roadmap and someone who pays
you for it. This book is not about that work.

## The reframe

Here's the reframe to take with you into the rest of the book.

You are not building software. You are buying yourself an afternoon
of leverage. The afternoon is the unit. The tool is a side-effect
of the afternoon.

Some afternoons produce a tool you'll use for years. Some afternoons
produce a tool you use once and forget about. Some produce a tool
that fails — that you abandon halfway, or finish but never use, or
finish and use once and find out it doesn't actually solve the
thing you thought it solved. That's also a successful afternoon,
because *now you know*. The wrong tool is information.

Disposable means held loosely. Loosely held tools fail in interesting
ways and the fixes are usually small. The next chapter is one of
those failures. It shipped a launch-root bug in v0.1.0 and the fix
landed twenty-six minutes later. The fix is a story. The original
bug is also a story. Both stay in the repo. That's the point.
