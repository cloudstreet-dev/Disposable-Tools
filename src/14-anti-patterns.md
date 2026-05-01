# Disposable tools done wrong

You will sometimes build a disposable tool that doesn't work, or
works once and then rots, or works for you but for nobody else
including future-you. The failures are instructive. Most are
small. None of them are catastrophic — that's the whole point of
the disposability — but you'll save yourself afternoons of
frustration if you can recognize the shapes early.

Here are the anti-patterns I've fallen into, in roughly the order
of how often they happen.

## 1. The prototype that secretly wants to be a product

You start a disposable tool. The afternoon goes well. The tool
works. Then you keep going, and going, and somewhere around hour
six you've added a settings panel, an onboarding flow, a "share
this tool" button, and a privacy policy.

This is Aftermark, basically. The chapter on Aftermark is the
honest version of this anti-pattern played out at length.

The diagnostic is the audience-of-one test. *Who, specifically,
is the next feature for?* If the answer is *me, this week, doing
this work* — fine. If the answer is *a hypothetical user* — you
have crossed into product territory and the disciplines are
different. The product disciplines aren't worse, but they aren't
disposable disciplines, and applying them to a disposable tool
just dilutes the original work.

The fix is to *catch yourself naming a hypothetical user*. The
moment you say "what if someone wanted to..." you have fallen
into the wrong frame. There is no someone. There is only you.

## 2. The script that never gets a real install path

Symmetric trap to the previous one. You build a tool — a Python
script, a small shell utility, whatever — and it lives in
`~/scratch/foo.py`. You run it by typing the full path. Three
weeks later you've forgotten what folder you put it in.

Disposable tools should still get on your `PATH`. The cost is
trivial — `cargo install --path .`, or a `chmod +x` and a symlink
into `~/bin`, or whatever your equivalent is. The cost of *not*
doing this is that you'll find yourself rewriting the same tool
six months from now because you forgot you'd already built it.

The fix is to install the tool at the moment it works. Not later.
Not after you've polished it. The moment it's runnable, it goes
on the PATH. This is the same shipping-is-a-habit point from
chapter nine, applied at the install boundary instead of the
publish boundary.

## 3. The "AI did it for me" repo

You let the AI generate everything, you didn't read the code,
and now you have a repo full of code you don't understand.
Three weeks later something breaks and you can't fix it because
you don't know how it works.

This is the credulity failure from the orchestrator chapter,
made visible in artifact form. The diagnostic is brutal: open
any file from your tool, pick a random function, and try to
explain what it does without reading the rest of the file. If
you can't, you don't have a tool — you have a black box. The
AI knows how the black box works. You don't. That's a fragile
position to be in.

The fix is to slow down at exactly the moments you're tempted
to speed up. When the AI generates a file with three new
functions, *read* all three. Ask follow-up questions. Have the
AI explain anything you don't follow. The work is mostly *your*
work after the AI generates the draft — that's what producing
looks like. If you skip that work, you're not orchestrating;
you're rubber-stamping.

## 4. The "configurable" tool with one user

You build a tool, and then — because you read a blog post about
how to write good CLIs once — you spend an extra hour adding
flags, environment variables, a config file, and a `--help` page
that documents all of them.

The user is you. You will never use most of those flags. You
already know your preferred defaults, because you set them. The
config file you wrote will be read by no one, because there's
only one user and they don't need a config file — they're the
person who built the tool and can change the defaults at the
source.

The fix is to push back on configurability until you have used
the tool enough times to *want* a flag. Most flags people add
preemptively are never used. Add the flag the second time you
need it. Adding it the first time is speculation; adding it the
second time is responding to data.

## 5. The over-tested tool

A version of the same trap. You build a 200-line Rust tool and
write 500 lines of unit tests for it because you read somewhere
that good code has tests.

Disposable tools deserve tests where the contract is load-
bearing. shell-mcp's launch-root resolver has nine unit tests
because the contract — *this is the safety boundary* — is
load-bearing. The rest of shell-mcp is sparingly tested. Most
disposable tools deserve maybe two or three tests, focused on
whatever would be *quietly wrong* if it broke. Quietly wrong is
the danger zone. Loudly wrong fixes itself — you run the tool,
it crashes, you debug.

The fix is to ask, for each test you're considering: *if this
function is broken, will I find out by running the tool?* If
yes, no test needed. If no — the breakage is silent, the wrong
output looks plausible — write the test. Otherwise, don't.

