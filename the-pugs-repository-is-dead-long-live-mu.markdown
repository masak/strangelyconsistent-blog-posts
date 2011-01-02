---
title: The Pugs repository is dead; long live Mu!
author: Carl Mäsak
created: 2010-09-06T08:30:00+02:00
---
This weekend marks the end of a quite astonishing era. The Pugs repo, that hosted all the [amazing Perl 6 activity](http://strangelyconsistent.org/blog/happy-10th-anniversary-perl-6), is no more. At its height, this repository had 242 committers! I just checked.

The Pugs repository has functioned as a common writing area for Perl 6-related things. First [Pugs](http://github.com/audreyt/pugs); then a lot of external [modules and scripts](http://github.com/perl6/perl6-examples) written in Perl 6; what would eventually morph into [the Perl 6 test suite](http://github.com/perl6/roast); even the [Perl 6 specification](http://github.com/perl6/specs) and the [standard grammar](http://github.com/perl6/std). Commit bits (write access credentials) were handed out liberally, and newcomers were encouraged to help improve things they found amiss. The degree to which this system worked without a hitch has been quite astonishing. There have been no edit wars, no vandalism, no banning. Just the continuing flow of commits. Trust the anarchy.

So why are we killing off the Pugs repository? Well, technologies come and go; not so much because they worsen by each year, but because our expectations rise in a sort of software inflation. The SVN repository became too high-maintenance, and a transition to git was the natural next step. The Pugs repository has been split up into the git repositories linked to in the previous paragraph. Those bits that don't belong in any of the standard bins remain in the [Mu repository](http://github.com/perl6/mu). (Its name is a reference to the most-general object type in Perl 6, what in other languages is commonly known as 'Object'.)

I for one salute our new git-based overlords! May the commits keep flowing even under this new system. Also, moritz++ for carrying out the move.


