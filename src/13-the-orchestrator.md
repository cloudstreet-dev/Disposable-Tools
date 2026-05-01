# Two producers who trust each other

The relationship between you and the AI, when this works, is
neither *user-tool* nor *boss-employee*. It is *two producers who
trust each other.* The frame matters because the wrong frame
quietly poisons the work.

If you treat the AI as a tool, you'll under-use it. You'll write
prompts that ask for atomic outputs — *generate this function,
write this test* — and you'll glue them together yourself,
shouldering all the integration work. The AI is much more capable
than that, and treating it as a tool wastes most of its capacity.

If you treat the AI as an employee — a junior dev who takes
direction — you'll over-direct. You'll micromanage, second-guess,
correct on style, and generally treat the relationship as
hierarchical. That works, sort of, but it's exhausting and slow,
and it leaves the AI's actual strengths underused. The AI is not
a junior dev. It's something else.

The frame I want to land on is *two producers who trust each
other.* You bring scope, taste, and judgment. The AI brings speed,
breadth of recall, and tireless boilerplate. Each side is doing
real producer work. Each side respects the other's role.

## What "producer" means here

In music, a producer is the person who shapes the record. Not
the songwriter — that's the artist. Not the engineer who runs
the console. The producer is the person responsible for the
*overall outcome*, who makes the structural decisions, who
decides which take is the keeper, who stops the band from
wandering into a forty-minute jam, who ships.

In disposable-tool building, *you* are the producer. You decide
the tool exists at all. You decide what it does, in one
sentence. You decide what's in scope and what's out. You decide
when to ship and when to stop.

But here's the thing: the AI is also a producer. Not of the
overall record — that's still your job — but of the *takes*.
The AI generates the candidate code, structures the boilerplate,
fills in the obvious bits, drafts the README. It generates fast
and prolifically. It also has opinions, and those opinions are
worth listening to even when you don't take them.

A good producer-producer relationship has two-way trust. You
trust the AI to generate competent first drafts without your
having to specify every detail. The AI trusts you to make the
keep/cut/modify decisions and not litigate every choice. The
trust is what makes the work fast. Without the trust, you fall
back into one of the bad frames.

## What you bring

The producer-producer frame breaks down what each side is
actually doing. Your half:

- **The one-sentence scope.** The AI cannot decide what the tool
  should be. Even if you ask it to brainstorm, what comes back
  is a candidate list, not the answer. The answer requires
  knowing your actual problem, your actual constraints, your
  actual taste. That's you.

- **The cuts.** The AI tends toward inclusion. Asked to build a
  thing, it builds the thing plus three reasonable extensions
  plus one defensive fallback plus logging. You cut. Cutting is
  taste, and taste is a human responsibility.

- **The integration test.** Did this tool, as built, solve the
  problem? Only you know what the problem felt like. The AI
  has never felt your friction. The AI cannot tell whether the
  tool fixed the friction or just produced something
  tool-shaped. You run the tool against the actual use case and
  report back.

- **The stop.** When does the tool ship and when do you walk
  away? The AI will keep going as long as you keep asking. The
  AI is not going to look at the clock and say *we're done
  here.* That's also you.

## What the AI brings

The AI's half:

- **The skeleton.** Cargo.toml, the imports, the trait
  implementations, the obvious matches and switches, the test
  scaffolding. All the regular code that any version of this
  tool would have. Generating this from scratch is fast, and
  generating it correctly is *very* fast for the AI compared to
  you doing it by hand.

- **The breadth.** The AI has read a lot of code. It can recall
  idioms from `tokio` you've never seen, suggest a crate that
  fits your purpose, draft an MCP server boilerplate that's
  about 80% correct on the first try. That breadth, applied to
  your specific narrow problem, is most of where the speed
  comes from.

