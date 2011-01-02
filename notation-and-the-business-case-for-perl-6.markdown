---
title: Notation, and the 'business case' for Perl 6
author: Carl Mäsak
created: 2009-12-27T20:39:00+01:00
---
A few days before Christmas, I was sitting in the car with my father. I decided to try to explain why I'm spending time doing Perl 6.

I compared the evolution of programming languages with the evolution of mathematical notation. In maths, it was [tricky to imagine](http://en.wikipedia.org/wiki/Linguistic_relativity) the concept of [zero](http://en.wikipedia.org/wiki/Zero#Early_history) before it was formalized down to a number; the idea of derivatives was tossed around in vague forms before [Newton and Leibniz](http://en.wikipedia.org/wiki/Derivative) made notations for it; we slowly but surely got a standard way of saying ["is equal to"](http://en.wikipedia.org/wiki/Equals_sign). And so on.

I was just about to connect this back to how each new programming language can be seen as a contribution to an ongoing, open debate about notation within the programming world, one that has only been going on in earnest for half a century or so. But I never got that far, because my father interrupted me and asked what the [business case](http://en.wikipedia.org/wiki/Business_case) is for Perl 6.

I was half stumped, half amused by this interjection. I didn't have a good answer for him, besides trying to explain FOSS and the fact that we're not developing Perl 6 with the expectation that someone will buy it from us. But I still felt he had a point; so, earlier today, I took the question to `#perl6`, and got a set of good [replies and musings](http://irclog.perlgeek.de/perl6/2009-12-27#i_1871329) by Su-Shee++, moritz++, vorner++ and mdxi++. If you have time, do read it.

I'll have to give the 'business case' train-of-thought a lot more time to mature before I can say anything coherent about it myself. But I'd like to say a bit more about notations.

Perl 6 isn't truly revolutionary in a lot of respects. It's mostly lots and lots of minor improvements. (The exception being, I think, things surrounding grammars. Those are truly revolutionary.)

What Perl 6 does do, is provide a 'strangely consistent' notation where a lot of thoughts, with practice, are easy to express. This is very much in the Perl spirit, and the main reason (according to me) to still call Perl 6 a language in the Perl family.

The notational convenience really consists of a thousand little things, but here are a few haphazardly chosen examples:

- The prefix symbols `?`, `+` and `~` have been co-opted to signify conversion to the 'contexts' `Bool`, `Num` and `Str`, respectively. This parallels their use in binary (infix) operators specific to those contexts, such as `+`, `~`, `?&`, `+&`, and `~&` 
- The three short and common verbs `is`, `has` and `does` all do useful work in the OO system. Not only that, but they are a good fit for what they do, and summarize both established and modern OO research.
- Some syntactic/semantic features are re-used where you least expect it, reducing the total number of independent components in the language. The left-hand side of a list assignment uses the same signature mechanism as do routines. (I.e. the `(Bool $a, Int $b, Str $c)` in `my **(Bool $a, Int $b, Str $c)** = True, 42, 'foo';` is the same kind of beast as in `sub foo**(Bool $a, Int $b, Str $c)** { ... }`.) Smartmatching with `~~` is implicitly re-used in `when` expressions, and the `given`/`when` construct is re-used as the `CATCH` exception handling mechanism. And so on.

It's things like these that I think make Perl 6 a convenient, attractive notation for thinking about programming. Seeing that notation come alive, and seeing people use it in cool new ways for amazing ends, is the main reason I spend time helping with Perl 6.


