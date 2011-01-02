---
title: November 15 2009 &#8212; money gets inflated, time gets deflated
author: Carl Mäsak
created: 2009-11-16T00:40:00+01:00
---
86 years ago today, [the Rentenmark was introduced](http://en.wikipedia.org/wiki/German_Rentenmark) in Germany to replace the old, worthless currency:

<div class='quote'><p>The Rentenmark replaced the Papiermark. Due to the economic crises in Germany after the Great War there was no gold available to back the currency. Therefore the Rentenbank, which issued the Rentenmark, mortgaged land and industrial goods worth 3.2 billion Rentenmark to back the new currency. The Rentenmark was introduced at a rate 1 Rentenmark = 1:10^12 Papiermark, establishing an exchange rate of 1 United States dollar = 4.2 RM.</p></div>

Now, that's an impressive exchange rate. Hm, [hyperinflation](http://en.wikipedia.org/wiki/Hyperinflation). Now, that sounds like a possibly interesting Wikipedia article:

<div class='quote'><ul><li>By late 1923, the Weimar Republic of Germany was issuing two-trillion Mark banknotes and postage stamps with a face value of fifty billion Mark. The highest value banknote issued by the Weimar government's Reichsbank had a face value of 100 trillion Mark (100,000,000,000,000; 100 billion on the long scale). One of the firms printing these notes submitted an invoice for the work to the Reichsbank for 32,776,899,763,734,490,417.05 (3.28&#215;10^19, or 33 quintillion) Marks.</li><li>The largest denomination banknote ever officially issued for circulation was in 1946 by the Hungarian National Bank for the amount of 100 quintillion peng&#337; (100,000,000,000,000,000,000, or 10^20; 100 trillion on the long scale). image (There was even a banknote worth 10 times more, i.e. 10^21 peng&#337;, printed, but not issued image.) The banknotes however didn't depict the numbers, "hundred million p.-peng&#337;" ("hundred million billion peng&#337;") and "one milliard p.-peng&#337;" were spelled out instead. This makes the 100,000,000,000,000 Zimbabwean dollar banknotes the notes with the greatest number of zeros shown.</li><li>The Post-WWII hyperinflation of Hungary held the record for the most extreme monthly inflation rate ever &#8212; 41,900,000,000,000,000% (4.19 &#215; 10^16%) for July, 1946, amounting to prices doubling every thirteen and half hours. By comparison, recent figures (as of 14 November 2008) estimate Zimbabwe's annual inflation rate at 89.7 sextillion (10^21) percent. which corresponds to a monthly rate of 5473%, and a half-life of about five days.</li></ul></div>

Yikes. About the only time you do not want to see lots of zeros on your banknotes is when everyone else has them, too.

<p class='separator'>&#10086;</p>

Today I'm well-rested, but I wasted much of my allotted time writing other stuff. Since I don't have time to do anything big, I'll fix a small thing that has annoyed me in the last couple of days: after I [went and deleted the `Makefile.PL` from November](http://strangelyconsistent.org/blog/november-9-2009-stuff-comes-tumbling-down-yay), the nightly smokes have been coming out wrong. So I went looking for the error.

It wasn't a big error. The build script needed to stop try and run that `Makefile.PL`, and there was a local change in the `Makefile.PL` itself, which prevented git from automatically removing it. After fixing these two things, I again have [green-and-clean](http://feather.perl6.nl/~masak/november-smoke.html) test reports.

Now if I could only find the time to dig into lichtkind++'s bugs. That last one, [with the login problem](http://strangelyconsistent.org/blog/november-11-2009-nobody-said-it-was-going-to-be-easy), was kinda fun in retrospect.


