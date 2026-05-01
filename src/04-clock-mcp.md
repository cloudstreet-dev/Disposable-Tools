# Case study: clock-mcp

> **Repo:** [`devrelopers/clock-mcp`](https://github.com/devrelopers/clock-mcp)
> **Language:** Rust
> **First commit → v0.1.0 release:** 25 minutes
> **First commit → v0.1.1 release:** 91 minutes
> **Total commits in repo:** 8

## The thing I wanted

I wanted to ask Claude what time it was and have it actually know.

That's the whole pitch. There is no second sentence. The model
sitting at the other end of my prompts had no wall clock — not a
real one, not a reliable one, not one tied to my actual machine's
view of UTC. So when I asked about durations or timezones or
"how long until Friday," the answers came out shaped like guesses,
because they were guesses. Some of them were close. None of them
were anchored.

The chapter before this one was about the wish-cutting reflex.
This is the chapter about what happens when you don't cut the
wish.

## The build

`init` at 17:50:31Z on April 20, 2026. v0.1.0 released at 18:15:46Z,
twenty-five minutes later. v0.1.1 released about half an hour after
that. Total wall time on the project, from `git init` through
"Cleanup: commit Cargo.lock and ignore .claude/ session state":
about ninety-one minutes.

The crate is small enough to describe in one paragraph. It's a
single Rust binary that speaks MCP over stdio, built on `rmcp` 1.5,
using `chrono` and `chrono-tz` for the actual time math. Five tools:

- `now` — current time in a given IANA timezone, defaulting to UTC.
- `time_until` — signed duration from now to a target datetime.
- `time_since` — signed duration from a past datetime to now.
- `time_between` — signed duration between two datetimes.
- `convert_timezone` — re-express an instant in another IANA
  timezone.

The signed-duration choice matters. An unsigned duration is a
booby trap when the model is computing "time until" and the target
is in the past — you don't want it silently flipping sign and
returning a happy positive number that's lying to you. Signed
means the model can reason about temporal relations honestly:
negative values mean "you missed it," and that's information.

Errors are structured `{ error, hint }` JSON. The hint field is
specifically for the model — a short string telling it what to do
next, e.g. *"the timezone string must be a valid IANA name like
America/Los_Angeles"*. Models are extremely good at consuming
structured error feedback if you give it to them. They are
extremely bad at parsing prose error messages dressed up as
exceptions. The hint field is two extra lines of code. It pays
for itself the first time the model recovers from a typo.

## What "the build" actually means

Eight commits, all in a single ~hour-and-a-half window:

```
51798f3  17:50:31Z  Initial commit
31e1d18  18:04:40Z  v0.1.0 scaffold
cad9560  18:10:27Z  Add CHANGELOG
63d6788  18:10:44Z  Align Cargo description with README pitch
c3d9d55  18:44:53Z  README: restructure install section, lead with Claude Desktop
af8176f  18:45:05Z  Bump version to 0.1.1
3eee2b8  18:45:19Z  CHANGELOG: 0.1.1 entry
9ee4e6f  19:21:47Z  Cleanup: commit Cargo.lock and ignore .claude/ session state
```

You'll notice something: only one of those commits is the actual
implementation. The rest are a CHANGELOG, a README polish, a
description tweak, a version bump, and a cleanup. That ratio is
typical. The "build" of a disposable tool is not the part where you
write the code. The code is the easy part. The build is the part
where you make the tool ready for the next person who's going to
look at it — which, in this case, is me, three weeks from now,
having forgotten everything.

The CHANGELOG is for me. The polished README is for me. The fact
that v0.1.0 and v0.1.1 are tagged separately even though v0.1.1
is purely a docs change — also for me. When I come back to this
repo in the future and want to know "did this tool do anything in
v0.1.1 that I should care about," I want the answer to be
discoverable in the CHANGELOG without my having to `git log -p`
through the tags. The two minutes I spent typing the CHANGELOG
saved me twenty minutes of forensics later.

This is one of the small disciplines that makes a disposable tool
not feel like garbage when you come back to it. The tool is for an
audience of one. The audience of one is forgetful.

## What the tool actually does in practice

I configured Claude Desktop to launch clock-mcp on every session
start. The five tools are now available in every conversation.

The behavior change is small in any one moment and large in
aggregate. I no longer type "today is Friday" into prompts. I no
longer correct dates. I no longer wonder whether Claude's "next
Tuesday" matches my "next Tuesday." When I ask the model to plan
something out by week, the durations are real durations. When I
ask it to convert a meeting time into Tokyo's zone, the conversion
is a function call, not a guess.

Here's a fragment of a tool call as it appears on the wire,
reconstructed:

```json
{
  "name": "time_until",
  "arguments": {
    "target": "2026-05-04T17:00:00-07:00",
    "now_zone": "America/Los_Angeles"
  }
}
```

And the response shape:

```json
{
  "from": "2026-05-01T11:14:42-07:00",
  "to":   "2026-05-04T17:00:00-07:00",
  "duration_seconds": 280518,
  "human": "3 days, 5 hours, 45 minutes"
}
```

That's the entire interface. Five of those, one for each tool. No
state. No subscription. No background task. No retry logic. No
auth. The server is stateless and idempotent. It does one thing —
report current time correctly — and the surface area is everything
you'd expect a wall clock to have and nothing else.

## The thing this chapter is *not* about

This chapter is not about how clever the implementation is. The
implementation isn't clever. `chrono` and `chrono-tz` do the work.
The interesting part of clock-mcp is not the code. The interesting
part is the noticing — the part where I stopped tolerating a small,
recurring annoyance, and built ninety-one minutes of work to make
it go away.

I'd been correcting Claude's date for months. Months. The
correcting cost was tiny per occurrence, which is exactly how
friction camouflages itself. The total annual cost was probably
hours, possibly days. I never noticed it adding up because I was
absorbing it one occurrence at a time.

Once the tool existed, the absence of the friction was
disorienting. Sessions ran differently. I asked the model harder
time-related questions because I trusted the answers. The cost of
*not* having the tool turned out to be much larger than the cost
of building it. I just couldn't see it from inside the friction.

That's a thing to remember. The cost of the absence is invisible
from inside the absence. You can only see it from outside —
which means the only way to know whether a disposable tool was
worth building is to build it and see what changes. The cost of
the build is so low now that this is a reasonable way to discover
the answer.

## A note on overengineering

clock-mcp could have been a 30-line script. It's instead a
published Rust crate with CI, a CHANGELOG, two release tags, and
five tools. Was that overengineering?

Honestly, no. The crate-shaped version costs me almost nothing
extra over the script-shaped version. The MCP integration would
have to exist either way. The five tools are five tools either
way. The CI pipeline I copy-pasted from another small Rust crate I
wrote and took less than a minute to wire up. The publish to
crates.io was `cargo publish` and a couple of metadata lines in
`Cargo.toml`.

The version of overengineering you do need to watch for in
disposable tools isn't *"I shipped a Rust crate when a script
would have done."* The Rust crate does the same thing the script
would have done; the binary just runs faster and the install path
is cleaner. The version of overengineering to watch for is *"I
introduced a config system, a plugin loader, and a domain-specific
language for things that don't exist yet."* That's the trap.
clock-mcp doesn't have any of that. It has five tools and no
ceremony around them. That's the line.

The next chapter draws the line in print. Scope is the whole game.
