---
title: It's about time
author: Carl Mäsak
created: 2010-04-09T09:49:00+02:00
---
jnthn++ [touched](http://use.perl.org/~JonathanWorthington/journal/40303) upon the subject. I thought I'd do the same. We're [rewriting the Temporal spec from scratch](http://www.nntp.perl.org/group/perl.perl6.language/2010/04/msg33480.html). It's not the first time this happens, but for some reasons, this attempt feels better than the previous ones.

Ever since the Web.pm work had its course [plotted](http://strangelyconsistent.org/blog/week-11-of-webpm-hitomi-does-templating) in more detail — not to mention since the work on [GGE](http://github.com/masak/gge) — I feel I belong to the "shameless copycat" school of design. More specifically, in many domains *not* directly related to the Perl 6 core model, our best chance of success is likely not to be oh-so-clever, but to start with something that works well in some other language (big-sister Perl 5, Ruby, Haskell, JavaScript, what-have-you), adapt it to Perl 6 idioms, and ship it. In the case of Temporal, the clear winner was CPAN's DateTime, a subset of which is now in [Synopsis 32](http://svn.pugscode.org/pugs/docs/Perl6/Spec/S32-setting-library/Temporal.pod).

Imitation may be the sincerest form of flattery, but basing your design on something that already works also seems a fairly safe way to make sure that the design you end up with isn't and abstraction-laden heap of wishful thinking.


