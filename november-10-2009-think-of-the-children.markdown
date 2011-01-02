---
title: November 10 2009 &#8212; think of the children!
author: Carl Mäsak
created: 2009-11-11T00:59:00+01:00
---
40 years ago today, a [children's TV show](http://en.wikipedia.org/wiki/Sesame_Street), which was to turn out to be very successful and educational to boot, debuted:

<div class='quote'><p>In Sesame Street's first season, the Educational Testing Service (ETS) reported that the cognitive skills of its young viewers had increased by 62%. They found that children who viewed the show the most often did 62% better at correctly recognizing a rectangle than less frequent viewers.</p></div>

How long before we see a children's show teaching programming?

<p class='separator'>&#10086;</p>

Since it's [Temporal Tuesday](http://strangelyconsistent.org/blog/november-3-2009-doing-it-with-style-and-sophistication) today, I've been making commits on the [temporal Rakudo fork](http://github.com/masak/rakudo). Things are moving forward, if a bit slowly.

The only challenging part today was [converting day-of-year to month and day-of-month](http://github.com/masak/rakudo/commit/72fd37f7117461629a75807b447c0c0c5b707c4d). I'm not sure I got that 100% right (i.e. need to write more tests); also, I cheated a bit and disregarded leap years for now. I have a feeling this detail might undergo some change before the dust lands. Still, I like the feature (specifying year and day-of-year), and can see its use in some situations. It's valid ISO 8601, just as all the other datetime formats I'm implementing.

The reason I'm doing this work at all, is because I think the current way Rakudo handles dates and times is too bare-bones. I also think that adding a few convenience methods here and there to make it more useable out-of-the box — scratch that; I want to make it *super-simple* — is important, because most every semi-large application one writes will involve date and time. My argument boils down to not wanting to write

    Temporal::DateTime.new(:date(Temporal::Date.new(:year(2010), :month(4), :day(1))), :time(Temporal::Time.new(:hour(0), :minute(0), :second(0))), :timezone(Temporal::Timezone::Observance.new(:offset(0), :isdst(False), :abbreviation(''))))

when I want to refer to April 1st, 2010. I'd prefer something like

    DateTime.new('2010-04-01')

(You think the former syntax is a joke? Go look at the [Rakudo source](http://github.com/rakudo/rakudo/blob/master/src/setting/Temporal.pm)!)

**Update:** and in no way whatsoever do I wish to blame mberends++, the chief author of Rakudo's current Temporal.pm. He's basically implemented things to spec... which is why the spec needs [changing](http://github.com/masak/temporal-flux-perl6syn/blob/master/S32-setting-library/Temporal.pod), too.


