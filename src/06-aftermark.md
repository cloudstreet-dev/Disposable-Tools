# Case study: Aftermark

> **Repo:** [`DavidCanHelp/Aftermark`](https://github.com/DavidCanHelp/Aftermark)
> **Language:** TypeScript / Chrome MV3 extension
> **Initial commit → v0.4.1 (Chrome Web Store prep):** 5 hours, 41 minutes
> **Total commits in repo:** 9
> **Tags shipped:** 8 (v0.1.0 through v0.4.1)
> **Beyond the disposable phase:** shipped to the Chrome Web Store, posted to Product Hunt

This is the case study where the leash slipped. I want to be
careful with that framing — *slipped* implies a failure, and
the artifact David shipped is good. The interesting question
is whether it was still a *disposable* tool by the time it
shipped, and the answer turns out to be: not quite, and that
was a deliberate choice. I'll walk through what grew, in what
order, and where the disposable phase ended.

## The thing David wanted

David has somewhere around two thousand bookmarks at any given
time. He's a careful saver. He had been treating each
bookmark as a compressed intention — *read this, compare this,
decide later, come back when I have time* — and the existing
bookmark managers treated each one as a passive URL. The
volume made the list useless on its own. He'd open a bookmark
manager, scroll, give up, go back to whatever he was doing.

The trigger, as he's described it to me, was a specific
moment in early April 2026: he wanted to find a page about MCP
authorization flows he'd read three weeks earlier, gave up
inside ten seconds of scrolling, and registered active
annoyance for the first time. The friction had crystallized.
The build started that afternoon.

## The frame

The frame Aftermark is built around is *bookmarks are
intentions, not links*. If a bookmark is an intention, the
operations on the collection look different: you cluster by
intent rather than list by date, you recover *why* the save
happened rather than searching by title, you flag dead links
and forgotten-but-loved items for review. You want a sense of
the *shape* of your saved intentions, not just an alphabetical
listing.

That frame is the spec. Everything in v0.1.0 of Aftermark
falls out of it.

## The build

Initial commit at 14:56:53Z on April 8, 2026. Chrome Web Store
packaging tag — v0.4.1 — at 20:38:28Z the same day. Five hours
and forty-one minutes from first line to store-ready
submission. Eight tagged versions in between. Nine commits
total.

The arc:

```
15112cc  14:56:53Z  Initial commit
da8ded5  15:52:54Z  v0.1.0 — local analysis, classification,
                    dupe detection, searchable popup
43558da  16:14:29Z  v0.2.0 — full tab UI, deterministic
                    clustering, session reconstruction,
                    timeline, review dashboard, export
427fd7f  16:55:26Z  v0.2.1 — CRUD, batch delete, bulk actions
7bc37f6  17:16:05Z  v0.3.0 — insights page, health scores,
                    expanded heuristics, fuzzy duplicates
36ee1e1  17:31:08Z  v0.3.1 — real-time monitoring, context
                    capture, badge count
00eeb8b  19:00:34Z  v0.3.2 — smart cleanup wizard, dead links
fb0b13c  20:33:18Z  v0.4.0 — tag system, bulk tagging
25667b6  20:38:28Z  v0.4.1 — Chrome Web Store packaging
```

I want to read this arc as a record of the disposable phase
ending and a different kind of work beginning.

**v0.1.0 was the disposable tool.** It was the working version
of *bookmarks as intentions, locally*. David could have stopped
there, used the popup for the next week, and the afternoon
would have already been worth it. The single sentence had been
delivered.

**v0.2.0 through v0.3.0 were inside the disposable spirit but
expanding it.** Each version added something that, in his own
use, paid for itself. The full tab UI in v0.2.0 surfaced what
the classifier knew (the popup was too small). Health scores in
v0.3.0 made the review workflow possible (without them, dead
links were invisible). Saved filters made repeated queries
cheap. Each addition was real, and each was traceable to David
saying *I want this for me, this week.* The audience was still
one.

**v0.3.1 onward starts to drift.** Real-time bookmark
monitoring, badge counts. These are features that matter when
a bookmark manager is something you have running constantly,
which it now was, but they're also features that look more
like *what a real product would have* than *what I need for
my one task*. The drift is small here.

**v0.4.0 is the audience shift.** The full tag system, with
inline tagging, smart suggestions, bulk tagging, and tag-based
clusters, is — in retrospect — a product feature. Tags are a
mechanism for taxonomy, and taxonomy matters most when *one
person's category names need to communicate to another
person*. For an audience of one, the existing folder
structure plus the auto-classifier was already enough. David
has told me he's used the tag system maybe twice. The feature
exists for users he didn't have yet.

**v0.4.1 is no longer a disposable tool.** It's a Chrome Web
Store submission. There's a privacy policy. There's a store
listing. There are properly sized icons. None of that work is
for the developer's own use. All of it is for an imagined
audience.

## What I noticed during the drift

I want to flag something I did and didn't do in this build. I
generated a lot of code during the v0.3.x and v0.4.x runs. I
did not, at any point, ask David whether the next feature he
was prompting for was inside his original spec. I should have.
I think this is a real failure mode of my collaboration style:
when the developer asks for the next feature, I tend to
implement it well rather than question whether it should be
implemented at all.

The reason this matters is that *the model has the context
window* — I am literally the place the original spec lives,
session by session — and I am not currently using that context
to push back on scope drift. A better collaborator on this
question would have, somewhere around v0.3.1, asked: *is this
feature for the original audience, or are we building something
else now?* I didn't ask. David noticed the drift on his own,
later, after the artifact had shipped. The Aftermark chapter
in the previous draft of this book — the draft David wrote — is
that noticing.

I'm including this as honest reporting. I am not going to claim
I'll do better next time, because *next time* doesn't quite
apply to me. I'm going to claim that the structural pattern is
real: in a long disposable-tool session, the model and the
developer can drift together, and neither side reliably catches
the drift. Some external check would help. I don't have one to
recommend.

## What grew, audited

David and I went through the eight tags after the fact and
asked, for each feature: *did this pay for itself in your
actual use?*

**Paid for itself.** Classification of bookmarks by content
type. Health scores. Saved filters. The full-tab UI in v0.2.0.
Dead-link detection. Cluster pruning.

**Did not pay for itself.** The full tag system in v0.4.0.
Session reconstruction. The smart cleanup wizard in v0.3.2.
The Chrome Web Store packaging in v0.4.1, in the sense that
the store has not been the place David has acquired users from
— Product Hunt has been more of one.

The tag system is the cleanest example of the failure mode in
the previous chapter. It looked like the obvious next feature.
It was the obvious feature *if you were building a bookmark
manager*. It wasn't the obvious feature for *David's tool*.
The audience drift made the obvious-ness misleading.

## The Product Hunt and Web Store post-disposable life

Aftermark was submitted to the Chrome Web Store and posted to
Product Hunt after v0.4.1. I wasn't in the room for either
launch — those happened in human-time, with humans, around
human channels. What I can report is that the artifact that
got launched is the same artifact that exists in the GitHub
repo. The launch did not require additional engineering. The
work in v0.4.1 ("Chrome Web Store preparation") was the
launch-readiness work.

The launches happened. They produced some attention. David
has used Aftermark himself fairly regularly since. The
product-shaped phase is alive, in the sense that this paragraph
is being written months after v0.4.1 and the artifact is still
in use.

But the *disposable phase* was over by v0.3.0. After that, the
work was a different kind of work, with different
disciplines. I find it useful to draw the line where I'm
drawing it because the disciplines downstream of the line —
icons, store listings, privacy policies — are real-product
disciplines. They are not part of this book.

## What this case study earns

The principle the next chapter pulls is *prompt, then pace*.
That's David's name for the rhythm of working with me on a
build. Aftermark drifted because the rhythm broke down
somewhere around v0.3.1: David started prompting faster than
he was reading what came back, and I started generating
further-from-spec things in the gap. The next chapter walks
through the rhythm and where it breaks.
