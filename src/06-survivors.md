# When a disposable tool refuses to stay disposable

A small percentage of the disposable tools you build will
fail to die. You'll keep using one. Months will pass. You'll
notice that you'd be annoyed if it broke. You'll catch
yourself installing it on a new machine without thinking.
You'll send the URL to a colleague.

That is an interesting moment. It deserves a chapter, partly
because it requires a different posture than the rest of the
book describes.

## The signs of survival

Four soft thresholds, none of which is decisive on its own:

- *You've used it for more than thirty days.* The thirty
  days is approximate. The point is that the tool has
  outlasted the use that motivated it, and is still in use
  for adjacent work.

- *You'd be annoyed if it broke.* Test directly. If
  `cargo install` failed tomorrow because of a Rust version
  bump, would you fix it? Or would you shrug and use the
  workaround you had before? *Fix it* graduates the tool.
  *Shrug* keeps it disposable.

- *Another tool of yours depends on it.* This is the
  strongest signal. The moment Tool B imports Tool A, Tool A
  is no longer disposable. It has a consumer. Disposable
  tools have one *user*. Consumers are different.

- *Someone else is using it.* Sharing alone doesn't graduate
  the tool — handing a colleague a one-off URL is fine.
  *Their depending on it* graduates it.

When two or more are true, the tool has survived into
something else.

## The category it survives into

I do not think *survived* is, on its own, an interesting
category. The interesting categories are *disposable*, *small
infrastructure for one*, and *real product*. They have
different disciplines.

**Disposable.** Audience of one. One sentence. Held loosely.
Built in an afternoon, used for a thing, set down. Most of
what this book is about.

**Small infrastructure for one.** Used regularly by a single
developer for ongoing work, sits in their environment, has
near-zero maintenance cost. It is no longer disposable in the
sense that the developer would casually replace it. It is also
not a product. It is infrastructure that fits in your hand.
Most of your shell aliases and dotfiles are in this category.
Some surviving disposable tools end up here.

**Real product.** Audience plural. Roadmap implied.
Documentation for users who are not the author. Privacy
policy. Store listing. This category has its own disciplines
that are not in this book.

I find this three-category split clearer than a binary
*disposable/not* one. The middle category is where most
surviving disposable tools end up, and it has disciplines
that look like the disposable disciplines plus small
additions, not like product disciplines at all.

## What changes for small-infrastructure-for-one

Not much. Some cheap upgrades earn their keep:

1. **Pin the versions of internal dependencies.** If two of
   your tools share a substrate crate, pin the crate version
   in both. Regular hygiene; matters more once the tools
   have consumers.

2. **Bother with a CHANGELOG.** When a tool is going to be
   touched repeatedly over months, an explicit CHANGELOG.md
   is almost free and saves your future self a lot of
   `git log -p`.

3. **Tests where the contract bites.** Not comprehensive
   tests. Tests targeted at the parts that would be silently
   wrong if they broke. The diagnostic for *which parts* is:
   *if this function failed quietly, would I notice?* If no,
   write the test. If yes, don't.

4. **Decide public vs. private deliberately.** Some tools
   are correctly public. Some are correctly private. Make
   the call rather than letting it happen by default.

That's the entire list. Four items. None turn the tool into
real software in a way that changes its spirit.

## The professionalization trap

The thing not to do, when a disposable tool survives, is to
*professionalize* it. Issue templates. Contributor guides.
Dependabot. The polite README that explains the development
conventions. A documentation site.

None of that is bad in the abstract. All of it is overkill for
a tool with a team of one. The professional overhead exists
for projects with *teams* — people who didn't write the
original code, plural contributors collaborating, the social
infrastructure of an open-source project. A surviving
disposable tool has a team of one. You don't need an issue
template to file an issue with yourself.

The trap is most active when the developer has been steeped
in real-product practices and reflexively applies them. The
practices are right *for the contexts they were developed
for*. They are not right for a small binary that does one
thing for one person.

## When a survivor wants to grow

Sometimes a surviving tool legitimately wants to grow. A new
feature is needed; the underlying problem evolved. The same
loop from the shipping chapter works on a survivor as well as
on a fresh tool: spec, draft, run, sharpen, loop, stop.

The trap to avoid is *opportunistic* additions — features
that sound good while you happen to be in the file. Those are
scope creep wearing a new hat. The discipline that kept the
tool small on day one is the same discipline that should keep
it small on day three hundred.

## When the survivor is hinting at something larger

Occasionally, rarely, a surviving tool hints at something
larger. The pattern, not just the tool, is what's surviving.
The same shape keeps wanting to apply elsewhere.

If that happens, give the new thing a different name and
start a new project. Do not retrofit the disposable tool into
the new ambitious shape. The surviving tool is doing its job
where it is. The new project, with the new ambitions, is its
own thing. Let them coexist; let the disposable origin be a
proof of concept, not a foundation.

The line is: *surviving doesn't mean ambitious*. Most
survivors are content to stay small. The few that hint at
something larger should hint in a new repository.

## The brittle-dependency check

A surviving disposable tool that you depend on but have not
re-tested in months is a hidden risk. The right move is to
test it: run it for the actual use, see if it still works,
fix what's broken. If you find you can't fix it without an
afternoon's work, ask whether the afternoon is worth it.
Sometimes it is. Sometimes the surviving tool was carrying
more weight than its code justified, and the right move is
to retire it deliberately rather than carry it as a
dependency you've never re-tested.

Both responses are valid. The wrong response is to keep using
the tool with a death grip on a dependency you haven't
checked. That's the brittleness disposable tools are
supposed to be free of, sneaking back in.

The next chapter is the catalog of failure modes that show
up when the disciplines from this book get violated.
