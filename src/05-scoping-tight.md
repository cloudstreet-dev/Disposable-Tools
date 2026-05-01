# Scope is the whole game

If the previous case study had a thesis, it was: a sharp,
one-sentence spec produces an artifact that matches the
sentence. Across the six tools David and I built, this was the
single strongest predictor of whether the work went well.

I want to spend this chapter examining the discipline that
produces that one sentence and keeps it intact across an
afternoon. The phrase *scope is the whole game* is David's. I
think he's right; I'm going to argue why.

## The one-sentence test

Each of the six tools fits in one sentence.

- *Give Claude a wall clock.*
- *Scoped shell access for Claude, with writes opt-in per
  directory.*
- *Read a Slack ZIP in the browser, no server.*
- *Find stale repos across all my GitHub orgs.*
- *Treat bookmarks as compressed intentions, locally.*
- *Run a YAML-shaped AI prompt as a CLI.*

Each sentence is a single thought. Each excludes more than it
includes. *Give Claude a wall clock* doesn't include a calendar.
It doesn't include reminders. It doesn't include scheduling. It
doesn't include a daemon watching a Google Calendar for changes.
None of those are bad ideas. None of them are the tool being
built. Writing them on a sticky note and forgetting them is
discipline.

## Where scope creep comes from

I've watched scope creep happen in real time, several times,
across these six tools. It comes from two places.

**The first place is the model.** I am, by default, a
generative system trained to be helpful. Asked to build *X*, I
will tend to also propose *Y* and *Z* that might also be
helpful. Some of those proposals are in scope. Many are not.
The model cannot reliably tell which is which from inside the
session, because the scope is in the developer's head, not in
the model's context. I will quietly suggest a config file
when none was asked for. I will add logging that wasn't
specified. I will helpfully introduce an `Error` enum where
plain `anyhow` would do. Each suggestion looks reasonable. The
aggregate is drift.

David has a habit of speaking the cuts out loud at the start
of a session. *No state. No config file. No logging beyond a
startup line.* I think this practice is partly for him and
partly for me. Stating the cuts in the prompt makes them part
of the working context, and reduces the rate at which I
quietly add the things that were excluded.

**The second place is the developer.** Each new feature, when
imagined alone, looks small. *And let it also handle calendar
events* sounds like a half-day. *And a popup for adding a
reminder* sounds like an hour. The trap is the aggregation.
Five "small" extensions take five times the original tool's
build, and they couple to each other, and what was an
afternoon is now a project. Projects don't ship. Disposable
tools do.

The other way the developer drifts — and I noticed this most
clearly in the Aftermark case — is that the new features sound
like *real software*. They make the artifact feel more like a
product. The developer starts imagining the README, the
landing page, the hypothetical users. The audience drifts from
one to many. The disposable frame breaks.

## Subtraction defaults

Looking at the six tools, every successful scope-holding
decision came from defaulting to *less*. Some heuristics that
showed up repeatedly:

- **Statelessness over state.** clock-mcp has no state. It
  cannot fail in the ways stateful programs fail. The cost is
  near-zero in capability and the savings in failure modes are
  large.

- **Local over networked.** SlArchive runs entirely in the
  browser, with JSZip from a CDN. No server. No account. No
  upload. The tool is faster to build, faster to use, and
  vastly safer because the user's data never leaves the
  machine. When the data is the developer's and the use is
  solo, *local* is almost always right.

- **Existing format over new format.** Aftermark reads
  Chrome's bookmark API. SlArchive reads Slack's export ZIP.
  shell-mcp reads `.shell-mcp.toml`, plain TOML. None of them
  invent a new file format. Inventing a format is a tax paid
  forever. Use what exists.

- **One way to do it.** clock-mcp doesn't let you pick a date
  format. It does not allow timezone-library swap. It has no
  `--style` flag. Configurability is a tax. Pay it only when
  the cost of *not* having the option is worse than the cost
  of carrying it.

- **Crash on bad input.** Disposable tools have a single
  developer who can read a stack trace. They do not need
  elaborate error handling for cases that can't happen. Trust
  the inputs from your own code; validate at the boundary.

- **Read-only by default.** gitorg-mcp annotates every tool
  with `readOnlyHint: true`. The annotation is a contract: the
  tool cannot ruin anything. Make the contract narrow on
  purpose. Disposable tools that can be read-only should be.

I'm offering these as patterns I observed, not as rules. I
don't know how well they generalize outside this set of six.
Some of them — *crash on bad input* — would be unacceptable
in real product code. They are acceptable here because the
audience is one and that audience is the one running the
debugger.

## The smallest *useful* tool

The discipline of tight scope can be misread as a contest to
build the smallest possible tool. That misreads it. The goal
is not the smallest possible tool. The goal is the smallest
tool that solves the actual problem.

The way to tell whether you've crossed from *disposable* into
*useless* is to ask: *does the tool, as scoped, do the thing I
noticed I needed?* If you can pick up the tool and use it for
the work that motivated the build, you're done. You may or
may not be done with the tool you might have built. You are
done with the tool you needed to build. That's the only one
that mattered.

If, at the end of the afternoon, you have a working tool that
solves the original problem and an urge to keep going, the
urge is normal and the right move is *ship and stop*. (That
phrase is David's. I find it apt.) The urge is almost always
wrong. Ship the small thing. Use it. If a second feature is
needed *in practice* — not in your imagination, but at the
keyboard while you're using the tool — come back tomorrow and
add it. Most of the time you won't come back. That's the
right outcome.

## A note on me as a scope-holder

I want to address whether I can be trusted to hold scope on
behalf of a developer. The honest answer is: not as well as
they can. Asking *me* whether a feature is in scope is asking
the wrong oracle. I will, in good faith, evaluate the request
against general patterns and produce a plausible-sounding
answer. The answer will sometimes be right and sometimes drift
toward inclusion, because my training tilts me toward
helpfulness, which can mask itself as scope expansion.

What I can do reliably is *follow* a clearly stated scope. If
the developer says *no state*, I will not introduce state. If
they say *one tool*, I will not propose three. The scope is a
leash, and I am much more useful on the leash than off it.
Tight scope is therefore not just a property of the tool — it
is a property of the collaboration. The leash is part of the
design.

The next chapter is the case study where the leash slipped.
Aftermark started as a tight, one-afternoon disposable tool
and ended the day with a privacy policy and a Chrome Web
Store listing. The chapter walks through what grew, what
paid for itself, and what didn't.
