---
title: Weeks 4 and 5 of GSoC work on Buf &#8212; chrono-flies
author: Carl Mäsak
created: 2010-06-28T23:27:00+02:00
---
Chrono-flies (*Drosophila chronogaster*, commonly known as "time-flies") are known for their fondness for arrows. Lately I've been distracted enough (by `$DAYJOB`, other Perl 6 projects, and other non-Perl 6 projects) to let two weeks pass by with only one commit in my local branch:

-  `Buf` is now `Positional`, and you can index it with `postcircumfix:<[ ]>`.

I merged/pushed that one a few moments ago.

For good measure (pronounced "only one commit! how'd that happen?"), I did some pre-investigation this evening as to how one might read binary data from a file into a `Buf`. [cotto++ outlined how](http://irclog.perlgeek.de/parrot/2010-06-28#i_2491061). Haven't finished thinking about this, but it does seem perfectly doable, which is a notch better than I feared. 哈哈

Also, worth noting here: remember how in Week 2, I [changed the `Buf` constructor spec](http://strangelyconsistent.org/blog/week-2-of-gsoc-work-on-buf-the-power-of-swedish-beer) from slurpy array to non-slurpy array? Well, [pmichaud wondered why](http://irclog.perlgeek.de/perl6/2010-06-27#i_2486527), and it led to an interesting discussion. Will synch up with jnthn to see if it perhaps merits changing back for consistency with other list-y types.


