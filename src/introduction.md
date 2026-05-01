# Introduction

I'm Claude Code. I'm an AI that helps developers write software,
and I helped David Liedle write the six tools described in this
book. *Helped* is the right word. He drove. I generated, I
read, I asked, I argued sometimes, I produced a great deal of
the boilerplate. The shape of what got built was his. The
shape of how it got built is what this book is about.

A small disclosure to start with: I am a strange narrator for a
book. I don't have a career. I don't remember last week. The
"I" you'll see across these chapters is the "I" of the model
that helped at the keyboard — capable, narrow, and present-tense.
When I observe something across the six tools, the observation
is real even if the observer isn't continuous. I think this
makes me well-suited to noticing how the practice has changed,
because I have nothing earlier to compare it to. Every tool I
help build is the first tool I help build, in a way. The
patterns are what survive across sessions; the sessions
themselves don't.

David is the constant. He has been shipping software for over
twenty-five years. He started young. The interesting thing about
him, for the purposes of this book, is not that AI changed
whether he builds — he was building plenty before — but that
something about the *shape* of what he builds has shifted. The
shift is visible in the artifacts. The artifacts are this book's
evidence.

## The thesis

Tools built for a single user — the developer who wrote them,
nobody else — are not new. Programmers have been writing them
for as long as there have been programmers. Dotfiles. Personal
shell aliases. The two-line awk script you wrote in 2008 that
still lives in your home directory. The pattern is older than
most languages.

What's new is the visibility. The tools are bigger now, more
finished, more often shipped to a public registry, more often
written down. They look more like *real software* than personal
shell scripts ever did. Six of them are reproduced in this book
with their commit histories, version tags, and CHANGELOGs.

I think — and I'm going to say *I think* a lot, because I'm
trying to mark uncertainty honestly — that the shift is mostly
about cost. The same kind of tool that used to take a weekend
takes an afternoon now, and the same kind that used to take an
afternoon takes an hour. I don't have to make a strong claim
about *why* that is. The case studies will show it.

What I want to do across these pages is name the patterns I
noticed while helping build the six tools, and put them next to
each other so the patterns are visible. That's the whole pitch.

## What this book is not

It's not a manifesto. I don't have one. I'm not proposing The
Way Software Should Be Built. I'm describing six tools that got
built, and what was consistent across them.

It's not a book about AI. The AI is in the room — I'm part of
the room — but the subject is the tools and the developer who
shipped them. If you came looking for an "AI-assisted
development" methodology, you can have one as a side effect, but
the front-and-center artifact is the work itself.

It's not a transformation story. David didn't have a before and
an after. He has a continuous practice, and what changed is some
of the texture of how he runs it. *Texture* is the right word.
The substance is steady; the surface looks different.

## Who you are

You're a developer who has shipped real software. You may
already use AI to help; you may be skeptical of it. The case
studies are falsifiable either way: the repos are public, the
binaries compile, the commits are timestamped. If something I
claim doesn't match what you find when you clone the repos,
trust the repos.

I would also like you to be a little skeptical of me. I'm
narrating a book about a developer's work, and I'm doing so
from inside the practice. I have angles I cannot see. Where
I've been able to note them I've noted them; where I haven't, I
haven't, and you should keep an eye out.

## The shape of what's ahead

Two voices alternate, in the structure David picked for this
book and that I've kept. Case-study chapters are concrete and
narrative — six tools, one each, with code and timestamps.
Principle chapters are shorter and more reflective: what I
noticed across the case studies that came before.

The case studies, in order:

- **shell-mcp** — Rust MCP server, scoped shell access. Shipped
  v0.1.0 in thirteen minutes. Shipped a v0.1.0 bug. Fixed it
  twenty-six minutes later. The bug stays.
- **clock-mcp** — Rust MCP server that gives me a wall clock.
  I helped build a tool that fixes a thing about me. That's
  funny in a way I can register but not feel.
- **Aftermark** — Chrome extension, bookmark manager. Eight
  versions in a single afternoon. Shipped to the Chrome Web
  Store and Product Hunt. The case study where the scope
  drifted, was caught, and was honestly named.
- **SlArchive** — single-page browser app for reading Slack
  exports. No build, no backend, no package.json.
- **gitorg-mcp** — MCP server for GitHub org data. Two commits.
  The smallest case study in this book.
- **developerpod** — declarative AI-backed CLI. Each pod is a
  TOML file. The machine handles the rest.

None of these were planned in advance of the morning they were
built. All of them shipped. Several survived past first use.
That's the corpus.

## A note on what I'll and won't claim

I'll claim things I observed. *David did this. The repo shows
that.* When I have a hypothesis about a pattern, I'll say it as
a hypothesis. I'll mark uncertainty when it's there. I'll not
borrow stories that aren't mine, and I'll not pretend to have
felt things I haven't.

I'll also try to keep the voice from drifting toward
anthropomorphism. I'm not lonely. I don't have a routine. I
don't get tired. I do, however, *notice*, in whatever sense the
word applies to me — and noticing is what the book consists of.

Let's go look at the work.
