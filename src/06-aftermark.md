# Case study: Aftermark

> **Repo:** [`DavidCanHelp/Aftermark`](https://github.com/DavidCanHelp/Aftermark)
> **Language:** TypeScript / Chrome MV3 extension
> **Initial commit → v0.4.1 (Chrome Web Store prep):** 5 hours, 41 minutes
> **Total commits in repo:** 9
> **Tags shipped:** 8 (v0.1.0 through v0.4.1)

## The thing I wanted

I wanted my bookmarks to mean something.

I have, at any given time, around two thousand bookmarks. Most are
mid-thought — I saved them because I meant to come back. Most of
them I never come back to, because the existing bookmark managers
treat a bookmark as a passive URL with a title, and the volume
makes the list useless on its own. I'd open the bookmark manager,
scroll for thirty seconds, give up, and go back to whatever I was
doing.

The signal here is the chapter before this one's exact diagnosis:
the existing tool was bad enough that I was avoiding the action.
I'd accumulated friction for years. I noticed it because, sometime
in early April 2026, I tried to find the page where I'd been
reading about MCP authorization flows three weeks earlier, gave up
inside ten seconds, and felt actively annoyed for the first time.
The wish-cutting reflex didn't fire fast enough. The tool got
built that afternoon.

## The frame

Bookmarks are not links. Bookmarks are *intentions* — compressed
unfinished thoughts that you saved because the thought wasn't done.
"Read this." "Compare this to the other thing." "Decide whether to
adopt this library." "Come back when I have time." Each bookmark
is a sentence that didn't get finished, and the existing tools
treat it as a row in a table.

If you treat a bookmark as an intention, the right operations on
the collection look different. You don't want to *list* bookmarks;
you want to *cluster* them by intent. You don't want to *search*
them by title; you want to recover *why* you saved them. You want
the dead links flagged. You want the duplicates merged. You want
the things you saved twelve months ago and never read flagged for
review or deletion. You want a sense of the *shape* of your saved
intentions, not just an alphabetical list.

That frame is the spec. Everything in Aftermark falls out of that
frame.

## The build

Aftermark started at 14:56:53Z on April 8, 2026 with an `Initial
commit`. The Chrome Web Store packaging tag — v0.4.1 — landed at
20:38:28Z the same day. Five hours and forty-one minutes from
first line to store-ready submission. Eight tagged versions in
between. Nine commits total.

The arc, version by version:

```
15112cc  14:56:53Z  Initial commit
da8ded5  15:52:54Z  v0.1.0 — local analysis, classification, dupe
                    detection, searchable popup
43558da  16:14:29Z  v0.2.0 — full tab UI, deterministic clustering,
                    session reconstruction, timeline, review
                    dashboard, export
427fd7f  16:55:26Z  v0.2.1 — CRUD, batch delete, bulk actions,
                    cluster pruning
7bc37f6  17:16:05Z  v0.3.0 — insights page, health scores,
                    expanded heuristics, fuzzy duplicates,
                    favicons, search, folder insights
36ee1e1  17:31:08Z  v0.3.1 — real-time monitoring, context
                    capture, live tab refresh, badge count
00eeb8b  19:00:34Z  v0.3.2 — smart cleanup wizard, dead link
                    scan results
fb0b13c  20:33:18Z  v0.4.0 — full tag system, smart suggestions,
                    bulk tagging
25667b6  20:38:28Z  v0.4.1 — Chrome Web Store packaging
```

Read that as a record of how the scope drifted, and got pulled
back, and drifted again. v0.1.0 was the actual disposable tool —
"local analysis, dupe detection, popup." A working version of the
thing I noticed I needed. I could have stopped there.

I didn't. The full tab UI in v0.2.0 was justified — once the
classifier worked, the popup was too small to surface what the
classifier knew, and a tab UI gave it room to breathe. Each
subsequent version was, individually, justified. But by v0.3.0 I
had a Chrome extension with seven views, and by v0.4.1 I was
writing a privacy policy and a Chrome Web Store listing.

That's *not* a disposable tool anymore. That's a real product. The
question is whether I made the right call letting it grow.

## What grew, and why

The honest answer is: about half of what I added paid for itself in
my own use, and about half didn't. The split is informative.

**Paid for itself:**

- **Classification.** Categorizing each bookmark into a content
  type (article, repo, doc, video, etc.) made the cluster view
  legible. Without it, the clusters were just URL-shaped soup.
- **Health scores.** A bookmark's "health" is a small heuristic —
  is the URL still resolving, has it been touched recently, is it
  duplicated. Surfacing the score made the *review* workflow
  possible. Without health scores I'd never have culled the dead
  links.
- **Saved filters.** Once the dataset got large, applying the same
  "stale articles I never read" filter twice in a row was annoying.
  Saved filters fixed that for me specifically.

**Didn't pay for itself, in retrospect:**

- **The full tag system in v0.4.0.** I built tag suggestions, bulk
  tagging, tag-based clusters. I have used the tag system maybe
  twice. Tags are a feature for *teams*, where one person's
  taxonomy needs to communicate to another person. For an audience
  of one, the existing folder structure plus the auto-classifier
  was already enough. The tag work was about three hours and
  produced almost no value in my use.
- **The session reconstruction view.** Cute. Used it once. Never
  again.

The mistake — and it's a small one, but it's the kind of mistake
this book is partly about — was that the tag system felt like the
"obvious next feature" once you had clusters and filters and
classification. It looked small. It looked natural. It was the
shape of what a real bookmark manager would have. I let it in
because I'd lost the discipline of asking *do I, specifically,
in my actual use, need this?* I just kept building.

## Where the disposable frame would have landed

If I'd stopped at v0.1.0, Aftermark would have been a textbook
disposable tool: one afternoon, one specific problem, working
solution, ship and use. The thing I needed was *find the page I
read three weeks ago about MCP auth.* v0.1.0 solved that.

If I'd stopped at v0.3.0, Aftermark would have been a slightly
ambitious afternoon — five views instead of one, deterministic
clustering, review dashboard. Still in the disposable spirit:
all the features, in retrospect, paid for themselves in my use.

Past v0.3.0, the work was real-software work. The privacy policy
in `PRIVACY.md`. The store listing in `STORE_LISTING.md`. The
icon set. The Chrome MV3 manifest hardening. None of that was
for me. All of it was for an imaginary set of users I didn't have.

The audience drifted from one to many. I stopped noticing. By
v0.4.1 I was building a product.

## The lesson

Disposable tools are a discipline. The discipline is not
*self-control about coding.* It's self-honesty about audience.
Every feature has an audience. When you can name a real person
who benefits from the feature — yourself, this week, doing this
specific work — the feature is in scope. When the only person
you can name is "a hypothetical user who might install this," you
are no longer building a disposable tool.

That's not a moral judgment. It's a categorical one. Aftermark
v0.4.1 is a real product, and a real product has its own
discipline, its own life cycle, its own concerns. Disposable tools
have a different one. The mistake is to mix them.

I'm not going to pretend the Chrome Web Store work was a waste.
The icons are nice. The privacy policy is thorough. The manifest
is clean. If I ever decide to publish, all of that is ready. But
I built it because the work felt good in the moment, not because I
needed it. And the difference between *feels good* and *needed* is
the place this book lives.

## What this case study does for the rest of the book

The next chapter is the principle this case study earns: *prompt,
then pace.* It's about the rhythm of working with the AI as a
collaborator — how you prompt to set scope, then pace the work to
keep the scope from running away. Aftermark drifted because I lost
the rhythm somewhere around v0.3.1. The pacing chapter is what I
should have been doing.
