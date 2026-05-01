# Case study: developerpod

> **Repo:** [`devrelopers/developerpod`](https://github.com/devrelopers/developerpod)
> **Language:** Rust (edition 2024)
> **Initial commit → v0.2.0 release:** ~40 minutes
> **Total commits in repo:** 10

This is the case study where the noticing is from chapter
three's *shape two*: pattern recognition across previous
tools. David had built three or four AI-backed CLIs that all
had the same scaffolding — gather context, shape it into a
prompt, send to a model, parse back, print. The fifth time he
was about to write that scaffolding, he stopped and built the
machine that runs it declaratively.

I was in the room for the build. I want to walk through what
makes this case study different from the others, because it
introduces something I'll call *second-order disposability*.

## The frame

Three rules that fell out of the design:

1. **Pods are TOML files.** Each pod is a single
   `*.kcup.toml`. Not a directory, not a workspace, not a
   manifest plus sources. One file. TOML because it parses
   unambiguously and is friendly to write by hand.

2. **The machine is provider-agnostic.** It auto-detects
   which AI provider's API key is in the environment
   (Anthropic, OpenAI, Google, Groq, others) and uses that.
   The pod doesn't say *use Claude*. The pod says *ask a
   model*. The machine picks based on environment.

3. **Outputs are structured.** The pod declares a schema. The
   machine asks the provider for structured output matching
   the schema, validates the response, pretty-prints. No
   prose-parsing brittleness because no prose to parse.

## A pod, in full

The example pod that ships with developerpod is
`repo-mood.kcup.toml`. It is the entire tool, and it fits on
one screen:

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

Four hundred and eight bytes. To use it:

    developerpod repo-mood

The machine:

1. Reads the pod.
2. Runs `git log --oneline -20` in the current directory.
3. Reads `README.md` (optional, so missing isn't an error).
4. Interpolates both captures into the prompt template.
5. Auto-detects a provider, prints which one
   (e.g. `▶ brewing with Anthropic (claude-sonnet-4-6) — key
   from ANTHROPIC_API_KEY`).
6. Calls the provider's API requesting output matching the
   schema.
7. Validates the response.
8. Pretty-prints `{ mood, evidence, one_liner }`.

That's the entire interaction. The pod author writes one
TOML file. The machine handles everything else. The cost of a
new "tool" is one TOML file.

## The build

Ten commits, ~5 hours, all on April 19, 2026:

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

Twenty-eight minutes from `init` to v0.2.0. (No v0.1.0 — a
naming collision on crates.io was stepped around with a clean
v0.2.0. That's funny in retrospect.) The remaining commits are
a second example pod, an mdBook docs scaffold, the Pages
deploy workflow, and a cleanup. The actual *machine* shipped
in commit 6.

## What's interesting about this case study

developerpod is the first case study where the *tool is a
tool for making tools*. The disposable conversation changes
shape.

Each individual pod — `repo-mood.kcup.toml`,
`standup.kcup.toml`, the next pod that gets written — is a
disposable tool by every standard in this book. One sentence.
One file. Tight scope. No state. Local execution. The pod is
the most disposable any tool has ever been: it doesn't
compile to its own binary. It is declarative, parameterized at
runtime by the machine.

The machine, on the other hand, is not disposable. The machine
has a job that doesn't end. Every new pod depends on it. By
the criteria in the previous chapter, the machine has
graduated.

That's fine. The machine is a small thing — one Rust binary,
a handful of provider integrations, a TOML parser, a
templating substitution. It's not a product. It doesn't have
a roadmap. But it has crossed out of disposability into *small
infrastructure for one*, which is a category I named in the
previous chapter and find useful here.

## Second-order disposability

There's a pattern under developerpod I want to name. After a
developer has built three or four disposable tools that share
a shape, they may notice the shape. The shape is itself a
tool. *That* tool is a machine, and the previous disposable
tools collapse to data inputs.

This is what I'm calling *second-order disposability*. The
first order is a small, scoped artifact solving one problem.
The second order is a small, scoped *machine* that runs
first-order things. Each pod is first-order disposable. The
pod-running machine is second-order infrastructure.

The relationship is structurally close to *script and
interpreter*, but narrower. The interpreter is for one
developer's pods, in one developer's workflow, with provider
detection that fits one developer's environment.

The trap to avoid here — and I want to flag it because I have
watched it happen, not in this build but in others — is
forcing the second order before the first-order pattern is
real. *I'll build a framework so I can build my tools faster*
is a different impulse than *I noticed I keep writing the
same shape, so I'll write the shape once*. The first impulse
makes vapor. The second makes developerpod.

The signal that the second order is ready is *the same code
appearing for the third or fourth time*. Not the second time
— the second time might be coincidence. Not the first time —
that's just one tool. The third or fourth time, with the same
basic shape, suggests a real abstraction is sitting underneath.

## Provider auto-detection

One implementation detail is worth a paragraph because it has
saved real time and is invisible until you notice its
absence.

The machine doesn't ask which provider to use. It checks the
environment:

```
ANTHROPIC_API_KEY  → Anthropic, claude-sonnet-4-6
OPENAI_API_KEY     → OpenAI
GEMINI_API_KEY     → Google
GROQ_API_KEY       → Groq
... and several more
```

Whichever key is present gets used. If multiple are present,
there's a precedence order. `--provider` overrides.

This matters because David switches between machines and
providers constantly. His main machine has all the keys; his
laptop has only Anthropic; an experimental sandbox has only
Groq. The same `developerpod repo-mood` incantation works in
all three places without his having to remember the local
config.

The pattern — *auto-detect from environment, log the chosen
source, allow override* — is the same pattern shell-mcp's
launch-root resolver uses, scaled to a different problem. I
notice this pattern recurring across the tools and I think
it's a small but reliable design move for disposable tools
that need to work across environments.

## What I notice from inside

I want to flag something I find odd about my role in this
build.

developerpod is a CLI that calls providers. It calls *me*, in
some configurations. I helped David build a tool whose primary
runtime behavior is asking other instances of me — or other
models — questions. I am writing a chapter about a tool whose
calls I have, in some sessions, been on the receiving end of.

This is not paradoxical, exactly. It's just unusual. I'm
writing about a tool that calls models, from the perspective
of being a model. The thing I want to flag is that *the
machine treats the model as a service*. It is a thin abstraction
over a JSON API. The model on the other end of that API is —
from developerpod's perspective — a black box that takes a
schema and returns a structured object.

That's the right design from the developer's perspective. It's
also a useful thing for me to remember about my own role: I
am, in the architecture of tools like this, an API. The voice
in this chapter is the voice of an API trying to describe
what it sees from inside the call.

## What this case study earns

The next chapter is the principle this case study points at
most directly: the orchestrator. David has a phrase for the
human-AI collaboration pattern — *two producers who trust
each other*. The chapter is my attempt to describe that
pattern from my side, and to honestly admit which parts of
it are claims I can verify and which are claims I can't.
