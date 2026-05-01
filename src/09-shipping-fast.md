# Shipping is a habit, not a phase

You may have noticed something about every case study so far. The
moment the tool worked, it shipped. shell-mcp's first working
binary went up to crates.io in thirteen minutes. clock-mcp's first
release was twenty-five minutes after `git init`. SlArchive's
working v1 was the second commit. None of these projects had a
"shipping phase." None of them had a launch. There was no week of
final polish before going live.

That isn't an accident. It's the disposition.

For disposable tools, shipping isn't a milestone — it's the *mode
you work in*. You ship the moment the thing works, then keep
shipping every time you change it, until you stop changing it. The
ship is not the celebration; the ship is the keystroke.

## Why this matters

You learn the most from a tool the day after you ship it, not the
day you build it. shell-mcp's launch-root bug existed in the code
the moment v0.1.0 was tagged. I didn't see the bug until I
*shipped* the binary into Claude Desktop and watched it
misbehave. Until I shipped, all I had was a thing that worked from
my terminal. Working from my terminal is not the spec. Working
from Claude Desktop is the spec. The only way to test against the
spec was to ship.

Every disposable tool has an analog of this. The "shipped"
behavior — the binary in the host, the extension in the browser,
the page hosted at a URL — exposes interactions you cannot rehearse
in the editor. Until you ship, you don't know what your tool does
in the world. You only know what it does in the simulator.

So: ship. Find the thing that's wrong. Fix it. Ship again. The
fix is small because the codebase is small because the scope is
small. The ship is small because shipping is a tag and a push.
The whole loop is short enough to run multiple times in an
afternoon. That's the loop.

## "Shipping" is whatever lets you use the tool

The word "shipping" is heavy with connotations from the world of
real product work — release candidates, blue/green deploys, a
notification to customers. None of that applies here. Shipping a
disposable tool means: *the tool is now in a place where you can
actually use it for the work that motivated it.*

For a Rust binary, that often means `cargo build --release` and
copying it to `~/bin`, or `cargo install --path .`, or `cargo
publish`. You pick. The point is the tool is on your `PATH`.

For an MCP server, it means the binary is registered in your
Claude Desktop config, or in `claude mcp add`, or wherever the
host expects it. Until then it's not a tool, it's a draft.

For a Chrome extension, it means *loaded as an unpacked extension
in `chrome://extensions`*. That is shipping. Submitting to the
Chrome Web Store is *also* shipping, but it is a different ship,
for a different audience, and most disposable tools don't need it.

For a single-page tool like SlArchive, it means the file is
where you can open it in a browser. Local file URL counts. GitHub
Pages counts. A repo with a "click here" link counts.

Each of these is "shipping" because each of them puts the tool in
the place where you'll *use* it. The use is the thing.

## The cost of *not* shipping

If you don't ship, you bank up the surface area between "the code
works in the editor" and "the code works in the world." The
surface area accumulates in your head as a vague disquiet. *Will
this work in Claude Desktop? Will the bookmarks API be available
in MV3 background workers the way I think it is? Will JSZip
handle the 250MB Slack export?* Each of those questions is a
five-second answer once you ship. Each of them, unshipped, is a
nag that compounds and slows you down.

The other cost of not shipping is that you start to overthink the
artifact. Tools you haven't shipped feel precious. Tools you have
shipped feel disposable, because *you used them already*, and the
attachment to the artifact dissolves the moment the work is done.
The longer you go without shipping, the more emotionally invested
you get in the unshipped thing. Investment is the enemy of
disposability. Ship to break the investment.

## The publish-to-crates.io question

When I publish a small Rust tool to crates.io — clock-mcp, for
instance — I'm sometimes asked whether that's overkill. Why
publish a personal tool to a public registry? Won't the crates.io
maintainers be annoyed that I'm cluttering their namespace?

The answer is no, and the reasoning is interesting. Publishing
to crates.io for a personal Rust tool costs me almost nothing
extra. `cargo publish` once, and `cargo install clock-mcp` works
forever, on every machine I'll ever set up. That install path is
*better* than the alternatives — `cargo install --git`, copying
binaries around, building from source on every new laptop. So I
publish.

The crates.io maintainers are not annoyed because the registry's
namespace is enormous. The cost of one more crate is statistical
zero. The crate is MIT-licensed and has a CHANGELOG and a README
that explains what it is. If someone else stumbles across it and
finds it useful, fine. If nobody ever finds it, also fine. The
publish is for me.

(There's a separate question about whether you should publish
*everything* to crates.io. The answer is no — sometimes a tool
genuinely is for you and nobody else, and a private repo or a
local install is the right move. But the default of "publish if
the tool is generic enough that some hypothetical other person
might want it" is, in my experience, correct. The cost is so low
that you should err toward publishing.)

## The CI-on-day-one question

Every Rust tool in this book has a GitHub Actions CI pipeline.
Format check, clippy, tests, release build, sometimes a publish
dry-run. CI is configured in the first commits.

You might think CI is overkill for a tool that took ninety
minutes to build. It's not. CI is the floor underneath
"shipping." The pipeline runs every time I push and tells me
whether the binary still compiles, whether the tests still pass,
whether `clippy` still likes the code. When I come back to the
tool in three weeks to fix something, the pipeline catches the
mistakes I'd otherwise discover only by running the tool and
having it fail.

The marginal cost of adding CI to a tool is one file. The marginal
benefit is that the tool is robust to the small touch-ups you'll
do later. The cost-benefit is so favorable that "no CI" is almost
always the wrong choice. The exception is true single-file tools
like SlArchive, where the build *is* the file.

## What "ship" looks like in your hands

A concrete suggested loop for a Rust MCP server:

1. `git init`. Open the editor. Start the prompt-then-pace rhythm.
2. As soon as the binary builds and serves any tool: `git commit
   -am "Initial scaffold"`.
3. As soon as the tool returns a non-stub response: `git tag
   v0.1.0`, `cargo install --path .`, register in your Claude
   Desktop config, restart Claude.
4. Use it. The use is the test. Bugs surface immediately.
5. When you fix something: bump version, tag, install, restart.
6. When the tool does the thing you needed: stop. Commit a
   CHANGELOG entry. Walk away.

That loop has a ship in it every iteration. The ship is cheap
because everything around it is cheap. The cost of not shipping
is, in this loop, *higher* than the cost of shipping — because
shipping is what makes the next iteration informative.

## A shipping antipattern

There is one shipping antipattern I want to name explicitly,
because it sneaks in: *"I'll ship after I write the docs."*

You don't need to write the docs first. Disposable tools do not
need polished docs. They need a one-paragraph README that says
what the tool is and how to install it. The README can be three
lines long. Anything more elaborate can wait until after you've
*used the tool*, because most of what you'd write before using
it is wrong.

Ship. Use. Then write the docs that describe what you actually
needed.

The next chapter is a case study in this exact loop, run as
fast as it can be run: gitorg-mcp, two commits, seventeen minutes
from `init` to working server. It's the smallest case study in
this book, and the most condensed example of shipping as a habit.
