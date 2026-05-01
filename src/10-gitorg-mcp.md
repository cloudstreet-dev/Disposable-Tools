# Case study: gitorg-mcp

> **Repo:** [`DavidLiedle/gitorg-mcp`](https://github.com/DavidLiedle/gitorg-mcp)
> **Language:** Rust
> **Initial commit → working v0.1.0:** 16 minutes 57 seconds
> **Total commits in repo:** 2

## The thing I wanted

I have several GitHub organizations. Most of the projects I've
built across the last decade are scattered across them. When I
want to know things like *which of my repos haven't been touched
in two months* or *what's open across all my orgs right now*, the
GitHub UI is the wrong shape. It's per-org. I have to go org by
org, repo by repo, page by page. The data exists. It's just
spread across a hundred clicks.

I'd built a CLI for this earlier — `gitorg`, a Rust tool that
queries the GitHub API, applies filters, and prints answers. It
worked. It still works. The CLI was the disposable tool that
solved the original problem.

What I wanted now was the same data, but addressable from inside
a Claude session. When I'm planning my week, I want to ask Claude
"what should I look at this week?" and have it actually be able
to answer — based on real data, not vibes. So: gitorg's
functionality, exposed as MCP tools, callable from Claude.

That's the spec. One sentence: *"Expose my GitHub org data to
Claude as MCP tools."*

## The build

This is the smallest case study in the book by elapsed time. Two
commits. Seventeen minutes from end to end.

```
682a2cf  20:26:55Z  Initial commit
bebe96e  20:42:52Z  Add MCP server exposing GitHub org data to AI
                    assistants
```

That's the whole repo's history. Sixteen minutes and fifty-seven
seconds between the two timestamps. The second commit is the
working server.

Six tools are exposed:

- `list_orgs` — list the user's GitHub organizations.
- `list_repos` — list repos across orgs, with optional filter and
  sort by stars / activity / name / staleness.
- `find_stale` — find repos with no recent pushes (60 days
  default).
- `list_issues` — list open issues across orgs (excludes PRs).
- `get_stats` — aggregate statistics: total repos, stars, forks,
  issues, top languages.
- `get_overview` — full dashboard: stats + recently active +
  stale + recent issues.

Every tool is annotated `readOnlyHint: true`. The server cannot
write anything to GitHub. It cannot create issues, close PRs,
push commits, change permissions, do anything destructive. The
read-only contract is part of the design — it's what makes me
comfortable having Claude call these tools without supervision.

## The pre-existing CLI did most of the work

Why was the build seventeen minutes? Because the underlying
`gitorg` library — the actual GitHub API querying, the filtering
logic, the staleness calculation — already existed. gitorg-mcp is
a thin MCP shell over an existing crate.

That's the pattern, and it's worth naming: **the second
disposable tool in a problem domain is much faster to build than
the first.** You've already paid the cost of figuring out the
domain — what GitHub's API returns, how to authenticate, how to
filter — when you built the first tool. The second tool, in a
different shape (MCP server instead of CLI), is mostly transport
and adaptation. It's the same logic with a new surface.

This compounds. After the first half-dozen disposable tools you
build, you'll have a small library of solved sub-problems sitting
in your repos and your `cargo` cache. The next disposable tool
will reach into that library, pull out three things, glue them
together, and ship in an afternoon. Not because you're getting
faster, but because the unsolved part of each new tool keeps
shrinking.

## What the tool does in practice

I added it to my Claude Desktop config the moment it built. The
first session after that, I asked Claude:

> "What should I look at this week across my orgs?"

Claude called `get_overview`. The response gave it a snapshot of
what was active, what was stale, what was open. It used that
snapshot to write a short, prioritized "things to check this
week" list, in plain English, anchored in real repo names with
real last-push dates. The list was useful. Some items I knew
about; some I'd forgotten about.

I had not realized, until that moment, how much of my mental
energy was going into *remembering what I had repos for*. The
answer had been "all of it, all the time, with mediocre recall."
gitorg-mcp moved that bookkeeping into the model, where it
belonged.

## A note on `readOnlyHint`

The MCP tool annotation `readOnlyHint: true` is one of those small
pieces of metadata that does outsized work. It tells the host
("Claude Desktop", in my case) that the tool is safe to call
without prompting the user for confirmation. That's the right
behavior for read tools — every prompt for "is it OK to run
`list_orgs`?" is friction the user has to absorb, and the tool
genuinely cannot ruin anything.

If gitorg-mcp ever grows write tools — *create_issue*,
*close_repo* — those would not be `readOnlyHint: true`. They'd
prompt every time. The boundary between "read" and "write" in
this server is enforced by which tools exist. The server has no
write tools. There is nothing to confirm because there is nothing
to break.

This is the same disposable-tools instinct from the chapter on
scoping: when you can be read-only, be read-only. gitorg-mcp
doesn't need to be a full GitHub API client. It needs to answer
my read questions. That's the whole spec. Write functionality
would have grown the surface area for no use I had.

## What the brevity says about the work

This case study is short because the build was short, and the
build was short because *there was nothing to design.* The data
shapes existed. The MCP transport was a known quantity from
shell-mcp and clock-mcp. The library underneath did the
hard work. The remaining task was glue.

You should expect this to happen more as you build more
disposable tools. Some afternoons you'll build the deep tool from
first principles. Some afternoons you'll glue two existing things
together in a new shape. Both are real disposable tools. The glue
ones are usually faster, and just as useful, and entirely worth
shipping.

The next chapter is the principle: *when a disposable tool isn't
disposable anymore.* gitorg-mcp is a candidate for that question.
I use it every week. It's been shipped at v0.1.0 for months. Has
it stopped being disposable? The chapter answers that question,
and it turns out the answer matters less than you'd think.
