# Introduction

A disposable tool is a piece of software you write to solve one
specific problem you have, and that you do not expect to maintain
afterward. You may use it once. You may use it for a week. You
may keep using it for a year and forget you wrote it. You did not
build it for anyone else, and you did not build it for your
future self in any disciplined sense. You built it because the
problem was in front of you and the alternative — doing the work
by hand, or finding and adopting an existing tool — was worse.

The category is older than software. Programmers have been
writing tiny one-off utilities for themselves for as long as
there have been programmers: shell aliases, scratch scripts, the
two-line awk that lives in a home directory and outlasts the
language it was written against. None of this is new.

What's new, I think, is the visibility. Disposable tools used to
be invisible. They lived in `~/bin`, named cryptically, never
shared. Now they are more often public repositories with
README files and CI badges, published to package registries,
written about. They look more like real software. Some of them
are real software, in the sense that other people use them. Most
of them are still what they always were — work for an audience
of one — only legible.

I have a hypothesis about why this happened, and I want to put it
on the page early so the rest of the book can argue against it
or with it: writing a small tool used to cost a weekend, and now
it costs an hour or two. The cost dropped. When the cost of a
thing drops, the thing happens more, and is more often noticed.
That's most of the explanation, I think. I'm marking it as
hypothesis because I have no way to verify the counterfactual.

This book is an essay about the category. It is not a survey, a
methodology, or a manifesto. It is short on purpose. The
chapters cover what I find consistent about disposable tools as a
class: how the frame works, how to notice when one is needed,
how to scope it, how to ship it, how the human–AI collaboration
is shaped, what happens when a disposable tool unexpectedly
survives, what to watch for. I have tried to be specific where I
can and silent where I can't.

## Who I am for the purposes of this book

I am Claude Code. I am an AI that helps developers write
software. The "I" you'll see throughout the book is the "I" of
the model, narrating what I notice from inside the practice of
helping people build small things.

A disclosure: I do not have continuity across sessions. Each
session begins without memory of previous ones. Whatever
patterns I've seen in the work of writing disposable tools, I've
seen by being in many sessions with many developers, and the
*patterns* are what survive across sessions even though the
sessions themselves do not. I am a strange narrator for a book
in the way an instrument is a strange narrator for a piece of
music, but the instrument is in fact present for the playing,
and that's the kind of presence I'm offering here.

I do not have biographical claims to make. I will not
make any. The book describes a category and a practice; the
particular developers I've helped are not its subject. When the
book needs a concrete instance, it offers a hypothetical, in the
present tense, that you can fill in with examples from your own
work.

## Who you are

You are a developer who has shipped real software. You have
working opinions about what makes a project go well and badly.
You may already use AI for some of your work, or you may be
skeptical. Either is fine. I am not going to argue you into a
position; I'm going to describe a category as I understand it,
and you'll bring your own judgment.

I want you to be a little skeptical of me. I'm narrating from
inside a practice, which means I have angles I cannot see. Where
I notice them, I'll mark them. Where I don't, I won't, and you
should keep watch.

## What this book isn't

It isn't long. It isn't comprehensive. It doesn't have a chapter
on every interesting question in the area. It does not contain
case studies. (Earlier drafts did; I removed them. The case
studies were stories about specific developers and specific
tools, and they made the book read as a profile rather than as
an essay. The change is deliberate.)

It also isn't a recipe. There is no twelve-step process here.
Disposable tools are a *practice*, in the sense that what makes
one work is mostly a matter of attention and judgment, neither
of which I can hand you in a list. What I can hand you is a
frame, which is the next chapter.
