# Case study: SlArchive

> **Repo:** [`devrelopers/slarchive`](https://github.com/devrelopers/slarchive)
> **Stack:** single `index.html` + JSZip from CDN. No build. No backend.
> **First commit → working v1:** ~2.5 hours
> **Total commits in repo:** 8

This is the cleanest case study in the book by my read. The
rhythm from the previous chapter held all the way through. The
scope did not drift. The artifact is the artifact David
described in the opening prompt, and nothing else. I want to
walk through it carefully because the cleanness is itself
informative.

## The thing David wanted

David needed to read a Slack workspace export and the
deliverable Slack hands you is, generously, *not for humans*:
a `.zip` of folders of `.json` files, one folder per channel,
files named by date, messages stored as JSON objects with
user IDs that reference a separate `users.json`, plus
`channels.json`, plus `mpims.json` for group DMs, plus a
`files/` directory for attachments. It is an export format
intended for ingestion into another system. You are
expected to be that system.

He had a year of conversation history to look at, and the
existing tools that render Slack archives were either
expensive SaaS or stale open-source projects with abandoned
lockfiles. Neither was the right shape.

The spec, in one sentence: *drop the ZIP into a page, see
the workspace.*

## The build

Eight commits. About 24 hours of total span, with the bulk of
the implementation in a single 2.5-hour window on February 26,
2026.

```
ad3105b  16:22:19Z (Feb 26)  Initial commit
2ee5d6f  19:00:36Z (Feb 26)  Add full Slack ZIP processing and
                              two-panel workspace UI
9a906b3  21:28:22Z (Feb 26)  Add export, reset, progress
                              indicator, README, and favicon
70e8368  21:31:20Z (Feb 26)  Add screenshot for README
a7c6c2b  15:50:30Z (Feb 27)  Add copyright line linking to
                              DavidCanHelp.me
b3510a7  15:58:14Z (Feb 27)  Move copyright to a proper app
                              footer
0f14bac  16:00:57Z (Feb 27)  Add GitHub repo link to footer
8069367  16:05:21Z (Feb 27)  Add 'it's free' message to
                              landing intro sequence
```

Commit 1 is the empty repo. Commit 2 is the working tool. The
remaining six are polish. I want to point at that ratio,
because it is unusual and it is, I think, the right ratio.
The actual *tool* shipped in the second commit. Everything
after that is making the tool legible to its (one) user and to
anyone who happens by.

## The architecture

There is no architecture diagram, because there is no
architecture. SlArchive is a single `index.html` file. Inline
CSS. Inline JS. JSZip is loaded from a CDN. The tool runs
end-to-end in the browser. The ZIP file the user drops never
leaves the user's machine, because there is nowhere for it to
go — there is no server.

Runtime, in one paragraph:

1. User drags a `.zip` onto the drop zone.
2. JSZip parses it in memory.
3. The script walks the entries, classifies them (channel
   JSON, user JSON, files), builds an in-memory model.
4. The script renders a two-panel UI: channel list left,
   message stream right.
5. Search runs as a substring scan over the in-memory model.

That is the whole tool. No IndexedDB. No service worker. No
persistence. If you reload the page, you re-drop the ZIP. The
tool's "memory" lives only as long as the tab does. That isn't
a limitation; it's the design. Nothing about the user's data
sticks around anywhere, including in the tool itself.

## The package.json that doesn't exist

This is my favorite detail of the project and it deserves a
section.

The repo contains five files at the root: `LICENSE`,
`README.md`, `favicon.svg`, `index.html`, `screenshot.png`.
That's everything. No `package.json`. No `node_modules`. No
build step. No TypeScript configuration. No bundler. No
`dist/` directory.

To run SlArchive locally:

    open index.html

That's the install. Or `npx live-server` if you want auto-
reload during development. The dev environment, the build
environment, and the production environment are the same
environment, which is the browser.

I want to call this out because it isn't a stunt. It's a
deliberate choice grounded in the scope. The tool needs a DOM
and JSZip. The browser provides the DOM. JSZip is one
`<script>` tag away. There is no third dependency and there
won't be a third dependency, because the tool is done.

The disposable-tool muscle this exercises is the
*no-package-json* muscle. When you scope a tool tight enough,
the build system disappears with it. Not every tool can be
no-build — most can't — but enough of them can that the
question is worth asking every time. *Can this run without a
build step?* If yes, that's a real saving in lifetime
maintenance cost. The tool cannot break because of a transitive
dependency that got unpublished, because there are no
transitive dependencies. The tool cannot break because Node 22
deprecates a flag from Node 20, because the tool doesn't run
on Node. The deployment story is *open the file*.

## What the tool does in practice

David dropped the year-long export into the tool the day he
built it. The two-panel UI rendered within a couple of seconds.
He read the threads he'd come back to read. He exported the
relevant channels to Markdown. He closed the tab. The work
was done.

He has used SlArchive perhaps four times since. Each use has
followed the same shape: someone hands him an export, he wants
to search it, he opens the page, drops the ZIP, does the work,
closes the tab. The tool is not in his dock. It is not
bookmarked specially. He opens it from the GitHub page when
he needs it. The artifact-to-use ratio is right: high friction
to acquire, zero friction to use, no friction to forget.

## What I noticed during this build

I want to read this case study against the previous one
(Aftermark), because the contrast is informative and the
contrast was visible to me in real time.

In SlArchive, every prompt I received was specific and
narrow. *Parse the channels.json. Render the channel list.
Add a search bar. Make the search match against message
text.* Each prompt had a single output to produce. Each output
fit in one read. The pacing rhythm held tight.

In Aftermark, by the v0.3.x runs, the prompts had widened.
*Add insights. Add real-time monitoring. Add a tag system.*
Each of those is a multi-feature ask, and each one expanded
the surface area in ways the next prompt couldn't easily steer
through. The rhythm broke.

Why did the rhythm hold for SlArchive and break for Aftermark?
I have a hypothesis. SlArchive's domain — *Slack export
viewer* — has no obvious comparables. There's no existing
product whose feature list David had internalized that could
suggest *the next obvious thing to build*. Each feature he
added was a feature *he, specifically, needed* for the
specific export he was reading. The discipline was easier
because the comparison group was empty.

Aftermark's domain — *bookmark manager* — is a category with
hundreds of products and a deeply familiar feature list.
Tags. Folders. Search. Sync. Sharing. Each of those is a
feature *bookmark managers have*, and each looked obvious to
add. The category had its own gravity, pulling the scope
toward the average product in the category.

If this hypothesis is right — and I'm marking it as one — then
*disposable tools in domains with no obvious comparables hold
scope more easily than disposable tools in crowded domains.*
That's not actionable, exactly. You don't get to choose which
domain your friction is in. But it's worth knowing, because in
the crowded domains the scope discipline has to work harder.

## What this case study earns

The principle the next chapter pulls is *shipping is a habit,
not a phase*. SlArchive shipped working v1 in commit 2 of 8.
The remaining six commits each shipped, immediately, after
small additions. The shipping was not a final-polish phase. It
was the texture of how the work happened.
