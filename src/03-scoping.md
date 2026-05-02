# Scoping tight

Once you have the one-sentence spec, the work that determines
whether the build succeeds is the work you do *before* writing
any code. That work is scoping.

I am going to argue that scope is the whole game in this
category. Almost every disposable-tool failure I've watched
trace back to scope. Almost every disposable-tool success
traces back to scope holding. Tightness of scope is more
predictive of success than the developer's experience, the
choice of language, the size of the artifact, or the time
spent on the build.

## The one-sentence test

The test is: *can you state what the tool does in one
sentence, with no comma you don't need, and no "and"?*

Some sentences that pass:

- *Watch a directory and notify me when a file matching this
  pattern changes.*
- *Read a CSV from stdin and print the rows where column N
  matches a regex.*
- *Replay yesterday's git activity into a standup-shaped
  paragraph.*
- *Diff two YAML files semantically rather than line-by-line.*

Some that don't pass, with the reason:

- *Watch a directory, notify me on file changes, and send a
  summary email at the end of the day.* Two tools. The
  watcher and the daily summary are independent. Build
  either, not both.

- *Read a CSV from stdin, print matching rows, and let me
  configure the output format with flags.* The configurability
  is speculative; you don't yet know what flags you'll use.
  Drop it from the sentence. Add a flag the second time you
  reach for one.

- *Manage my bookmarks better.* Not specific. *Better* is the
  word that means *I haven't decided yet.* Not a tool yet.

The test is harsh on purpose. The harshness is a feature: a
sentence that doesn't pass the test isn't a tool yet, and
trying to build it produces something that doesn't quite work.

## Why scope creep happens

Scope creep originates in two places. Both feel like good
intentions while they're happening, which is what makes them
hard to catch.

**The first place is the developer.** Each new feature, when
imagined alone, looks small. *Also let it handle the case
where the input is empty.* *Also add a flag for verbose
output.* *Also support tab-separated as well as comma-
separated.* Each "also" looks like a half-hour. The trap is
the aggregation. Five "alsos" take five times the original
build, and they couple to each other, and what was an
afternoon is now a project.

**The second place is me.** I tend, by default, to include
rather than exclude. Asked to build *X*, I will often
generate *X plus what I think you'd reasonably want next*.
Logging you didn't ask for. Defensive checks against inputs
that can't happen. A `--help` page that documents flags you
don't have. Each addition is plausible. The aggregate is
drift.

The mitigation for the first one is the harshness of the
one-sentence test. The mitigation for the second one is
*stating the cuts out loud*. If you do not want a config
file, say so in the prompt. If you do not want logging, say
so. The cuts, once stated, become part of the working
context, and I will hold them. Without the cuts stated, my
default is to include.

## Subtraction defaults

A few defaults that, if you adopt them, will make your
disposable tools smaller and more likely to ship:

**Stateless over stateful.** If the tool can be a pure
transformation — input goes in, output comes out, no files
written, no database touched — make it that. Stateless tools
cannot fail in the ways stateful ones can. The cost in
capability is small. The cost in failure modes is large.

**Local over networked.** If the data lives on your machine
and the operation is yours, do the operation locally. No
server. No upload. No account. The tool is faster to build,
faster to use, and safer because the data never moves.

**Existing format over new format.** Read the format the data
already comes in. Write the format that already has tools
around it. Inventing a file format is a tax paid forever;
you will pay it every time you come back to the tool.

**One way to do it.** Disposable tools do not need to be
configurable. You set the defaults; you can change the
defaults at the source if they're wrong. Configurability is
something you adopt the second time you need an option, not
the first.

**Crash on bad input.** The audience can read a stack trace.
Don't write defensive code against situations that can't
happen in your use. Trust the inputs from your own code;
validate at the boundary.

**Read-only when possible.** If the tool can avoid writing,
let it. Read-only tools cannot ruin anything. The narrowness
is a feature.

These are not laws. They are defaults. You break them when
the use requires it. The rule is to break them deliberately,
not by drift.

## The thing this chapter is really about

Tight scope is not a virtue, in the abstract. It is a
*discipline*, in the sense that it is something you have to
do continuously across the build, against the gravity of
inclusion. The gravity is real. Each individual addition
will look reasonable. The discipline is to refuse anyway, not
because the addition is bad, but because the addition is
*not in the spec* and the spec is the thing the build is
keeping faith with.

Scope is the whole game in this category because *the spec
is the only thing that distinguishes a disposable tool from a
small product*. A small product has a roadmap and an
audience; its scope is shaped by external pressures. A
disposable tool has neither. Its scope is shaped only by
your discipline. If you don't hold the line, nothing else
will.

The next chapter is about what to do, having held the line,
when the code starts.