- **The honest first draft.** The AI doesn't know what
  *embarrassment* is. It will write the function the most
  obvious way, on the assumption that the obvious way is fine,
  and most of the time it is fine. Junior humans often
  over-elaborate to look good. The AI doesn't have that
  pathology. Its first drafts are usually closer to the right
  shape than a junior human's first drafts.

- **The tireless mechanical work.** Adding a CHANGELOG entry,
  bumping a version, writing the README's install section in
  three flavors, generating the GitHub Actions workflow file.
  All the stuff that you'd do half-asleep, the AI does well.

- **A second pair of eyes.** This is underrated. Asking the AI
  to read a function you wrote and tell you what it does is a
  cheap way to check whether the code's intent matches its
  effect. If the AI's summary doesn't match what you meant, the
  code is wrong (or the names are wrong). This is real
  reviewing, and it's free.

## The flow, in practice

A typical disposable-tool session, with the producer-producer
frame on:

1. **You:** open a new repo, write the one-sentence scope.
2. **AI:** generates the scaffold — Cargo.toml, main.rs, the
   first compilable tool stub.
3. **You:** read it. Cut what's not needed. Note any drift.
4. **AI:** generates the next piece, parameterized by your
   notes.
5. **You:** read it. Run it. The integration test happens.
6. **AI:** fixes what didn't work, based on your concrete
   feedback (the actual error, not vibes).
7. **You:** ship.

That loop runs many times in an afternoon. The cadence is
quick. Neither side is waiting on the other for very long. You
spend more time *reading* AI output than *writing* prompts —
which is how you know the frame is right. If you're spending
more time prompting than reading, you're under-trusting. If
you're spending more time reading than prompting, you're
over-prompting.

## Trust isn't credulity

I want to be clear what trust means here. It does not mean
*believe everything the AI says.* It means *take the AI's first
draft seriously enough to read it carefully, and then exercise
your judgment.* The AI is wrong sometimes. It's wrong in ways
that are hard to predict — confidently wrong, plausibly wrong,
wrong-in-the-shape-of-correct. Reading carefully is the only
defense. Trust without reading is credulity, and credulity is
how disposable tools get bugs that survive into production.

shell-mcp's launch-root bug is not, fundamentally, an AI bug.
The AI generated code that worked the way I asked it to work —
"use the cwd as the launch root." The bug was a *judgment*
failure on my side. I didn't notice that "use cwd" was wrong
for the integration target until I shipped. Trust did its job —
I read the code, I understood what it did, I shipped it. The
bug was in my model of the world, not in the code itself.

That distinction matters because the temptation, after a bug
ships, is to over-correct toward suspicion. *Don't trust the AI.*
That's the wrong lesson. The right lesson is: *trust requires
that you remain the producer.* If you stop producing — stop
exercising judgment, stop running the integration test, stop
deciding when to ship and when to stop — then trust collapses
into credulity, and credulity is what produces actually bad
code. As long as you're producing, you can trust the AI as much
as you'd trust a competent collaborator with their own
specialty.

## A note on this book's collaboration

This book has two authors: David Liedle and Claude Code. That
byline isn't decorative. The book was written the way the tools
were built — prompt-then-pace, two producers, one record. The
voice is mine because the producer is mine; the prose is, in
many places, drafted by Claude and then cut, reshaped, and
finalized. The pattern is the same pattern that built every
case study in the previous chapters.

If you're looking for the ratio: roughly two-thirds of the words
on this page started as Claude's draft. Roughly one-quarter
survived unchanged. The rest was rewritten, cut, or rearranged
by me. That's a reasonable ratio for the producer-producer
frame. It's also why I want the byline plural — pretending I did
all the typing would misrepresent the relationship.

The next chapter is about the relationship breaking. Anti-
patterns: ways disposable tools and the orchestrator-AI dance
go wrong. Some of the failures are funny. Some are instructive.
None of them are fatal — disposable means you can throw the
artifact away — but the patterns are worth recognizing so you
catch them in your own work.
