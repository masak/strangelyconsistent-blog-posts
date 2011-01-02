---
title: November 27 2009 &#8212; straight towards the surface
author: Carl Mäsak
created: 2009-11-28T00:58:00+01:00
---
38 years ago today, the *Mars 2* orbiter [releases a descent module](http://en.wikipedia.org/wiki/Mars_2#Lander) towards the surface of Mars.

<div class='quote'><p>The descent module separated from the orbiter on 27 November 1971 about 4.5 hours before reaching Mars. After entering the atmosphere at approximately 6 km/s, the descent system on the module malfunctioned, possibly because the angle of entry was too steep. The descent sequence did not operate as planned and the parachute did not deploy. Mars 2 was the first manmade object to reach the surface of Mars. The landing site is unknown.</p></div>

*Mars 3*, practically at the coat-tails of its sibling, made a descent a few days later, and landed softly.

<p class='separator'>&#10086;</p>

Today I wrote a toy script, which might get integrated into the [Perl 6 book](http://github.com/perl6/book) we're writing.

The script shuffles a full deck of cards, draws two hands from it, and compares them. Could easily be extended into a full-fledged poker game. [Here it is](http://gist.github.com/244255).

Writing it was remarkably fun, and it shook out [a](http://rt.perl.org/rt3/Ticket/Display.html?id=70888) [couple](http://rt.perl.org/rt3//Public/Bug/Display.html?id=70894) rakudobugs.

Look carefully at the use of `where` clauses in the script. We found out on the channel that `where` clauses are much more powerful than any of us had realized until now. Here are a few of the more beautiful ones:

    subset FullHouse of PokerHand where OnePair & ThreeOfAKind;

    subset Flush of PokerHand where -> @cards { [==] @cards>>.suit }

    subset StraightFlush of Flush where Straight;

I originally had that last one as `Straight where Flush`, but I think the `Flush` computation is faster, so I put that one first, according to some fail-fast principle.

The script also raised the question "how does one get all the values of an enum", and TimToady gave the tentative answer `SomeEnum::.keys`, but this [does not work](http://rt.perl.org/rt3/Ticket/Display.html?id=70894) in Rakudo yet.