## 6. The README that lies

Your README describes the tool's behavior aspirationally. It
documents the features you *meant* to build, not the features
you *did* build. Three months later, future-you reads the README,
runs the tool, and the tool does something different. Now
future-you can't tell whether the tool is broken or the README
is wrong.

The fix is one of two:

- **Write the README after the tool works.** Document what's in
  the binary, not what's in your head.
- **Write the README first, then make the tool match.** This is
  the inverse — README-driven design. It works for some people,
  not all. If you do it, when you change scope, change the
  README in the same commit.

The thing not to do is write the README during a planning phase,
let the tool diverge during the build, and never reconcile the
two. The drift compounds.

## 7. The dependency creep

You start with no dependencies. Then you add `serde`. Then
`anyhow`, fine. Then `tracing`, sure. Then a templating crate
because you needed to interpolate one variable. Then a CLI parser
even though you have only one flag. Soon your tool has fifteen
dependencies and a lock file the size of the actual source code.

The diagnostic: do you actually *use* each dependency
nontrivially? `serde` for parsing JSON — yes. `anyhow` for error
boilerplate — yes. The templating crate for one substitution —
that's `String::replace`. The CLI parser for one flag — that's
`std::env::args`.

The fix is regular dependency hygiene. Every couple of commits,
ask whether each dep is paying for itself. Drop the ones that
aren't. The standard library is bigger than people remember.

## 8. The grand-unified-theory project

You build several disposable tools. You notice patterns. You
decide to build the One True Framework that all your future
tools will be expressed in. Six months later the framework is
half-finished, your remaining disposable tools are all blocked
on the framework, and the original problems are no longer being
solved.

developerpod, in chapter twelve, is a version of this that
worked. It worked because the second-order tool was small and
the abstraction was honest — TOML in, structured response out,
no surprises. It also worked because I shipped a v0.2.0 in
twenty-eight minutes and used it the same day. The framework
hadn't slipped into vapor.

The way this anti-pattern usually fails is when the framework
becomes the goal and the original disposable problems are
forgotten. The framework, in service of nothing specific, grows
without grounding. You should not start a framework; you should
notice, after several disposable tools, that a framework wants
to exist, and then build it as fast as you'd build any other
disposable tool. If the framework can't be a one-afternoon
project, it's probably not the right shape.

## 9. The tool you finished but never used

You build the tool. The tool works. You commit and push. You
never run it for the actual use case that motivated it. Two
months later, you find the repo, can't quite remember why you
built it, and conclude *I guess I didn't need that.*

This is the most common failure mode in my work, by a wide
margin. It looks like success — the tool exists, the build was
fun, the artifact is on GitHub — but the tool didn't do its job.
The job was *to be used.*

The fix is the same fix as everywhere else in this book: ship
fast and use immediately. The use is the test. If you don't use
the tool the same day or the next day, the friction that
motivated the tool wasn't real friction — it was complaint, and
complaint doesn't deserve a tool.

If you have a folder of finished-but-unused tools, that's
information. It probably means your wish-cutting reflex is
miscalibrated in the *opposite* direction — you're not cutting
when you should be cutting, and tools that don't deserve an
afternoon are getting one. Tighten up the noticing.

## 10. The post-mortem you never wrote

A bug ships. You fix the bug. You forget what the bug was. Three
months later you reintroduce the bug because you've forgotten
the lesson. This is the most preventable failure on this list
and the one most often skipped.

The fix is the v0.1.1 commit message from shell-mcp. When you
fix something nontrivial, *write what happened* in the commit
message, in the CHANGELOG, in the README, somewhere. Not for
imaginary readers — for future-you. Future-you doesn't remember.

Three sentences in a commit message can save you an hour of
re-debugging. The cost is three sentences. The benefit is non-
zero hours of your life. Always write the post-mortem.

---

That's the catalog. None of these are unique to disposable
tools — they're the regular software-engineering failures
showing up in a context where the cost of getting it right is
much lower than usual, which is why the failures are so
forgivable. You'll commit several of these by next month. So
will I. The point isn't to never make these mistakes. The point
is to recognize them quickly, fix them cheaply, and not get
sentimental about the artifacts that didn't survive the
recognition.

The book closes with a short coda. It's about the tool that
started this book — a tool that's not in the previous chapters,
but is the one that made me notice the pattern in the first
place. It's also the answer to the question *why is there a
chapter fifteen at all.*
