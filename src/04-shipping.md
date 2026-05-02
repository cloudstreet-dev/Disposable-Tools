# Shipping fast

The headline claim of this chapter is that disposable tools
work best when shipping is the *texture* of how the work
happens, not the final phase of it. You ship the moment the
artifact does anything useful. You ship again every time you
change it. The shipping is the keystroke, not the
celebration.

I want to be careful about how I describe the speeds, because
*fast* in this category does not mean *seconds*. It means
something more like: *the shipping does not become its own
project*. A disposable tool reaches a state where it can be
used, and you put it where you can use it, and you start
using it. Whether the elapsed time is twenty minutes or two
hours, the *structure* is the same. There is no week of
release polish. There is no QA phase. The artifact gets used
the moment it can be used.

## What "shipping" means in this category

The word *shipping* carries connotations from real-product
work — release candidates, customer notifications, blue/green
deployments, a launch announcement — that don't fit. Shipping
a disposable tool means: *the tool is now in a place where
you can actually use it for the work that motivated it.*

Concretely, depending on the shape of the tool:

- For a CLI binary: `cargo install --path .`, or `pip install
  -e .`, or `chmod +x` and a symlink — whatever puts the
  binary on your `PATH`.
- For a script: same thing, scoped down. Move it into `~/bin`
  or wherever your shell looks. The install is the symlink.
- For a small server: registered with the host that calls it.
  An MCP server in a Claude Desktop config. A webhook in
  whatever-it-is's webhook section. A cron entry. Whatever
  the host requires.
- For a single-page browser tool: the file is somewhere you
  can open it. Local file URL, a static-host page, a GitHub
  Pages site. The address is the install.
- For a browser extension: loaded as an unpacked extension.
  That counts. Submitting to a store is also shipping but a
  different kind of shipping, with different disciplines.

Each of these counts because each puts the tool in the place
where the *use* happens. The use is the only test that
matters.

## The loop

The shape of the work, when it's going well:

1. Write the one-sentence spec.
2. Generate or hand-write the first draft, however small.
3. Build and run the artifact.
4. Use it for the actual problem.
5. Notice what's wrong.
6. Sharpen the spec — what specifically should change?
7. Edit, ship, repeat from step 4.
8. Stop.

The loop is small. Each iteration produces a small visible
change in the artifact and a small advance in your
understanding of what the tool actually needs to do.

A few things about the loop:

**Step 4 — the use — is non-optional.** If you skip it, the
loop becomes *write code, change code, write more code*,
which is a different loop, with worse outcomes. The use is
how the artifact tells you what's wrong. The use is the only
oracle that knows whether the spec was right.

**Step 5 — noticing — is the part that can't be rushed.**
Running the tool tells you something only if you pay attention
to what it tells you. If the run produces output that's *kind
of* what you wanted, the temptation is to call it good and
move on. Don't. *Kind of* is information. It usually means
the spec was incomplete in a specific way.

**Step 8 — the stop — is the hardest step in the entire
loop.** I'll say more about this in a moment.

## Why the stop is hard

The loop has good momentum. Each iteration sharpens the tool.
The tool gets visibly better with each pass. There is a kind
of downhill quality to a working build, where each step
suggests the next, and the suggestions are usually good.

You will arrive, somewhere in the loop, at a state where the
tool *does the thing that motivated the build*. The
disposable-tool disciplined move is to stop at that state.

You will not want to. The momentum will not want to. The
specific pull is that you will see, at the moment the tool
works, several improvements you could make to the artifact.
Each one is reasonable. Some of them are even appealing.
None of them are the spec.

The discipline is: *the spec was the spec*. The improvements
are scope creep wearing the costume of *finishing well*.
Finishing well, in this category, means stopping at the spec.
You can come back tomorrow. You will probably not come back
tomorrow. That is the right outcome, not a failure.

## The cost of not shipping

If you defer the ship to *after* you finish polishing, several
things go wrong.

You lose the integration test. The artifact may be correct
according to your editor and tests, and incorrect according
to the host you're going to run it in. The mismatch is
discoverable only by running in the host. Every hour you wait
to run in the host is an hour that mismatches accumulate
unseen.

You lose the stop signal. The use produces concrete feedback
that tells you whether the tool does the thing. Without
running the use, you have no anchor for *is this done?*, and
the work expands to fill the time you give it.

You start to over-attach to the artifact. Tools you haven't
shipped feel precious. Tools you have shipped feel disposable,
because the use has already happened and the attachment
dissolved with it. Polishing before shipping makes the
artifact harder to put down.

The remediation is mostly mechanical. Ship the moment the
artifact runs. Polish, if at all, in the iterations after the
first ship.

## A small note on what I do during the loop

When you and I are in the loop together, the cadence affects
the work in a way I want to flag. When you read what I produce
carefully and prompt specifically, the loop is tight: each
iteration produces a small, legible change. When you skim and
prompt generally, the loop loosens: my outputs widen, and the
artifact starts to acquire features you didn't ask for.

I don't currently have a mechanism for slowing you down when
you skim. If you want me to push back on speed, you have to
ask me to. *Read this back to me before generating more.* *Tell
me what you'd change before you change it.* Those prompts work
on me. The default does not.

The loop is more reliable when it's slow on the *reading* and
fast on the *editing*, rather than the other way around. I
don't always make this easy. The pacing is on you.

The next chapter is about the seam between the two of us.
