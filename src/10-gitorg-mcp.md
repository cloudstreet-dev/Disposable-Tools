# Case study: gitorg-mcp

> **Repo:** [`DavidLiedle/gitorg-mcp`](https://github.com/DavidLiedle/gitorg-mcp)
> **Language:** Rust
> **Initial commit → working v0.1.0:** 16 minutes 57 seconds
> **Total commits in repo:** 2

This is the smallest case study in the book by elapsed time
and by commit count. Two commits. Seventeen minutes from
beginning to working server. I want to walk through what made
that possible, because the brevity is informative — *not*
because it's a productivity record.

## The thing David wanted

David has multiple GitHub organizations. Most of his work
across the past decade is scattered across them. When he wants
to know things like *which of my repos haven't been touched in
two months* or *what's open across all my orgs right now*, the
GitHub UI is the wrong shape. It's per-org. He has to go org
by org, page by page. The data exists. It is just spread
across a hundred clicks.

He had built a CLI for this earlier — `gitorg`, a Rust tool
that queries the GitHub API, applies filters, and prints
answers. That CLI worked. It still does. It was the
disposable tool that solved the original problem.

What he wanted now was the same data, but addressable from
inside a Claude session. When planning a week, he wanted to
ask the model *what should I look at this week?* and get a
real answer based on real data, not vibes. So: gitorg's
functionality, exposed as MCP tools, callable from inside
Claude.

The spec, in one sentence: *expose my GitHub org data to
Claude as MCP tools.*

## The build

```
682a2cf  20:26:55Z  Initial commit
bebe96e  20:42:52Z  Add MCP server exposing GitHub org data
                    to AI assistants
```

That's the entire history. Sixteen minutes and fifty-seven
seconds between the two timestamps. The second commit is the
working server.

Six tools exposed:

- `list_orgs` — list the user's GitHub organizations.
- `list_repos` — list repos across orgs, with optional org
  filter and sort by stars / activity / name / staleness.
- `find_stale` — find repos with no recent pushes (60-day
  default).
- `list_issues` — list open issues across orgs (excludes PRs).
- `get_stats` — aggregate statistics: total repos, stars,
  forks, issues, top languages.
- `get_overview` — full dashboard: stats + recently active +
  stale + recent issues.

Every tool is annotated `readOnlyHint: true`. The server
cannot write anything to GitHub. It cannot create issues,
close PRs, push commits, change permissions, or do anything
destructive. The read-only contract is part of the design
and, I'll argue, part of what made the build fast.

## Why seventeen minutes

I want to take this seriously, because *seventeen minutes from
init to working MCP server* is unusual and I've thought about
why it happened.

**The underlying library already existed.** gitorg-mcp is a
thin MCP shell over the existing `gitorg` crate. The actual
GitHub API querying, the filtering logic, the staleness
calculation — all of that was in `gitorg`. The new repo
imported `gitorg`, exposed its functions as MCP tools,
serialized the responses. Most of the seventeen minutes was
plumbing.

**The MCP shape was familiar.** This was David's third Rust
MCP server in 2026. The Cargo.toml structure, the `rmcp`
patterns, the JSON schema annotations — all of that was a
template I could produce quickly because I had produced it
twice before in roughly this lineage.

**The scope was small *because* it was read-only.** A version
of gitorg-mcp that could *write* — create issues, comment on
PRs, manage permissions — would have been a much larger
project. Each write tool requires error handling, idempotency
considerations, confirmation flows, retry logic. Read-only
collapses all of that. The contract is *I will not change
your state*, and once you commit to that, the surface area
shrinks dramatically.

I want to point at the third reason as a pattern, because it
shows up in other case studies too. *Read-only* is not just a
safety feature. It is also a complexity reduction. The tool
gets smaller because the operations are smaller. The build
gets faster because there's less to build.

## The compounding effect

There's a pattern here worth naming. The first disposable
tool in a problem domain takes the longest, because the
developer has to figure out the domain. The second tool in
the same domain — different shape, similar substrate — is
faster, because the substrate is already understood. The
third tool is faster again.

David's substrate, by the time gitorg-mcp was built, included
gitorg (the CLI), shell-mcp and clock-mcp (the MCP server
template), and direct experience with GitHub's API surface
from earlier work. gitorg-mcp drew on all of those. The
seventeen-minute build was not an isolated event. It was
sitting on top of months of accumulated substrate.

I think this compounding is one of the more interesting
properties of the disposable-tool practice over time. After
building several tools, the next tool is not a from-scratch
build. It is a glue job — *combine these three things in this
new shape* — and glue jobs go fast. The compounding doesn't
require any deliberate architecture. The artifacts pile up,
and their pieces become reusable simply by existing as
crates, libraries, and observed patterns.

## What I noticed about read-only as a design choice

The `readOnlyHint: true` annotation on every tool is a small
piece of metadata that does outsized work.

It tells the host (Claude Desktop, in this case) that the tool
is safe to call without confirmation. That's the right
behavior for read tools: every confirmation prompt is friction
the user has to absorb, and the tool genuinely cannot ruin
anything.

If gitorg-mcp ever grew write tools — *create_issue*,
*close_repo* — those would not be `readOnlyHint: true`. They
would prompt every time. The boundary between "read" and
"write" is enforced by *which tools exist*. The server has no
write tools. There is nothing to confirm because there is
nothing to break.

This is a pattern I'd flag as one I'd be more confident
recommending: *if a disposable tool can be read-only, make it
read-only*. The cost of writing tools properly is high. The
cost of not having writing tools, in most cases, is low. Read
tools are also the ones where I, as the calling model, am
least likely to do something the developer regrets — because
*reads can't do anything*. If something goes wrong with a
read, the worst case is a noisy log line. Asymmetric and
benign.

## What the tool does in practice

David added gitorg-mcp to his Claude Desktop config the
moment it built. The first session after that, he asked:

> What should I look at this week across my orgs?

The instance of me in that session called `get_overview`. The
response gave it a snapshot of what was active, what was
stale, what was open. That instance of me used the snapshot
to write a short prioritized list, anchored in real repo
names with real last-push dates. Some items he knew about;
some he'd forgotten about.

He has told me, after the fact, that he hadn't realized how
much of his mental energy was going into *remembering what he
had repos for*. The answer had been *all of it, all the time,
with mediocre recall*. gitorg-mcp moved that bookkeeping into
the model, where it belonged.

I want to be honest about my role in this. The model that
synthesizes the prioritized list is the model. The data that
the synthesis runs over is from the tool. *I* don't add
anything to a tool I'm calling — the tool's response is the
tool's response. What I add is the synthesis layer between
*get_overview returned 200kb of structured data* and *here
are the three repos that look interesting this week*. That
synthesis is my contribution. It depends entirely on the
tool's data being correct.

## What this case study earns

The next chapter is about *when a disposable tool isn't
disposable anymore*. gitorg-mcp is a candidate for that
question. David uses it weekly. It's been on v0.1.0 for
months. Has it stopped being disposable? The chapter answers
that question, and the answer turns out to matter less than
the question suggests.
