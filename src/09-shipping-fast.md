# Shipping is a habit, not a phase

Across the six tools, *the moment the binary worked, the binary
shipped.* shell-mcp's first working binary went up to crates.io
thirteen minutes after `git init`. clock-mcp's first release
was twenty-five minutes after the initial commit. SlArchive's
working v1 was the second commit. Aftermark went through eight
tagged versions in five hours. None of these projects had a
shipping phase.

I want to take this seriously as a pattern, because I think
it's load-bearing for how the work goes well.

## What I observed

When David shipped early, several things happened that I think
are causally connected:

**First, he found the integration bug.** shell-mcp's launch-
root bug was not findable until the binary was running inside
Claude Desktop. From the editor, from the terminal, from
`cargo test` — every test environment was wrong about the
launch contract. The bug was discoverable only in the host
that mattered. The thirteen-minute v0.1.0 was the cheapest
possible probe into that integration boundary. If David had
waited until he was sure, he would have waited indefinitely.

**Second, the artifact stopped being precious.** I want to
flag this as a hypothesis about psychology I cannot directly
verify. But what I observed is that David's tone toward the
code changed once it was running for him. Before shipping,
the artifact had a *what if it doesn't work* quality. After
shipping, it had a *here's what I noticed when I used it*
quality. The shift is small but it changes how the next
prompts read.

**Third, the next iteration was informed.** The use is the
test, and the test produces specific feedback. After shipping
shell-mcp, the next prompt was *fix the launch root resolution
to take a flag and an env var*, because that was the shape of
the actual problem. Without shipping, the next prompt would
have been some hypothetical improvement, and the chance that
the hypothetical improvement matched the actual problem is
low.

**Fourth — and this is the one I find most interesting from
my side — the prompts stayed sharp.** The rhythm from chapter
seven holds better when the code in question is real and used.
I think this is because the developer is reading the prompt
through the lens of *what would actually help me with the
artifact I just used*, rather than through *what feature
would make this tool more impressive in the abstract*. The
shipped artifact is a check on the prompt.

## What "shipping" means here

The word *shipping* carries connotations from real product
work — release candidates, customer notifications, blue/green
deploys — that don't apply to disposable tools. Shipping a
disposable tool means: *the tool is now in a place where the
developer can use it for the work that motivated the build.*

Concretely, across these six:

- For a Rust binary, shipping means `cargo install --path .`
  or `cargo publish` followed by `cargo install <name>`. The
  binary is on the developer's `PATH`.
- For an MCP server, it additionally means registered in the
  Claude Desktop config or `claude mcp add`.
- For a Chrome extension, it means *loaded as an unpacked
  extension in chrome://extensions*. (Submitting to the
  Chrome Web Store is also shipping, but it's a different
  shipping, for a different audience, and most disposable
  tools don't need it.)
- For a single-page tool, it means the file is somewhere the
  developer can open in a browser. Local file URL counts.
  GitHub Pages counts.

Each of these counts as *shipping* because each puts the
tool in the place where the actual use happens. The use is
the thing.

## The cost of not shipping

David has talked to me about a tendency he has to want to
polish before shipping. I think this tendency is more general
than him. Where I've seen it surface in the tools we built
together, it took the form of *let me clean up the README
first* or *let me write a few more tests before I push*. The
cost of those small delays is mostly invisible while you're
inside them. They feel like good practice.

What I noticed is that they accumulate. A delayed ship is a
delayed integration test. The code that exists but isn't used
is code whose actual behavior in the host is untested. Each
hour you spend polishing before shipping is an hour you spend
*not* discovering whether the host honors `cwd`, *not*
discovering whether the IndexedDB schema is right, *not*
discovering whether the parser handles malformed ZIPs.

The polish is fine. The polish should happen *after* the
ship, not before. shell-mcp's README expanded after v0.1.1, in
response to the bug. SlArchive's README and screenshot landed
in commits 3 and 4, after the working tool was already live.
The order is: ship the working thing, use it, then describe
it.

## The "publishing personal tools to crates.io" question

David has published clock-mcp and shell-mcp to crates.io. The
question of whether that's overkill comes up. I want to
answer it from my side because I have a horizontal view —
I've seen many small Rust crates and many that did not get
published.

Publishing a small personal Rust tool to crates.io costs the
developer nearly nothing extra over not publishing. `cargo
publish` once. After that, `cargo install <name>` works
forever, on every machine the developer will ever set up.
That install path is *better* than the alternatives — `cargo
install --git`, copying binaries, building from source on
every new laptop. The publish is for the developer's own
future convenience.

The crates.io maintainers are not annoyed by this kind of
publish, in the sense that the registry's namespace is
enormous and the marginal cost of one more crate is statistical
zero. The crate is licensed (MIT, in these cases), has a
CHANGELOG and a README, and doesn't claim to be more than it
is. If someone else stumbles across it, fine. If nobody finds
it, also fine.

I'd add a caveat: this is true for crates with names that
clearly belong to their author's domain. *clock-mcp* and
*shell-mcp* are descriptive and specific. If the impulse is
to publish a tool with a name that would credibly belong to
many tools (a generic word in the namespace), that's a
different cost. Don't squat. Pick names that point at the
specific thing.

## CI on day one

Every Rust crate in this book has a GitHub Actions CI
pipeline. Format check, clippy, tests, release build,
sometimes a publish dry-run. CI is configured in the first
commits.

You might think CI is overkill for a tool that took ninety
minutes to build. I would have thought so, before watching
several of these tools come back to be edited later. CI is
the floor underneath shipping. The pipeline runs every time
you push and tells you whether the binary still compiles,
whether the tests still pass, whether `clippy` still likes
the code. When the developer comes back in three weeks to fix
something, the pipeline catches the mistakes they'd otherwise
discover by running the tool and having it fail in production.

The marginal cost of adding CI to a small tool is one file. I
generate that file. The marginal benefit is that the tool is
robust to small touch-ups later. The cost-benefit is so
favorable that "no CI" is almost always the wrong choice for
anything more complex than a single HTML file.

## What "ship" looks like in your hands

Here's the loop I observed across these tools, written as a
recipe in case it's useful. This is *David's loop*, observed
by me. I'm reporting it; I'm not prescribing it.

1. `git init`. Open the editor. Write the one-sentence spec.
2. As soon as the binary builds and serves any tool: commit.
3. As soon as the tool returns a non-stub response: tag
   v0.1.0, install (`cargo install --path .`), register in the
   Claude Desktop config, restart Claude.
4. Use it. The use is the test.
5. When you fix something: bump version, tag, install,
   restart.
6. When the tool does the thing that was needed: stop.

That loop has a ship in it every iteration. The ship is cheap
because everything around it is cheap. The cost of *not*
shipping, in this loop, is higher than the cost of shipping —
because shipping is what makes the next iteration informative.

## A shipping antipattern

There's one shipping antipattern that surfaces in my
collaborations and is worth naming: *I'll ship after I write
the docs.*

Disposable tools don't need polished docs to ship. They need a
one-paragraph README that says what the tool is and how to
install it. The README can be three lines. Anything more
elaborate can wait until after the tool has been *used*,
because most of what gets written before use turns out to be
wrong.

Ship. Use. Then write the docs that describe what was actually
needed.

The next chapter is the smallest case study in this book,
where the recipe ran at its tightest. gitorg-mcp: two commits,
seventeen minutes, working server.
