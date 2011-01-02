---
title: November 17 2010 &#8212; suppression and regressions
author: Carl Mäsak
created: 2010-11-17T23:48:00+01:00
---
21 years ago today, riot police suppressed a peaceful student demonstration in Prague, capital of Czechoslovakia.

<div class="quote">On the eve of International Students Day (the 50th anniversary of death of Jan Opletal, a Czech student murdered by the German occupiers during World War II), Slovak high school and university students organized a peaceful demonstration in the center of Bratislava. [...] They walked to Opletal's grave and - after the official end of the march - continued into downtown Prague, carrying banners and chanting anti-Communist slogans.</div>

This is all very reminiscent of the [Tiananmen Square protests (六四事件)](http://en.wikipedia.org/wiki/Tiananmen_Square_protests_of_1989) earlier that year, which also started with a grief-based demonstration, that time over the much more recent death of [Hu Yaobang](http://en.wikipedia.org/wiki/Hu_Yaobang), a well-liked reform-friendly politician.

Anyway, back in November and Czechoslovakia, the peaceful demonstration was brutally suppressed. [This image](http://en.wikipedia.org/wiki/File:Policemen_and_flowers.jpg) has become a classic symbol of the stark contrast &mdash; flowers against shields and guns.

The following detail reveals that the secret police may have had a role in orchestrating the events that followed:

<div class="quote">Once all the protesters were dispersed, one of the participants - secret police agent Ludvík Zifčák - kept lying on the street, posing as dead, and was later taken away. It is not clear why he did it, but the rumor of the "dead student" was perhaps critical for the shape of further events. That same evening, students and theatre actors agreed to go on strike.</div>

Things snowballed from there.

<div class="quote">By November 20 the number of peaceful protesters assembled in Prague had swollen from 200,000 the previous day to an estimated half-million. [...] With the collapse of other Warsaw Pact governments and increasing street protests, the Communist Party of Czechoslovakia announced on November 28 that it would relinquish power and dismantle the single-party state.</div>

I'm trying to think of something insightful here, about how the peaceful overturning of Communist states by the people is perhaps the ultimate sign that those states (and, by extention, the Marxism-Leninism practiced in those states) weren't working well. Then again, one might argue that details such as rampant nepotism, massive deportations, or barbed wire to prevent people from escaping the country, were just as clear signs.

<p class='separator'>&#10086;</p>

Last night I left off with an error in `Text::Markup::Wiki::MediaWiki` that seemed to indicate that something nasty was sent into `Str.trans`.

Well, there wasn't anything wrong with what was sent in. The call looks like this:

    my $cleaned_of_whitespace = $trimmed.trans( [ /\s+/ => ' ' ] );

Rakudo has no idea what to do with the regex `/\s+/` among the arguments. It used to obviously, since November once ran this line of code fine. But it doesn't any more. Yet another `ng` regression. I reported this as [rakudobug #79366](http://rt.perl.org/rt3/Ticket/Display.html?id=79366).

I'd like to fix this, preferably before the monthly release of Rakudo tomorrow. We'll see if I have time to write up a patch. (Unless someone beats me to it; hint, hint.) I don't think it'd be that difficult to fit into the [existing algorithm](http://strangelyconsistent.org/blog/november-7-2010-man-we-suck-at-this).
