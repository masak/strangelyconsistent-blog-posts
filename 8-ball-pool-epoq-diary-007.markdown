---
title: 8 ball pool (epoq diary 007)
author: Carl Mäsak
created: 2020-09-09T22:10:00+08:00
---
I don't play games on my phone much these days, but for [8 ball pool](https://cn.bing.com/images/search?q=8+ball+pool+miniclip) I make an exception.
It's relaxing, and somehow doesn't feel like a complete waste of time.

There's something strangely alluring about a game that's based entirely on Newton's laws of motion.
Good shots and bad, it really feels like a game where the only luck is the one you make yourself.

In fact, it's like the game is happening on two levels:

1. (Physics) The inertial motion and elastic collisions of the balls.
2. (Rules) In-game consequences of the cue ball hitting/not hitting other balls, and balls being pocketed.

I can almost see this forming the basis of a really nice object-oriented implementation of the game, where the "Physics" event stream feeds into the "Rules" event stream, not unlike how a token stream feeds into a parser.
A nice layering.

It occurred to me only the other day that the game pits me not just against other human players, but against AI players too sometimes.
It does a pretty good job at hiding the difference, which is why I hadn't noticed.

If that were all, I probably wouldn't set aside a blog post just for a game.
But 8 ball pool is an excellent example of _real-time distributed document editing_.
Yes, like Google Docs, except the shared state isn't a Word file, it's a game.

Building a distributed system is hard.
Not just "engineering challenge" hard, but "theoretically impossible" hard.
There is inevitably a difficult sacrifice to be made somewhere, and it needs to be made gracefully.

Let's say I'm playing against another human player with a phone somewhere else in the world.
The pleasant illusion/abstraction the distributed game aims to convey is that there is a single game being played, with only a single game state.
Except that's patently untrue: there are definitely two machines/states involved, separated by lots of air and cable. (Quite probably three, counting a central server.)

What am I saying here; that [abstractions leak](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)?
Am I invoking [the CAP theorem](https://www.ibm.com/cloud/learn/cap-theorem) as well as [CAP theorem 12 years later](https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/)?
Am I reminding the enlightened reader that The First Law of Distributed Object Design is [You Do Not Talk About Fight Club](https://www.martinfowler.com/bliki/FirstLaw.html)?
Yes, yes, yes, and yes.

The frustrating thing about a distributed system is that the terms "distributed" and "system" are irreconcilable.
A distributed system isn't one system anymore &mdash; it's two or more of them.
A leaky abstraction can patch it up and heal sometimes, but sometimes (like with a Git merge conflict) there simply isn't a machine-only fix.

The game?
Well, it's still good fun and good relaxation on my commute.
The most frustrating thing is the _rare_ occasions where the game rolls back because it turns out I was disconnected without noticing, and instead of making that great shot, I had silently run out of time and ceded my turn.
Or sometimes, due to a bad connection or whatnot during parts of the commute, I simply lose contact with the game and get booted out.

I tell myself it's OK, because distributed systems are impossible.
