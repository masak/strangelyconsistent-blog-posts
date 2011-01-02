---
title: Scripters, now is the time!
author: Carl Mäsak
created: 2008-12-30T12:22:00+01:00
---
Instead of futilely trying to sleep last night, I sat down and tried out an idea in Rakudo. Just a quick throwaway script, comprising a couple of arrays and a few for loops.

Now, I *know* that as soon as you expand your repertoire into something new, you run into bugs in Rakudo. But this time I ran into 7 bugs in the course of an hour. That surprised even me.

Actually, of those 7 "bugs", one was a TODO feature request, and one was a general question about syntax... but still, 5 bugs in an hour is a lot. This tells me three things:

1.  **A lot has happened in Rakudo lately.** And the last thing I want is for this to come through as a critique of the Rakudo development process, of which I'm constantly impressed. But the fact that I ran into those bugs tells me that I'm one of the very first people to actually try those features.
2.  **Tests are great, but they're no substitute for real programs.** Rakudo has a lot of tests, and all of them pass most of the time. But actual programs tend to shake out slightly different kinds of bugs, it turns out. Even if the "actual program" happens to be a toy problem thrown together at one o'clock in the night, as in my case.
3.  **We need more people who love to throw together scripts in Perl 6.** And who are eager to try them out in Rakudo, and who care enough when things break to report bugs to `rakudobug@perl.org`. I know you're out there.

Rakudo has evolved at an amazing rate the last few months. You're likely to find Perl 6 features you've heard about already implemented in Rakudo, and most of the time, they work as specced! Did I mention that it feels great to be writing code in Perl 6, and then typing `perl6 code-in-perl6` on the command line, and seeing it run?

We need more people who think it's great to write and run Perl 6 programs. With more eyeballs and hands, more bugs will be unearthed sooner, and Rakudo will be a stable, production-usable product sooner.

If you want to get involved, check out eric256's [Perl 6 examples repository](http://github.com/eric256/perl6-examples/), and think of something you'd like to add to that. Or take pmichaud's [challenge](http://use.perl.org/~pmichaud/journal/38134). Or just come visit at #perl6 @ irc.freenode.net and try some quick Perl 6 one-liners on the eval bot hanging around, discussing the finer points of syntax with the regulars. Or just scratch a scripting itch of your own.

Whatever you do, there's a small chance you might turn up what appears to be a bug. If you do, and if you submit that bug to `rakudobug@perl.org`, you're a hero, because you've made Rakudo Perl 6 a little bit more stable.

I'm sure there's lots of that type of heroes out there, just waiting for some cool new piece of software to try. Try Rakudo.


