# When a disposable tool isn't disposable anymore

Some of the tools in this book have outlived the afternoons
that produced them. shell-mcp and clock-mcp are still in use,
months on. gitorg-mcp gets called in a Claude session most
weeks. Aftermark went to the Chrome Web Store and Product
Hunt. The disposable-tool frame from chapter one says that
survival is fine but not the spec. *That's the bonus, not the
design.* I want to spend this chapter on what happens when the
bonus comes through anyway.

## The signs of survival

Across the six tools, four signs that a disposable tool has
crossed into something else:

- **Used for more than thirty days.** Soft threshold, not
  hard. Some tools were used once and never again — that's
  the canonical disposable shape. Some are used weekly without
  being thought about. The thirty-day mark is roughly when *I
  built a thing for an afternoon* stops being the accurate
  description and *I depend on a thing I built* becomes
  accurate.

- **The developer would be annoyed if it broke.** This is the
  diagnostic. Ask directly: if `cargo install` of this tool
  stopped working tomorrow because of a Rust toolchain change,
  would the developer notice and fix it? If yes, the tool has
  graduated. If the answer is *I'd shrug and use the
  workaround*, the tool is still disposable.

- **Another tool depends on it.** This is the strongest signal
  and the most consequential. The moment Tool B imports Tool
  A, Tool A is no longer disposable. It has a consumer.
  Disposable tools have one user, not consumers.

- **It's been shared with others, who are using it.** Sharing
  alone doesn't graduate the tool — handing a colleague a
  one-off URL is fine. *Their using it as a dependency*
  graduates it.

When two or more of these are true, the tool has survived
into a different category.

## What I notice about the categories

I want to argue, gently, that *survived* is not the
interesting category — the interesting categories are
*disposable*, *small infrastructure for one*, and *real
product*. They have different disciplines.

**Disposable** is what this book is mostly about. Audience of
one. One sentence. Held loosely. Built in an afternoon, used
for a thing, set down.

**Small infrastructure for one** is a category I think exists
and that the previous chapters mostly didn't name. clock-mcp
is one. shell-mcp is one. gitorg-mcp is one. Each is used
regularly by a single developer for ongoing work, sits in
their environment, and has a maintenance cost approaching
zero. It is not disposable in the sense that the developer
would replace it casually. It is also not a product. It's
infrastructure, but the kind that fits in your hand.

**Real product** is what Aftermark became after v0.4.1.
Audience plural. Roadmap implied. Documentation for users
who are not the author. Privacy policy. Store listing. This
category has its own disciplines that are not in this book.

I find the three-category split clearer than the binary
*disposable / not-disposable* one. The middle category is
where most surviving disposable tools end up, and it has
disciplines that look like the disposable disciplines with
small additions, not like product disciplines at all.

## What changes for small-infrastructure-for-one

Not much, honestly. The cheap upgrades that earn their keep,
across these tools:

1. **Pin the versions of internal dependencies.** If
   gitorg-mcp ever depends on a specific gitorg version, pin
   it. If two of your tools share a substrate crate, pin the
   crate version in both. This is regular software hygiene; it
   matters more once the tools have consumers (even if the
   consumers are also you).

2. **Bother with a CHANGELOG.** clock-mcp has one from v0.1.1.
   shell-mcp does not — its v0.1.1 commit message is the de
   facto changelog. For surviving tools, an explicit
   CHANGELOG.md is almost free and saves the future-self a
   lot of `git log -p`.

3. **Tests where the contract bites.** shell-mcp's launch-root
   resolver has nine unit tests because the contract — *this
   is the safety boundary* — is load-bearing. The rest of
   the crate has integration tests for the write-allowlist
   path. The coverage isn't comprehensive — it's *targeted at
   the parts that would be silently wrong if they broke*.

4. **Decide public vs. private deliberately.** SlArchive and
   developerpod are public. Aftermark is public and
   distributed. Some tools are correctly private. Make the
   call deliberately rather than by default.

That's the entire list. Four items. None of them turn the
tool into "real software" in a way that changes its spirit.

## The professionalization trap

The thing not to do, when a disposable tool survives, is to
*professionalize* it. Issue templates. Contributor guides.
Dependabot. The polite README that explains the development
conventions. Documentation site.

None of that is bad in the abstract. All of it is overkill for
a tool with a team of one. The professional overhead exists
for projects that have *teams* — people who didn't write the
original code and need to learn it, plural contributors
collaborating, the social infrastructure of an open-source
project. A surviving disposable tool has a team of one. The
single-author tool doesn't need an issue template to file an
issue with itself.

I notice this trap most when developers have been steeped in
real-product practices and reflexively apply them. The
practices are right *for the contexts they were developed
for*. They are not right for a Rust binary that does one thing
for one person.

## What if the tool needs to grow?

Sometimes a surviving tool legitimately wants to grow. A new
feature is needed; the underlying problem evolved. The same
prompt-then-pace rhythm works exactly as well on a surviving
tool as on a fresh one.

The trap to avoid is *opportunistic* additions — features
that sound good while you're already in the file. Those are
scope creep wearing a new hat. The discipline that kept the
tool small on day one is the same discipline that should keep
it small on day three hundred. You're not building a product;
you're maintaining a small thing you happen to like.

## When survival is a signal

Occasionally — not often — a surviving disposable tool will
hint at something larger underneath. The pattern, not just
the tool, is what's surviving. The same shape keeps wanting
to apply elsewhere.

If that happens, *give the new thing a different name and
start a new project*. Don't try to retrofit the disposable
tool into the new ambitious shape. The surviving tool is
doing its job where it is. The new project, with the new
ambitions, is its own thing.

shell-mcp is a candidate for this. The pattern of *scoped,
allowlisted, walks-up-like-git* is interesting beyond the
specific server. If that pattern wanted to become a more
general thing — a library other MCP servers use, a shape for
tool-permissioning — that would be a separate project.
shell-mcp the binary stays where it is.

The line is: *surviving doesn't mean ambitious*. Most
surviving disposable tools are content to stay small. The few
that hint at something larger should hint in a new repo.

## Holding it loosely, even after survival

The whole frame of this book is about the loose grip. A
disposable tool held tightly stops being a disposable tool —
even when it's useful, even when the developer would be
annoyed if it broke. The grip is the thing.

If a tool the developer built has survived a year and they'd
be catastrophically inconvenienced if it broke, *that's
information*. It probably means the tool is more important
than its disposable origins suggest. Plan accordingly: more
tests, wider deployment, an actual product mindset. Or,
alternatively, delete the tool and see if the catastrophe is
real. Sometimes the *catastrophic dependency* was a story
the developer told themselves, and the absence turns out to
be fine.

Both responses are valid. The wrong response is to keep using
the tool with a death grip on a dependency they've never
tested. That's the brittleness disposable tools are supposed
to be free of, sneaking back in.

The next chapter is a different kind of survivor —
developerpod, a tool that started disposable and grew into a
small machine for running other disposable tools. The frame
is what survived, more than the tool itself.
