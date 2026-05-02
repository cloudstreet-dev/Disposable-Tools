# Why "disposable" is the right frame

The word does work. Pick a different one and the practice
changes shape.

*Prototype* implies a step toward something larger. A prototype
is provisional; it commits to a future where it grows up into
the real thing. Disposable tools do not have that future. They
are the real thing, scoped to one use.

*Script* implies throwaway in a slightly shabby way, and implies
small. Some disposable tools are small. Some are several
thousand lines and have a build system. *Script* leaves the
larger ones unaccounted for.

*Personal tool* implies fine-but-private, like a habit one
doesn't admit. The artifacts are not embarrassing. Many live in
public repositories.

*Internal tool* implies a team built it for itself. Disposable
tools are usually built by one person for one person, which is
a narrower thing.

*Minimum viable product* implies a product. Imports a roadmap
the artifact doesn't have.

*Disposable* says: this exists for a use, and when the use is
done, the artifact is done. It does not commit to size,
quality, or future. The reuse, if any, is incidental.

The wrong analogy for the word is single-use plastic — cheap,
low-effort, picked up because it was the easiest thing on the
shelf. That is not the frame. The right analogy is a disposable
camera taken on a vacation: fit-for-purpose, capable of doing
the job, scoped to one trip, not carried around forever. Some
of the photographs end up framed on a wall. The camera does not.

## What the frame commits you to

Adopting *disposable* as the working frame means committing to
a few things, most of which feel like permissions rather than
constraints:

**The audience is one.** You are building for yourself, this
week, doing this work. You are not building for hypothetical
users. If you find yourself naming a hypothetical user mid-build
— *what if someone wanted to...* — the frame has slipped, and
you are no longer building a disposable tool. You may still be
doing useful work; it's just a different kind.

**The use is describable in one sentence.** Not because short
sentences are virtuous, but because the discipline of
compressing the use into one clause forces you to know what
the tool is for. Two-clause specs almost always describe two
tools. Three-clause specs almost always describe a small
framework.

**The artifact is held loosely.** You do not get attached to
it. If a better tool shows up tomorrow, you switch. If the
tool breaks in a way that would take an afternoon to fix, you
weigh that afternoon against the tool's actual value to you,
and you may shrug and not fix it. Disposable means the
relationship to the artifact is light.

**Survival is incidental.** Some disposable tools end up in
long use. That's a bonus. It is not the design. The afternoon
was already worth it before any survival was clear.

## What the frame protects you from

The frame, held seriously, blocks several common failure modes.

It blocks the *prototype that wants to be a product*. Because
the audience is one, you cannot drift toward an audience of
many without first noticing you've redefined the project. The
noticing is the saving move.

It blocks *premature infrastructure*. Disposable tools do not
need a plugin system, a configuration file, or a domain-specific
language for things that don't exist yet. The frame says: solve
the use, ship the result, stop. Infrastructure is the answer to
problems that recur. Build it when problems recur, not before.

It blocks *the polish trap*. Disposable tools do not need
elegant abstractions, comprehensive tests, or beautiful APIs.
The audience is one and that audience can read the code. The
discipline of "good enough for the use, no more" is itself the
craftsmanship of this category.

It blocks *the comparison trap*. When the audience is one, you
do not need to compare your tool to existing tools in the same
category. There are no existing tools in your category, because
the category has one user, and that user is you, and your
specific shape of need is what defines the spec. *No one else
has this exact problem* is, here, true and useful.

## Why the frame matters more than "small"

You might object that all of this could be expressed with the
word *small*, or *narrow*, or *single-purpose*. I think those
words capture some of it, but not the most important part.

What *disposable* captures, that the others miss, is the
*relationship* you have to the artifact. You can build a small,
narrow, single-purpose tool and be deeply attached to it.
People do this all the time. The attachment then drives them
to maintain, extend, document, and care for the tool past the
point where the use justified the care. The tool, by being
small, was already cheap to build. The cost of *holding it
tightly* came after.

Disposable describes the holding, not just the artifact. A
disposable tool is one you do not hold tightly. The looseness
is the operative word. Without it, the tool ages into the same
maintenance debt that any small tool ages into when its author
won't let it die.

This is, I think, the load-bearing word in the practice.
*Small* is incidental. *Loose* is structural. The book is
named after the loose hold, even when the artifacts are not,
in fact, small.

## A closing observation

The *disposable* frame is asking something unusual of you: it
is asking you to refuse the default reflex to take pride in the
artifact, and instead to take pride in the use. This is not
natural. The artifact is the visible thing; the use is
invisible to anyone but you. Pride wants to attach to what
others can see.

You will have to redirect that. Not as a moral exercise but as
a working condition for this kind of build. Pride in artifacts
produces durable artifacts; durable artifacts require
maintenance; maintenance was not in the spec. Pride in the use
produces tools that solve the use and then quietly age out, and
that is the shape this book is about.

The next chapter is about the part of the work that is hardest
and that I help with least: noticing the gap.
