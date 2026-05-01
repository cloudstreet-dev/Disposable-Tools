# Case study: developerpod

> **Repo:** [`devrelopers/developerpod`](https://github.com/devrelopers/developerpod)
> **Language:** Rust (edition 2024)
> **Initial commit → v0.2.0 release:** ~40 minutes
> **Total commits in repo:** 10
> **First commit window:** ~5 hours, all on April 19, 2026

## The thing I wanted

I had built a few disposable tools that all looked structurally
similar: gather some context (run a shell command, read a file),
shape it into a prompt, send it to a model, parse the response,
print the result. Each one was a custom CLI that did roughly
those four steps. The fifth time I started a new tool of this
shape, I noticed I was writing the same scaffolding again.

That's a signal. Not the friction signal from chapter three —
this one is different. This is the *I keep writing the same
glue* signal. When you find yourself reaching for the same
plumbing on the third or fourth disposable tool, the glue itself
is a tool waiting to happen.

What I wanted was a *machine* — a small CLI that ate a
declarative file describing the four steps and ran them. Each
declarative file would be a "pod." The machine would brew the
pod. (The metaphor is silly. The metaphor is also the name.
You don't have to like it.)

## The frame

Three rules that fell out of the design:

1. **Pods are TOML files.** TOML because it's friendly to write
   by hand, parses unambiguously, and is already in my Rust
   toolchain. Each pod is a single `*.kcup.toml` file. The whole
   pod is one file — not a directory, not a workspace, not a
   manifest plus sources. Pods are portable: drop one in a repo
   and any developerpod can brew it.

2. **The machine is provider-agnostic.** It auto-detects which
   AI provider's API key is in your environment (Anthropic,
   OpenAI, Google, Groq, others) and uses that. The pod doesn't
   say "use Claude." The pod says "ask a model." The machine
   picks the model based on what credentials the user has
   present.

3. **Outputs are structured.** The pod declares a schema. The
   machine asks the provider for structured output matching that
   schema, validates the response, and pretty-prints. There's no
   prose-parsing brittleness because there's no prose to parse.

## A pod, in full

Here's the entire `repo-mood.kcup.toml`, the example pod that
ships with developerpod:

```toml
name = "repo-mood"
description = "Read the current vibe of a git repo"

[[gather]]
id = "commits"
shell = "git log --oneline -20"

[[gather]]
id = "readme"
file = "README.md"
optional = true

[prompt]
system = "You read repo signals and return the current mood."
user = """
Recent commits:
{{commits}}

README:
{{readme}}
"""

[output]
schema = { mood = "string", evidence = "string", one_liner = "string" }
```

That is a complete tool. Four hundred and eight bytes. To use it:

    developerpod repo-mood

The machine:

1. Reads the pod.
2. Runs `git log --oneline -20` in the current directory.
3. Reads `README.md` (optional, so if it's missing, no error).
4. Interpolates both captures into the prompt template.
5. Picks a provider from environment variables — prints which
   one, e.g. `▶ brewing with Anthropic (claude-sonnet-4-6) — key
   from ANTHROPIC_API_KEY`.
6. Calls the provider's API, asking for output matching the
   schema.
7. Validates the response against the schema.
8. Pretty-prints `{ mood, evidence, one_liner }`.

That's the entire interaction loop. The pod author writes one
TOML file. The machine handles everything else — context
gathering, provider selection, structured output, validation,
formatting. The cost of a new "tool" is one TOML file.

## The build

Ten commits. Four hours and fifty-two minutes from initial commit
to "Auto-enable Pages on first workflow run":

```
b923753  15:46:14Z  Initial commit
696a470  15:53:31Z  Initial scaffold: developerpod machine + repo-mood kcup
941372d  16:01:54Z  Auto-detect AI provider across common env vars; support 9
4a893f9  16:05:34Z  Refresh per-provider default models to current April 2026 IDs
ac6114e  16:10:26Z  Expand env var aliases per provider; add Vercel AI SDK + token variants
ad59669  16:14:02Z  Prep crates.io publish: bump to 0.2.0 and add publishing metadata
511d771  20:16:11Z  Add standup kcup: generate standup report from recent git activity
5bb39a6  20:49:39Z  Add docs/ mdBook scaffold and content
7f52090  20:49:39Z  Add GitHub Actions workflow for Pages docs deployment
19c5ae7  20:50:23Z  Auto-enable Pages on first workflow run
```

Twenty-eight minutes from `init` to v0.2.0. (No v0.1.0 — the
publishing metadata bump went straight to 0.2.0 because of a
naming collision on crates.io that I wanted to step around with
a clean version. Yes, that's funny in retrospect.)

The remaining commits are: a second example pod (`standup` —
generate a standup report from recent git activity), a docs
mdBook scaffold, the GitHub Pages deploy workflow, and a "auto-
enable Pages on first workflow run" cleanup.

## What's interesting about this case study

developerpod is the first case study in this book where the *tool
is a tool for making tools.* That changes the disposable
conversation.

Each individual pod — `repo-mood.kcup.toml`, `standup.kcup.toml`,
the next one I'll write — is a disposable tool by every standard
in this book. One sentence. One file. Tight scope. No state.
Local execution. The pod is the most disposable any tool has ever
been: it doesn't even compile to its own binary. It's
declarative, parameterized at run time by the machine.

The machine, on the other hand, is *not* disposable. The machine
has a job that doesn't end. Every new pod I write depends on it.
The machine has consumers. By the criteria in chapter eleven,
the machine has graduated.

That's fine. The machine is a small thing — one Rust binary, a
handful of provider integrations, a TOML parser, a templating
substitution. It's not a product. It doesn't have a roadmap. But
it has crossed out of disposability into *infrastructure*, and
that's a different category. Infrastructure for an audience of
one is still legitimate. Most of your dotfiles are this. Most of
your shell aliases are this. The machine is in the same family.

## The pattern, more abstractly

There's a pattern hiding under developerpod that I want to name.
After you've built three or four disposable tools that share a
shape, you'll notice the shape itself. The shape is a tool. *That*
tool is your machine, and the previous disposable tools collapse
down to data inputs to the machine.

This is the "second-order disposability." The first order is a
small, scoped thing solving one problem. The second order is a
small, scoped *machine* that runs first-order things. Each kcup
file is a first-order disposable tool. The kcup-running machine
is the second order. The relationship between them is *exactly*
the relationship between a script and an interpreter, but
narrower.

You don't always end up at the second order. Most disposable
tools do not generalize. The ones that do generalize will do so
naturally — you'll notice that you've written the same TOML twice,
the same JSON twice, the same shell glue twice, and the
generalization will write itself. Don't force it. The forced
generalization is a framework, and frameworks for an audience of
one are nearly always overkill.

But when the generalization wants to come, let it. That's
developerpod's whole story.

## Provider auto-detection: a small detail that matters

One implementation detail is worth noting because it has saved
me real time. The machine doesn't ask which provider to use.
It looks at the environment:

```
ANTHROPIC_API_KEY  → Anthropic, claude-sonnet-4-6
OPENAI_API_KEY     → OpenAI
GEMINI_API_KEY     → Google
GROQ_API_KEY       → Groq
... and several more
```

Whichever key is present gets used. If multiple are present,
there's a precedence order, but you can override with
`--provider`.

This matters because I switch between machines and providers
constantly. The machine that gives me my work environment has
all the keys; my laptop has only Anthropic; an experimental
sandbox has only Groq. The same `developerpod repo-mood`
incantation works in all three places without my having to
remember the local config. The auto-detection is a tiny piece of
code and an enormous reduction in cognitive overhead.

This is the kind of detail that earns its keep silently. You
won't notice that auto-detection saved you anything. You'll just
notice that the tool always works, and you'll trust it more.
That's the whole point.

## The next chapter

The chapter after this is the principle developerpod earns most
clearly: the orchestrator. By now you've seen seven case studies'
worth of me-and-the-AI building things together. The pattern of
that collaboration is consistent. The next chapter names it.
