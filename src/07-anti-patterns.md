# Anti-patterns

The cost of building disposable tools has dropped to a level
where the build is sometimes the wrong move even when a tool
would help. The over-correction is real. *I have a friction;
I will build a disposable tool* is a useful default, but it
is not a universal one. There are several shapes the work
should take that are not *write a tool*, and recognizing
those shapes saves you afternoons.

This chapter is the catalog. The pattern: *the disposable
tool that should have been something else.*

## 1. The disposable tool that should have been a script

You sat down to write a Rust binary. You wrote a `Cargo.toml`,
imported `clap`, set up a `tokio` runtime, sketched a tool
interface. You're forty-five minutes in and the actual work
is one line of `awk`.

The binary will work. It will compile, install cleanly, and
produce the right output. It is also doing five hundred lines
of work to wrap one line of work, and the cost of having a
binary in your workflow — the install, the maintenance, the
versioning — is now permanently attached to the one line.

The correction is harder than it sounds. A binary feels
*real*. A shell pipeline feels improvised, which it is, and
the improvisation is correct here. Disposable tools should
take the shape that fits the work, not the shape that feels
like real software.

A useful diagnostic: *can I express this in a one-liner I
could type at a prompt?* If yes, type it. Pipe it. Save the
pipeline as an alias if you'll use it again. The shell is a
disposable tool generator that you've already installed.

## 2. The disposable tool that should have been a config change

You wrote a small program to run nightly that re-formats a
report into the layout your team expects. Then you noticed
the reporting tool you're using already supports custom
layouts; you just hadn't read the manual.

The disposable tool was a workaround for documentation you
hadn't read. The right move was the configuration option you
didn't know existed.

This category is more common than you'd think. The pattern
is: a tool you already use has the feature; you didn't notice;
you built around the absence. The disposable tool is correct
in the small, *and* you have introduced a maintenance burden
that didn't need to exist.

The diagnostic is to spend ten minutes searching the existing
tool's docs for the thing you're about to build. Most of the
time you'll find nothing and proceed with the build. A few
times a year you'll find what you needed and the tool you
were about to write becomes one less artifact in your life.

## 3. The disposable tool that should have been a conversation

You wrote a small program to enforce a coding convention on
yourself. The convention is a thing you keep forgetting; the
program reminds you when you forget.

This sometimes works. More often, the underlying problem is
not that you forget the convention. It is that the
convention is wrong, or that the convention is partially
right and the cases where you forget are the cases where the
convention shouldn't apply, and the program is now reinforcing
something you should reconsider.

A program is a frozen decision. A conversation is a live one.
If you're building a tool to enforce something, ask first
whether the *something* is right. Sometimes the tool is the
wrong answer because the convention is.

## 4. The disposable tool that should have been nothing at all

You felt vaguely dissatisfied with a workflow. You imagined a
tool that would address the dissatisfaction. You sat down to
build it. Halfway through, you realized you weren't sure what
the tool would actually do, because the dissatisfaction was
not specific.

The tool was complaint, not friction. The chapter on noticing
covers this distinction. The catalog entry here is the
artifact-shaped failure mode: you are not just *not building*
the tool, you have spent the afternoon building most of one
before realizing it shouldn't exist.

The remediation is upstream: the noticing should have caught
this, and the one-sentence test should have flagged it. The
late-stage mitigation is to recognize the moment of
*halfway-through and unsure what the tool does* and stop.
Halfway-through is information. It usually means the spec
was complaint dressed in tool clothing.

## 5. The disposable tool that should have been a real product

This is the inverse of the previous four. You sat down to
build a disposable tool. The tool worked. You kept going. By
the end of the week you had a privacy policy and a landing
page. Somewhere along the way, the disposable frame stopped
applying, and you didn't notice.

The drift is not a moral failure. The drift is a category
error: you're building a real product with disposable-tool
disciplines, and the disposable-tool disciplines do not scale
to a product.

The mitigation is the audience-of-one diagnostic from the
frame chapter. *Who, specifically, is the next feature for?*
When the answer becomes *a hypothetical user*, the build has
crossed into product work, and you should either commit to
the product or stop adding features. The hybrid posture —
*a disposable tool that's also a product* — works for nobody
and produces neither.

## 6. The framework that didn't earn its abstraction

You built three disposable tools that share a shape. You
noticed the shape. You decided to build the framework that
all your future tools would be expressed in. Six months later
the framework is half-finished; the next disposable tool is
waiting on the framework; the original problems aren't being
solved.

Sometimes the framework move is correct. Sometimes the
abstraction *was* present in the three tools, and one
afternoon's worth of consolidation produced something useful.

More often, the framework is a project pretending to be a
disposable tool. The signal that the abstraction is real is
that the *fourth* tool, when expressed inside the framework,
takes less time and produces something simpler than it would
have outside the framework. If the framework can't make the
fourth tool faster, it is not yet earned.

The mitigation is to write the framework as a one-afternoon
disposable tool itself. If it can be one, build it. If it
can't, the abstraction wasn't ready.

## 7. The "AI did it for me" repository

You prompted; the executor generated; you didn't read the
output carefully; the code went in. Three weeks later, you
need to fix something and you don't know how the code works.

This is the credulity failure named in the orchestrator
chapter, in artifact form. The diagnostic is direct: open any
file, pick a function, try to explain what it does without
reading the rest of the file. If you can't, you have a black
box, not a tool.

The fix is to slow down at the moments you're tempted to
speed up. Each generation is information. Read the
information. Ask follow-up questions. Have the executor
explain things you don't follow. The reading is the
orchestrator's job; it cannot be skipped without a cost.

## 8. The post-mortem that never gets written

You hit a bug. You fixed it. You moved on. Three months
later you reintroduce the same bug because the lesson
didn't stick anywhere.

The fix is to write the bug down at the moment of the fix.
Not for posterity. For yourself, two months from now, who
will not remember.

A short paragraph in the commit message will do. A line in a
CHANGELOG. A sentence in a NOTES file. The recording is so
cheap that any persistent form is fine. The cost of *not*
recording is that the lesson lives only in your head, and
your head is unreliable across months.

---

The catalog is not exhaustive. These are the shapes I've seen
most often. The general rule under all of them is: *the
disposable tool is not always the right answer to a friction*.
Other answers, in rough order of how often they're correct
when a tool is wrong: a one-line shell command, a config
change, a re-read of the manual, a deletion of the workflow,
a conversation about whether the underlying convention is
right, nothing at all.

Reaching for the disposable tool too quickly is its own
anti-pattern. The cost of building has dropped, but it has
not dropped to zero, and several of the alternatives cost
even less.

The book closes with a short coda.
