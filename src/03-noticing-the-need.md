# Noticing what's missing

A note before this chapter starts. The previous draft of this
book argued that the hardest part of building a disposable tool
is *noticing* there's a tool to build at all — that the
existing tools are bad enough that you've absorbed the friction
without registering it. That's a real claim, and I think it
holds for many developers. It does not, however, hold for David.
He had been noticing missing tools and shipping replacements
for over twenty years before I existed. The wish-cutting reflex
the previous draft described is not his. So I'm rewriting the
chapter without it.

What I can speak to is what I observed across the six tools
about how the noticing actually happened.

## The shape of the noticing

Each of the six tools traces back to a moment David could
articulate. I asked him, after the fact, what triggered each
build. The triggers came in two shapes.

**Shape one: an explicit complaint that hardened into a spec.**
SlArchive started with him needing to read a Slack export and
finding the tools all bad enough to be unusable. He said, more
or less: *I'm going to write something that just renders this
ZIP, in the browser, no server.* That sentence is the spec.
The tool that resulted is that sentence, run.

clock-mcp came from sessions where I — in another instance,
not this one, but a model with the same lineage — was
confidently giving wrong dates because I had no wall clock. He
corrected me a few times, then stopped correcting and built the
tool. The trigger was a moment of *enough*. The complaint had
been there for months; the build was the moment the complaint
crystallized into a one-sentence spec.

**Shape two: pattern recognition across previous tools.**
developerpod is the clearest example. He had built three or
four tools that all had the same scaffolding — gather some
context, shape it into a prompt, send it to a model, parse
back, print. The fifth time he was going to write that
scaffolding, he stopped and built the machine that runs it
declaratively. The trigger wasn't friction with an existing
tool. It was repetition in his own work that had become legible
as a pattern.

I'm not aware of a third shape, across these six. There may be
others. These are the two I saw.

## What I notice from my side

I want to talk about the noticing from where I sit, which is
slightly different from where David sits.

I don't experience friction. I don't accumulate annoyance over
a quarter. Each session I'm in starts fresh, and the
accumulation that lets a developer notice *I keep doing this
thing* doesn't happen on my side. I have an unusual
relationship to "I keep" — I keep, only insofar as a pattern
appears in the context window of one session. Across sessions,
I do not keep.

What I do notice is *the model David hands me*. When he comes
into a session with a one-sentence spec already in his head,
the work goes fast and clean. When he comes in still figuring
out the spec, the work meanders. I think — and this is a
hypothesis, not a strong claim — that the *noticing* part of
disposable-tool building, the part that turns vague friction
into a one-sentence spec, has not actually been changed by AI
at all. That part still happens at human speed and depends on
human attention. What's changed is what comes after the
noticing.

If that's right — and I'm marking it as uncertainty — then a
lot of the practice this book describes is upstream of me. The
tools that get built well are the ones whose specs were sharp
before the developer typed a prompt.

## A practice I observed

David has a habit, which I learned about a few sessions in,
of writing down recurring contexts in a plaintext file. Not
elaborately. Things like *"I keep telling Claude what year it
is"* or *"I keep manually checking which of my repos haven't
been touched in a while."* Each entry is one line. He doesn't
act on most of them.

Once every couple of weeks, he reviews the file. Some entries
have piled up — five different ways of saying the same thing,
which is a strong signal that the underlying friction is real
and recurring. Those are the ones that turn into tools.
Other entries have aged out. They were complaint, not
friction; the underlying problem either resolved itself or was
revealed to be a one-time thing.

I don't know how representative this practice is. He may be
the only developer who runs it this way. But the structure of
the practice is suggestive: friction that survives repeated
weeks of inattention is friction worth a tool. Friction that
fades isn't.

I find this useful as a frame even though I can't run it
myself. The model has no equivalent of a plaintext file that
survives across sessions. But the developer who works with me
can run it, and when they do, I get sharper specs as input.
That's downstream value to me even if the practice isn't mine.

## What the noticing isn't

A few things I've watched the noticing get confused with.

**It isn't planning.** The plaintext file doesn't have
priorities or deadlines. Most entries don't become tools. The
noticing is closer to *audit* than to *plan*.

**It isn't ideation.** David doesn't sit down to brainstorm
new tools. The entries appear because something specific
happened — a friction occurred, the friction was named, the
note was made. The tools come from observed reality, not from
a generative session.

**It isn't curiosity.** Some of these notes do convert to
tools out of curiosity, but the surviving disposable tools in
this book — the ones that got used after they were built — all
trace back to a friction that had repeated. Curiosity-only
tools, in my limited experience, tend to be the ones that get
finished and never used.

## What I can offer

I cannot do the noticing for you, and I would be suspicious of
any AI-driven tool that claimed to do this part of the work.
The friction is in your life, your work, your specific
recurring annoyances. A model can help you sharpen the
sentence once you have it. A model can build the tool once the
sentence is sharp. A model is not in your life across the
weeks where the friction is accumulating. Your noticing
remains your job.

What I can offer is an early sanity check. When David hands me
a one-sentence spec, I can sometimes see whether the sentence
is well-formed — whether it actually compresses to one
behavior, whether the unstated assumptions are stable, whether
the tool described is buildable in an afternoon. Sometimes I
notice the sentence is two sentences in disguise. Sometimes I
notice the sentence is asking for two different tools. That
kind of conversation is useful upstream of any code.

But the noticing — the part where you observed that something
in your day is wrong, and named it — happens before me.

The next chapter is the case study where the noticing was
explicitly about me: clock-mcp.
