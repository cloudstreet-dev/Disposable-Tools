# When a disposable tool isn't disposable anymore

You will, occasionally, build a disposable tool that doesn't
dispose. You'll keep using it. Months will pass. You'll find
yourself sending the GitHub URL to colleagues, or installing it on
new machines without thinking, or noticing that you'd be annoyed
if it broke.

That's an interesting moment. It deserves a chapter.

## The signs of survival

A disposable tool has crossed into something else when:

- **You've used it for more than thirty days.** This is a soft
  threshold, not a hard one. Some tools you'll use once and never
  again — that's the canonical disposable shape. Some you'll use
  weekly without thinking. The thirty-day mark is roughly when
  "I built a thing for an afternoon" stops being the accurate
  description and "I depend on a thing I built" becomes accurate.

- **You'd be annoyed if it broke.** Test this directly. Ask
  yourself: if `cargo install` of this tool stopped working
  tomorrow because of a Rust toolchain change, would I notice?
  Would I fix it? If the answer is yes, the tool has graduated.
  If the answer is "I'd shrug and use the workaround I had
  before," it's still disposable.

- **You added a dependency on it from another tool.** This is
  the strongest signal and the most dangerous. The moment Tool B
  imports Tool A, Tool A is no longer disposable. Tool A has a
  consumer. Disposable tools don't have consumers. They have one
  user.

- **You've shared it with someone else who is using it.**
  Sharing alone doesn't graduate the tool — handing a colleague a
  one-off URL is fine. *Them depending on it* graduates it.

When two or three of these are true, the tool has survived. The
shape of the survival matters.

## What changes after survival

The good news: not much. You don't have to retrofit the tool with
enterprise machinery. You don't need a roadmap. You don't need a
versioning policy beyond what you already have. The tool is the
same tool. Your relationship to it is what changed.

The thing that *does* change is the cost of breaking it. A
disposable tool can crash; you'll see it crash; you'll fix it or
throw it away. A surviving tool, especially one with consumers,
can crash and *you might not be the one who notices*. That's the
shift.

Practically, this means a few small upgrades earn their keep:

1. **Pin the version your other tools depend on.** If gitorg-mcp
   ever becomes a dependency of another tool of mine, that other
   tool should depend on a specific version, not "whatever's
   latest." This is regular software hygiene, but I name it here
   because disposable-tool builders often skip it.

2. **Bother to write a CHANGELOG.** clock-mcp had a CHANGELOG
   from v0.1.1 onward. shell-mcp does not (yet) have a CHANGELOG;
   the v0.1.1 commit message is the de-facto changelog. For
   surviving tools, a CHANGELOG.md is almost free and saves
   future-you a lot of `git log -p`.

3. **Add tests where the contract bites.** shell-mcp's launch-root
   resolution function has nine unit tests. The rest of the
   crate has integration tests for the write-allowlist path. The
   test coverage isn't comprehensive — that's not the point. The
   tests exist where breakage would corrupt the safety story. If
   you can't articulate which part of the tool would be
   embarrassing if it broke, you don't need tests there. If you
   can, you need tests there.

4. **Decide whether the tool is public or private, and own that
   decision.** Disposable tools live in either category. SlArchive
   and developerpod are public. Aftermark is public but unpublished
   to the Chrome Web Store. Some tools you'll build are correctly
   private — they encode information about your specific work that
   you don't want to share. Make the call deliberately. Don't
   leave it set by default.

That's it. Four small disciplines. Each is cheap. None of them
turns the tool into "real software" in a way that changes its
spirit. The tool stays in the same family; it's just been worn in
a little.

## The temptation to professionalize

The thing not to do, when a disposable tool survives, is to
*professionalize* it. By "professionalize" I mean: install a
contributor guide, add issue templates, set up Dependabot, write
the polite README that explains how to run the project locally
and what the development conventions are, set up a documentation
site.

None of that is bad in the abstract. All of it is overkill for a
disposable tool that happens to have survived. The professional
overhead exists for projects that have *teams* — people who
didn't write the original code and need to learn it, plural
contributors collaborating, the social infrastructure of an open
source project. A surviving disposable tool has a team of one.
You don't need an issue template to file an issue with yourself.

Professionalization is an attractive nuisance. It looks like
seriousness. It feels like maturing. It's mostly busywork. Most
of the maintenance hours I see go into surviving disposable tools
go into the professional layer, not the actual code. Don't do
this. Resist.

## What if the tool needs to grow?

Sometimes a surviving tool legitimately wants to grow. A new
feature is needed; the underlying problem evolved. That's fine.
Add the feature. The same prompt-then-pace rhythm works on a
surviving tool exactly as well as on a fresh one. Open the editor.
Write a one-sentence description of the change. Prompt the model.
Pace the work. Ship.

The trap to avoid is *opportunistic* additions — features that
sound good while you're already in the file. Those are scope
creep wearing a new hat. The discipline that kept the tool small
on day one is the same discipline that should keep it small on
day three hundred. You're not building a product; you're
maintaining a small thing you happen to like.

## Sometimes survival is a signal

Occasionally — not often, but occasionally — a disposable tool
will survive in a way that suggests something larger underneath.
You'll notice that the tool keeps getting reached for in
unexpected contexts. You'll notice that the tool's pattern, not
just the tool itself, is what's surviving. That's a different
beast. That might be the seed of an actual product, or a library,
or a thing worth open-sourcing for real.

If that happens, *give it a different name and start a new
project.* Don't try to retrofit the disposable tool into the new
ambitious shape. The surviving disposable tool is doing its job
right where it is. The new project, with the new ambitions, is
its own thing. Let them coexist. The disposable origin is the
proof of concept; the new project is the product.

clock-mcp is, as of this writing, on v0.1.1 and probably will not
get a v0.2.0. It does what it does. It hasn't suggested anything
larger. It's a surviving disposable tool that knows what it is.

shell-mcp might be different. The pattern of "scoped, allowlisted,
walks-up-like-git" is interesting beyond the specific server. If
that pattern wants to become a more general thing — a library
other MCP servers use, a shape for tool-permissioning across the
ecosystem — that's a separate project. shell-mcp the binary
stays where it is. The library, if it ever exists, would be its
own repo with its own life.

The line is: *surviving doesn't mean ambitious.* Most surviving
disposable tools are content to stay small. The few that hint at
something larger should hint at it in a new repo, not in the
original one.

## Holding it loosely, even after survival

The whole point of this book is the loose grip. A disposable tool
held tightly stops being a disposable tool — even when it's
useful, even when you'd be annoyed if it broke. The grip is the
thing.

If a tool you built has survived for a year and you'd be
catastrophically inconvenienced if it stopped working, *that's
information*. It probably means the tool is more important than
its disposable origins suggest. Plan accordingly: more tests, a
wider deployment, an actual product mindset. Or, alternatively,
delete the tool and see if the catastrophe is real. Sometimes the
"catastrophic dependency" was a story you told yourself, and the
absence of the tool turns out to be fine.

Both responses are valid. The wrong response is to keep using the
tool with a death grip on a dependency you've never tested. That's
the shape of the brittleness that disposable tools are supposed
to be free of.

The next chapter is a different kind of survivor — developerpod,
a tool that started as a clear disposable thing and grew into a
tiny machine for running other disposable tools. The case study
is interesting because the *frame* is what survived, not the
specific tool.
