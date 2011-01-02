---
title: Week 1 of GSoC work on Buf &#8212; not a chocolate cake recipe
author: Carl Mäsak
created: 2010-05-28T16:29:00+02:00
---
No-one likes to trawl through long grant progress reports that read like a recipe for chocolate cake, but without the chocolate cake. So I'll be mercifully brief.

-  [Implemented](http://github.com/rakudo/rakudo/commit/c5fdb17fb3e642ef2789fa4ef5b27be72295d267) `Str.encode` and `Buf.decode`.
- Discovered two things in Parrot from doing Buf.decode. The first was [a bug](http://trac.parrot.org/parrot/ticket/1665) which bacek++ fixed quickly.
- The second was that [Parrot can't decode lists of bytes](http://irclog.perlgeek.de/perl6/2010-05-28#i_2377448). That's why [test 8](http://gist.github.com/417142) fails. We've been [discussing](http://irclog.perlgeek.de/parrot/2010-05-28#i_2377545) [how to](http://irclog.perlgeek.de/parrot/2010-05-28#i_2377661) [proceed](http://irclog.perlgeek.de/perl6/2010-05-28#i_2377770) from here.

By the [plan](http://gist.github.com/360097) I'm supposed to work on constructors and indexing now, but I gravitated towards the Str.encode and Buf.decode stuff because that's where I left off last time I worked on this. There's still plenty of time next week to do the constructors and indexing.

Not used to writing a report this small. But I think I like it. 哈哈

(What, this thing? Yes, it so happens it's a [chocolate cake](http://www.kevinandamanda.com/recipes/dessert/the-best-chocolate-cake.html). No, you can't have any. Get your own.)


