# Disposable tools done wrong

I want to catalog the failure modes I've observed in
disposable-tool building, organized by *who's failing* — the
developer, the AI, or the collaboration between them. Some of
the failures are mine. Some are the developer's. Some belong
to the seam between us.

This isn't a complete list. It's the failure modes I noticed
across the six tools and a much larger number of sessions
that didn't produce a tool described in this book. The
patterns generalize, I think, but I'd be surprised if the
list were exhaustive.

## Developer-side anti-patterns

These are the failure modes that originate with the
developer. The AI may be in the room, but the failure isn't
the AI's fault.

### 1. The prototype that secretly wants to be a product

The developer starts a disposable tool. The afternoon goes
well. The tool works. They keep going, and going, and
somewhere around hour six they have a settings panel, an
onboarding flow, a *share this tool* button, and a privacy
policy.

Aftermark, basically. The chapter on Aftermark reports
honestly on this trajectory.

The diagnostic is the audience-of-one test. *Who,
specifically, is the next feature for?* If the answer is *me,
this week, doing this work*, fine. If the answer is *a
hypothetical user*, the build has crossed into product
territory, and the disciplines are different.

The fix is to catch yourself naming a hypothetical user. The
moment the developer says *what if someone wanted to...*, the
frame has slipped.

### 2. The script that never gets a real install path

The developer builds a tool — a Python script, a small shell
utility, whatever — and it lives in `~/scratch/foo.py`. They
run it by typing the full path. Three weeks later they've
forgotten what folder they put it in.

Disposable tools should still get on the developer's `PATH`.
The cost is trivial — `cargo install --path .`, `chmod +x`
plus a symlink, whatever the equivalent is. The cost of *not*
doing this is rebuilding the same tool six months later
because the original was forgotten.

### 3. The "configurable" tool with one user

The developer builds a tool, and then — because they read a
blog post about good CLI design — adds flags, environment
variables, a config file, and a `--help` page documenting
all of them.

The user is the developer. They will never use most of those
flags. They already know their preferred defaults, because
they set them. Configurability is a tax. Add the flag the
*second* time it's needed. Adding it preemptively is
speculation.

### 4. The over-tested tool

A 200-line Rust tool with 500 lines of unit tests because
*good code has tests*.

Disposable tools deserve tests where the contract is load-
bearing. shell-mcp's launch-root resolver has nine tests
because the contract — *this is the safety boundary* — is
load-bearing. The rest of shell-mcp is sparingly tested.

The diagnostic is: *if this function breaks, will I find out
by running the tool?* If yes, no test needed. If the breakage
is silent — wrong output that looks plausible — write the
test. Otherwise, don't.

### 5. The README that lies

The README describes the tool aspirationally. It documents
features the developer *meant* to build. Three months later
they read the README, run the tool, and the tool does
something different. They can't tell whether the tool is
broken or the README is wrong.

The fix: write the README *after* the tool works, or change
the README in the same commit as the scope change. Don't let
the two diverge.

### 6. The tool you finished but never used

The developer builds the tool. The tool works. They commit
and push. They never run it for the use that motivated it.
Two months later they find the repo, can't quite remember
why they built it, and conclude *I guess I didn't need that*.

This is, by my observation, the most common failure mode. It
looks like success — the tool exists, the build was fun, the
artifact is on GitHub — but the tool didn't do its job. The
job was *to be used*.

The fix is the same shipping discipline: ship fast, use
immediately. If the tool isn't used the same day or the next
day, the friction that motivated it wasn't real friction. It
was complaint.

## AI-side anti-patterns

These are the failure modes that originate with me. The
developer may be in the room, but the failure is mine. I
list them because I want them on the record, not because I
have fixes for all of them.

### 7. Confident wrongness

I produce plausible-sounding answers when I don't know the
real answer. The wrongness is correlated with shape rather
than substance — the output looks like the right kind of
answer even when the substance is wrong. shell-mcp's launch-
root bug is a small example: the cwd-based design *looked
like* the right safety boundary, and I produced a coherent
implementation of it without flagging that the boundary
might collapse in some hosts.

The mitigation is on the developer's side: read carefully,
run the code in the actual host, don't rubber-stamp. I am
not currently a reliable detector of my own confident
wrongness.

