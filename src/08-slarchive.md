# Case study: SlArchive

> **Repo:** [`devrelopers/slarchive`](https://github.com/devrelopers/slarchive)
> **Stack:** single `index.html` + JSZip from CDN. No build. No backend.
> **First commit → working v1:** ~2.5 hours
> **Total commits in repo:** 8

## The thing I wanted

I wanted to read a Slack workspace's history without writing a
converter.

If you have ever exported a Slack workspace, you know what the
deliverable looks like: a `.zip` file of folders of `.json` files,
one folder per channel, files named by date, messages stored as
JSON objects with user IDs that reference a separate `users.json`,
plus `channels.json`, plus `mpims.json` for group DMs, plus a
`files/` directory for attachments. It is not a format intended
for human consumption. It's an export format intended for ingestion
into another system, and you are expected to be that system.

I needed to look at a year of conversation history, and I had a
ZIP file, and the tools that exist to render Slack archives are
either expensive SaaS or stale open source projects with abandoned
package.lock files. Neither was what I wanted.

What I wanted was: drop the ZIP into a page, see the workspace.

## The build

Eight commits. Total span: about 24 hours, with the bulk of the
implementation in a single 2.5-hour window on February 26, 2026.

```
ad3105b  16:22:19Z (Feb 26)  Initial commit
2ee5d6f  19:00:36Z (Feb 26)  Add full Slack ZIP processing and
                              two-panel workspace UI
9a906b3  21:28:22Z (Feb 26)  Add export, reset, progress
                              indicator, README, and favicon
70e8368  21:31:20Z (Feb 26)  Add screenshot for README
a7c6c2b  15:50:30Z (Feb 27)  Add copyright line linking to
                              DavidCanHelp.me
b3510a7  15:58:14Z (Feb 27)  Move copyright to a proper app footer
0f14bac  16:00:57Z (Feb 27)  Add GitHub repo link to footer
8069367  16:05:21Z (Feb 27)  Add 'it's free' message to landing
                              intro sequence
```

Commit 1 is the empty repo. Commit 2 is the working tool. The
remaining six are polish.

## The architecture

There is no architecture diagram, because there is no architecture.
SlArchive is a single `index.html` file. Inline CSS. Inline JS.
JSZip is loaded from a CDN. The tool runs end-to-end in the
browser. The ZIP file the user drops never leaves the user's
machine, because there is nowhere for it to go — there is no
server.

The shape of the runtime, in plain English:

1. User drags a `.zip` onto the drop zone.
2. JSZip parses it in memory.
3. We walk the entries, classify them (channel JSON, user JSON,
   files), build an in-memory model of the workspace.
4. We render a two-panel UI: channel list on the left, message
   stream on the right.
5. Search runs as a substring scan over the in-memory model.

That is the whole tool. There is no IndexedDB. No service worker.
No persistence. If you reload the page, you re-drop the ZIP. The
tool's "memory" lives only as long as the tab does. That's not a
limitation. That's the design. The whole point is that nothing
about the user's data sticks around anywhere, including in the
tool itself.

## The package.json that doesn't exist

This is my favorite detail and it deserves a section. The repo
contains five files at the root: `LICENSE`, `README.md`,
`favicon.svg`, `index.html`, `screenshot.png`. That's it. There
is no `package.json`. No `node_modules`. No build step. No
TypeScript configuration. No bundler. No `dist/` directory.

You don't need any of those things to ship a working tool. The
absence of all of them is a feature. To run SlArchive locally you
type:

    open index.html

That's it. Or `npx live-server` if you want auto-reload during
development. The dev environment, the build environment, and the
production environment are the same environment, which is your
browser.

This isn't a stunt. It's a deliberate choice grounded in scope.
The tool does one thing: render a Slack export. It needs a DOM
and JSZip. The browser has the DOM. JSZip is one `<script>` tag
away. There is no third dependency and there will never be a
third dependency, because the tool is done.

The disposable-tool muscle this exercises is the *no-package-json*
muscle. When you scope a tool tight enough, the build system
disappears with it. Not every tool can be no-build — most of them
can't — but enough of them can that you should ask the question
every time. *"Can this thing run without a build step?"* If yes,
that's a real saving in lifetime maintenance cost. The tool
cannot break because of a transitive dependency that got
unpublished, because there are no transitive dependencies. The
tool cannot break because Node 22 deprecates a flag from Node 20,
because the tool doesn't run on Node. The tool's deployment story
is "open the file."

## What it does in practice

I dropped the year-long export into the tool the day I built it.
The two-panel UI rendered within a couple seconds. I read the
threads I'd come back to read. I exported the relevant channels
to Markdown. I closed the tab. The work was done.

I have used SlArchive maybe four times since. Each use has
followed the same shape: someone hands me an export, I want to
search it, I open the page, I drop the ZIP, I do the work, I
close the tab. The tool is not in my dock. It is not bookmarked
in a special place. I open it from the GitHub page when I need
it. The artifact-to-use ratio is exactly right: high friction to
acquire, zero friction to use, no friction to forget.

## What this case study is for

SlArchive is the cleanest example in this book of a disposable
tool done right.

- **One sentence spec.** *"Read a Slack ZIP in the browser, no
  server."* That sentence held all the way to v1.
- **Subtraction defaults.** No backend. No database. No build. No
  framework. No package manager. Each *no* paid for itself.
- **Local over networked.** The data never leaves the machine
  because there's nowhere to send it.
- **Existing format over new format.** It reads Slack's export
  exactly as Slack hands it to you.
- **Stateless.** Reload the tab, you're back to the drop zone.
- **Polish came after function.** Commits 3 through 8 are footer
  links and a screenshot. The actual *tool* shipped in commit 2.

The contrast with Aftermark is instructive. Both projects took an
afternoon. Both shipped working software. Aftermark grew into
something product-shaped and I had to talk myself off the ledge
in the chapter before this one. SlArchive stayed exactly where it
started. The difference is not effort or skill. The difference is
that SlArchive's one-sentence spec excluded almost everything,
and I let that exclusion *hold*.

I think the secondary reason SlArchive stayed disciplined is that
it had no obvious "next feature." Aftermark, as a bookmark
manager, has a thousand obvious next features — that's the curse
of working in a category where every product has a feature list
you've internalized. SlArchive is in a category of one. The Slack
export viewer that runs locally and does nothing else has no
peers, and so no peer-pressure feature list. The tool gets to be
exactly what it is.

You will sometimes find yourself in this lucky position — building
a tool with no obvious comparable. When you do, savor it.
The discipline is easier when there's nothing to drift toward.

The next chapter is the principle this case study earns: *shipping
is a habit, not a phase.* SlArchive shipped in commit 2. That's
the canonical shape.
