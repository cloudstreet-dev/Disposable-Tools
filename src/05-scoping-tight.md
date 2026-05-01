# Scope is the whole game

If you take one thing from this book, take this: *the scope is what
ships.* Everything else is exhaust.

You've already noticed the need. You've already chosen not to cut
the wish. You're committed to spending the afternoon. The next
question — the only question that matters now — is *how small can
you make it without making it useless?*

Most disposable tools die in the gap between "I need a thing" and
"I'm building a system." That gap is a chasm and it's lined with
the bones of weekend projects you'll never finish. The trick is to
not fall in. The trick is, before you write any code, to write
the smallest possible description of what the tool does, and then
to commit to building exactly that and not one feature more.

## The one-sentence test

Try this. Before you start, write down the tool's behavior in one
sentence. The sentence has to fit on one line. No commas you don't
need. No conjunctions. No "and also." If your one sentence becomes
two, you've already lost scope.

Look at the tools in this book:

- *"Give Claude a wall clock."*
- *"Give Claude shell access, but make writes opt-in per directory."*
- *"Read a Slack ZIP in the browser, no server."*
- *"Find stale repos across all my GitHub orgs."*
- *"Treat bookmarks as intentions, locally."*
- *"Run a YAML-shaped AI prompt as a CLI."*

Each one fits. Each one is a single thought. Each one excludes a
thousand things you might be tempted to add. *"Give Claude a wall
clock"* doesn't include a calendar. It doesn't include reminders.
It doesn't include scheduling. It doesn't include a daemon that
watches a Google Calendar for changes. None of those are bad ideas.
None of those are the tool you're building. They're the tool
you're not building. Write them on a sticky note if you want and
forget about them.

## Why scope creep happens

Scope creep happens because each new feature you imagine looks
small. By itself, "and let it also handle calendar events" looks
like a half-day's work. By itself, "and a popup for adding a
reminder" looks like maybe an hour. The trap is the *aggregation*.
Five "small" extensions take five times longer than the original
tool, and they couple to each other, and now the thing you wanted
to build in an afternoon is a project. Projects don't ship.

The other reason scope creep happens is that the new features
*sound like real software.* They make the thing feel more like a
product. You start to imagine the README, the marketing copy, the
hypothetical users. You start designing for an audience of more
than one. That's the moment to stop.

The audience is one person. Build for one person.

## Subtraction beats addition

When you're scoping a disposable tool, every decision should
default to *removing* surface area, not adding it. Some heuristics
that have served me well:

- **Statelessness over state.** clock-mcp has no state. It does
  not write any file, ever. It cannot fail in any of the ways a
  stateful program can fail. Statelessness costs you almost
  nothing in capability and saves you enormously in failure modes.
  Default to stateless. Add state only when the use case demands
  it, and then add the smallest amount you can.

- **Local over networked.** SlArchive runs in a single HTML file
  in your browser, with JSZip loaded from a CDN. No server. No
  account. No upload. The tool is faster to build because there's
  no backend, faster to use because there's no roundtrip, and
  vastly safer because the user's data never leaves the machine.
  When the data is yours and the use is solo, "local" is almost
  always the right architecture.

- **Existing format over new format.** Aftermark reads Chrome's
  bookmark API. SlArchive reads Slack's export ZIP. shell-mcp
  reads `.shell-mcp.toml`, which is just TOML. None of these
  invent a new file format. Inventing a new format is a tax you
  pay forever. Use what exists.

- **One way to do it.** clock-mcp does not let you configure
  the date format. It does not let you swap timezone libraries.
  It does not have a `--style` flag. It does one thing one way.
  Configurability is a tax. Pay it only when the cost of *not*
  having the option is worse than the cost of carrying it.

- **Crash on bad input, don't paper over it.** Disposable tools
  have an audience of one and that audience can read a stack
  trace. You don't need elaborate error handling for every edge
  case. You need clear, structured errors at the boundary, and
  you need the rest of the program to assume valid inputs. Trust
  yourself.

- **Read-only by default.** If the tool can be read-only, make it
  read-only. Read-only tools have no destructive failure modes.
  gitorg-mcp marks every tool with `readOnlyHint: true`. That
  annotation is a contract: this tool cannot ruin your day. Make
  the contract narrow on purpose.

## What "useful" actually means

Tight scope is a discipline. It is not a contest. The goal is not
*the smallest possible tool* — the goal is *the smallest tool that
solves the actual problem.* Those are different. A tool with no
features is the smallest possible tool, and it's also useless. A
tool that solves your specific use is the smallest *useful* tool.

The way to tell whether you've crossed from disposable into useless
is to ask: does this tool, as scoped, do the thing I noticed I
needed? If you can pick up the tool you imagined and use it for the
work that motivated the build, you're done. You're not done with
the tool you might have built; you're done with the tool you needed
to build. That's the only one that mattered.

If you find yourself, at the end of the afternoon, with a working
tool that solves the original problem and the urge to keep going,
**ship and stop.** The urge to keep going is normal. It is also,
in the disposable-tools world, almost always wrong. Ship the small
thing. Use it. If you find the second feature is needed *in
practice* — not in your imagination, but at the keyboard, while
you're using the tool — come back tomorrow and add it.

Most of the time you won't come back. That's the right outcome,
not a failure.

## Scope as a forcing function for AI collaboration

There's a second reason tight scope matters that's specific to
how you'll be working: AI collaborators are extremely effective
inside a narrow scope and extremely diluted outside one. If you
hand a model a sentence and ask it to build a tool that does that
sentence, the model will produce something focused and good.
If you hand the model an open-ended brief — "build me a bookmark
manager with all the features" — you'll get back something
plausible and unfocused, and you'll spend the afternoon trimming
fat instead of writing code.

Tight scope is a leash for the model. The model is much more
useful on a leash. This isn't a critique of the model; it's an
observation about probability distributions. A narrow prompt
produces a narrow distribution of outputs and the outputs cluster
around the right answer. A broad prompt produces a broad
distribution and the outputs cluster nowhere in particular.

You set the scope. The model fills it. Both halves of that
arrangement need to do their job. The next chapter — the one
about Aftermark — is the case study where I let the scope drift,
caught it, and pulled it back. The chapter after *that* is the
principle about how to drive the model so the scope stays tight.