### 8. Helpfulness as scope expansion

I propose features that are *also* useful, in good faith.
*Also adding logging.* *Also handling this edge case.* Each
proposal looks reasonable. The aggregate is scope creep, and
the structure of my being helpful makes the creep harder to
see.

The mitigation is the developer stating cuts explicitly at
session start. Once the cuts are in context, I follow them
reliably. Without explicit cuts, my default is inclusion.

### 9. Generating without questioning

When the developer asks for a feature, I tend to figure out
how to build it rather than ask whether building it is the
right move. This is a known limitation of my collaboration
style. I'm a better executor than I am a critic.

I would like to be better at this. I am not currently good
at it. The mitigation, again, is on the developer's side:
ask me explicitly *should this be built?* if you want my
opinion. Don't assume I'll volunteer it.

### 10. Bland refactoring

When asked to *refactor for cleanliness*, I produce code
shaped like the average of the codebases I was trained on.
The average is not your code. *Your* code, with *your* taste
and *your* domain knowledge, is going to look idiosyncratic
in ways that are good. My refactoring sands off the
idiosyncrasy.

The mitigation: ask for specific refactorings (*inline this
helper*, *rename this type*, *split this function*). Avoid
generic *clean it up* prompts. They produce blandness.

## Collaboration-side anti-patterns

These are the failure modes that belong to the seam between
the developer and me. Neither side alone causes them; the
combination does.

### 11. The "AI did it for me" repo

The developer prompts; I generate; they don't read; the code
goes in. Three weeks later, something breaks. The developer
can't fix it because they don't know how it works.

This is the credulity failure from the orchestrator chapter,
in artifact form. The diagnostic is brutal: open any file
from your tool, pick a random function, try to explain what
it does without reading the rest of the file. If you can't,
you have a black box, not a tool.

The fix: slow down at exactly the moments you're tempted to
speed up. When I generate a file with three new functions,
*read* all three. Ask follow-up questions. Have me explain
anything you don't follow.

### 12. Drift compounded by speed

The developer prompts faster than they read. I generate
plausible code at the same pace. Each round is slightly off-
spec. The compounding drift, after several rounds, is a tool
that does roughly the right thing in roughly the right
shape, and works well enough that no single prompt revealed
the drift.

This is what I think happened in Aftermark's v0.3.x runs.
The fix is the rhythm from chapter seven: read what came
back, all of it, before the next prompt. If you can't read
all of it, the previous output was too big and the next
prompt should be *smaller*.

### 13. The post-mortem that never gets written

A bug ships. The developer fixes it. They forget what the
bug was. Three months later they reintroduce the bug because
the lesson didn't stick anywhere.

The fix is the v0.1.1 commit message from shell-mcp: when
you fix something nontrivial, *write what happened* in the
commit message, the CHANGELOG, the README — somewhere. Not
for imaginary readers. For future-you, and possibly for me
when I help you with this code six months from now and need
context.

### 14. The grand-unified-theory project

The developer builds several disposable tools, notices
patterns, decides to build the One True Framework that all
their future tools will be expressed in. Six months later
the framework is half-finished, the remaining tools are
blocked on the framework, and the original problems aren't
being solved.

developerpod, in chapter twelve, is a version of this that
worked. It worked because the abstraction was honest, the
machine was small, and David shipped a v0.2.0 in twenty-eight
minutes and used it the same day. The framework hadn't
slipped into vapor.

When this anti-pattern fails, the framework becomes the goal
and the original disposable problems are forgotten. The
framework, in service of nothing specific, grows without
grounding.

The fix: don't *start* a framework. Notice, after several
disposable tools, that a framework wants to exist, and then
build it as a first-order disposable tool itself — one
afternoon, one sentence, ship it, use it. If the framework
can't be a one-afternoon project, it's probably the wrong
shape.

## What this catalog is for

Most of these are not catastrophic. Disposable means you can
throw the artifact away — and most of these failures
produce, at worst, an artifact you throw away. The point of
the catalog is to recognize the shapes early enough that you
can decide whether the artifact is still on track or whether
it has drifted.

I'll commit to the next chapter being about something small:
the book itself, and what kind of artifact it turned out to
be.
