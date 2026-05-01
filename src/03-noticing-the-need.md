# Noticing what's missing

The hardest part of building a disposable tool is not building it.
It's noticing that there's a thing to build at all.

Most of the time, when something is missing in your work, you don't
register it as missing. You register it as friction, and friction
gets absorbed. You re-export the same Slack workspace four times
this year, fighting the export format each time, and you don't
think *I should build a viewer.* You think *ugh, this format,* and
you keep going. The friction is so well-integrated into the work
that the work would feel weird without it.

The friction is the signal. You have to learn to listen to it.

## The shape of the signal

You'll feel the signal as a small, repeated, mostly-private
annoyance. It's almost never a thunderclap. Some shapes it takes:

- *I keep doing this exact thing by hand.* You've typed the same
  sequence of commands fourteen times this week. You've thought
  about a shell alias. You haven't made one because the inputs
  are slightly different each time and an alias wouldn't quite
  fit, but a small program would.

- *I keep avoiding this thing because the existing tool for it is
  bad.* You let your bookmarks pile up because the bookmark
  manager doesn't know what you meant. You don't read your old
  Slack archives because the export is unreadable. You don't check
  your stale repos because going from org to org in the GitHub UI
  takes ten minutes you don't have.

- *I keep telling Claude something it should already know.* The
  current time. Your timezone. The fact that today is Tuesday.
  This one is its own genre now. Whenever you find yourself typing
  the same context into a prompt across multiple sessions, that
  context is a tool waiting to happen.

- *I keep wishing for a thing that doesn't exist, then giving up
  on the wish before I'd even fully formed it.* This is the
  trickiest signal because the wish gets cut off so fast you
  barely remember thinking it. The wish-cutting reflex is calibrated
  for the old cost equation. You'll need to recalibrate it.

## The reflex you're disabling

The reflex you grew up with goes like this: you notice an absence,
you start to imagine the tool that would fill it, and somewhere
around the third sentence of imagining it your brain says *who has
the time?* and the wish dissolves. The reflex is protective.
It saved you from a thousand half-finished side projects in the
years when finishing a side project actually cost a weekend.

The reflex is now wrong. Not always wrong — some tools really do
take a weekend, or a month, and the reflex is still calling those
correctly. But for the small bespoke tool, the disposable kind,
the reflex is firing on stale data.

The thing to do, when you feel the wish-cutting reflex fire, is
to pause it for thirty seconds and run a different check. The
check is: *if I had this tool right now, what would I do with it
in the next hour?* If you have an answer — concrete, specific, the
actual data and the actual command — you have a tool worth an
afternoon.

## A worked example

Here's the wish-cut, in slow motion, from the morning I built
clock-mcp.

I was in a Claude session, mid-architecture. I asked it about a
release schedule. It answered confidently with dates. The dates
were wrong. They were wrong because Claude didn't know what year
it was. I corrected it. I went on with my work. Three sentences
later I asked it to compute a duration — "how long until next
Tuesday?" — and it gave me an answer that assumed today was a
date from training-cutoff time, not now.

The old reflex fires here: *yeah, models don't know what time it
is, that's the deal, move on.* The wish dissolves. I keep
correcting it manually for the rest of the session.

Pause. Run the new check. *If I had a tool that gave Claude a
wall clock, what would I do with it in the next hour?* The answer
is concrete: I'd ask Claude to plan my week, and it would compute
durations correctly without me babysitting. The data is concrete:
real timestamps, real timezones. The command is concrete: an MCP
server with a `now` tool, a `convert_timezone` tool, a few duration
helpers. The whole shape is visible. The unknowns are small.

That's a tool. Build it.

The next chapter is about the tool I built. The whole arc, init
through `crates.io`, took ninety-one minutes.

## Sharpening the noticing

You can practice this. The practice is small and weird and looks
like nothing. Whenever you find yourself typing context into a
prompt that you've typed before, write it down somewhere — a
plaintext file is fine, an issue in a private repo is fine, a
note in your phone is fine. Don't act on it yet. Just collect.

After two weeks you'll have a list of ten or fifteen recurring
contexts, and three or four of them will resolve into the same
shape: a small piece of state your tools don't know about, that
you keep restating. Those are the disposable tools you should
build first. They're concrete because the context you've been
restating *is* the spec.

You'll also notice — and this is the secondary lesson — that
some of the items on the list resolve into nothing. They were
not friction. They were complaint. The difference between
friction and complaint is that friction has a workaround you keep
running, and complaint is just dissatisfaction in search of a
target. You don't build tools for complaint. You build tools
for friction, because the workaround is the spec.

## The cost has not always been low

A reminder, because the reflex is hard to retrain: this didn't
used to be true. There were years when noticing the missing
tool was the *easy* part and building it was the cliff. Now the
cliff is gone — it's a slope — and the bottleneck has moved
upstream. The bottleneck is *seeing the absence at all.*

The bottleneck has moved. Move with it.
