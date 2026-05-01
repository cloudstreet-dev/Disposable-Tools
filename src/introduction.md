# Introduction: tools for an audience of one

Most software writing is about building for users — plural,
hypothetical, to-be-discovered. Personas, total addressable markets,
adoption curves, the whole long catechism. This book is about the
other thing.

This book is about the tool you build for yourself, this week,
because nothing else does the job. The audience is a single,
known, present-tense developer: you. Not a market. You.

That used to be a strange thing to want. Bespoke software was
expensive, and the cost was paid in your weekends. So you absorbed
the friction of whatever existed: you exported Slack to a folder of
JSON nobody could read, you typed `date -u` ten times an hour into a
chat with a model that didn't know what year it was, you scrolled
through fifty browser bookmarks looking for the one you saved last
Tuesday because none of the existing managers understood that you
had *intended* to come back to it. You absorbed the friction because
the cost of the alternative — a custom tool, designed and built and
maintained by you — was worse than the friction.

That is no longer true.

The cost equation moved. AI didn't make programming free, but it
made bespoke tooling cheap enough that the calculation flipped.
A tool that used to take a weekend can take a Sim block — an hour,
maybe two, sometimes less. Read that not as a productivity boast.
Read it as a change of category. When the cost of building the tool
drops below the cost of tolerating its absence, the answer to "should
I build this?" stops being a question. The question becomes: *what
was stopping me?*

Mostly, what was stopping you was the friction. The friction is
gone now. The book is about what to do with that.

## What this book is not

This book is not about The Future Of Software, building agents that
autonomously run your business, or 10x productivity. It is not a
manifesto. It is not a methodology you will license. There is no
framework here. There is no acronym. The author has watched enough
people pour their savings into "AI-native" subscriptions to want to
keep some distance from that whole vocabulary.

This book is also not the meta-meta book about writing books with
AI. The book you are holding was, in fact, written with AI, and that
deserves a footnote, but only a footnote. The subject of the book is
not the book. The subject is the tools.

What this book *is* is a journal with structure. Six tools — real
tools, real code, real GitHub URLs, real bugs — and the patterns
that fall out when you put them next to each other. The tools came
first. The patterns came later, when I noticed I kept doing the same
things.

## Who you are

You're already a developer. You've shipped real software for other
people and you have working opinions about what makes a project
go badly. Your bullshit detector is on. You don't need a hype lap.

You may already be using AI to write code. Or you're skeptical
of it, in which case the case studies are the part to read first —
they're falsifiable. Clone the repos, run `cargo build`, watch the
binary not lie to you about what it does.

The pieces of advice in this book are first-person observations.
"This worked for me." Not "this is The Way." If a thing here doesn't
match how you work, throw it out. Disposable.

## The shape of what's ahead

The book alternates between two voices. Case-study chapters are
first-person, narrative, with code: the tool, what it does, what
it cost, what went wrong. Principle chapters are second-person and
shorter: the pattern that fell out of the case study before it.

The case studies are real and verifiable:

- **shell-mcp** — Rust MCP server, scoped shell access. Shipped in
  thirteen minutes. Shipped with a bug. Fixed twenty-six minutes
  later. The bug stays in the story.
- **clock-mcp** — Rust MCP server that gives a language model a
  wall clock. Built because I got tired of telling Claude what
  year it was.
- **Aftermark** — Chrome extension that treats bookmarks as
  intentions instead of links. Shipped eight versions in five hours
  and forty-one minutes.
- **SlArchive** — single-file Slack export viewer that runs entirely
  in the browser. Because the export format Slack hands you is, to
  put it generously, not for humans.
- **gitorg-mcp** — MCP server over GitHub org data. Two commits,
  seventeen minutes, working server.
- **developerpod** — declarative AI-backed CLI. The pod is a TOML
  file. The machine handles the rest.

None of these tools were planned. Each of them shipped because
something specific stopped working, and a few hours of focused
orchestration produced something that fixed the specific thing.
That's the pattern. The chapters are how the pattern shows up
each time, slightly differently.

## A word on "disposable"

The frame is in the title because it's load-bearing. Disposable
isn't about quality — it's about attachment. You build a thing,
you use it, you don't get sentimental about it. If it survives,
fine. Most of them won't. Two of the six tools in this book have
already been used exactly once. One of them got published to
crates.io and probably will be again. Both outcomes are correct.

The work is the use. Not the artifact.

Let's go.
