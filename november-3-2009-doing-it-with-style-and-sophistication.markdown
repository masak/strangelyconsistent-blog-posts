---
title: November 3 2009 &#8212; doing it with style and sophistication
author: Carl Mäsak
created: 2009-11-04T01:01:00+01:00
---
126 years ago today, a gentleman bandit in the American Old West, got away with his last stagecoach robbery, but left an incriminating clue that [eventually led to his capture](http://en.wikipedia.org/wiki/Charles_Bolles):

<div class='quote'><p>Wells Fargo Detective James B. Hume (who allegedly looked enough like Bolles to be a twin brother, moustache included) found several personal items at the scene, including one of Bart's handkerchiefs bearing the laundry mark F.X.O.7.<br></br>Wells Fargo detectives James Hume and Henry Nicholson Morse contacted every laundry in San Francisco, seeking the one that used the mark. After visiting nearly 90 laundry operators, they finally traced the mark to Ferguson &amp; Bigg's California Laundry on Bush Street. They were able to identify the handkerchief as belonging to Bolles, who lived in a modest boarding house.</p></div>

90 laundry operators! That's endurance, if you ask me. I think my favorite quote from that Wikipedia article is this:

<div class='quote'><p>The fame he received for his numerous daring thefts is rivaled only by his reputation for style and sophistication.</p></div>

If you're to be a villain in the Old West, why not do it with style?

<p class='separator'>&#10086;</p>

It's Temporal Tuesday. Some of you might not know what "Temporal Tuesday" means; that's because I just invented the term.

I have a long-standing project, to make a lot of things right in the Perl 6 spec on date and time. I'm not sure I can summarize those things succinctly here; instead, just read up on [all I've ever said about Temporal on IRC](http://irclog.perlgeek.de/search.pl?channel=perl6&amp;nick=masak&amp;q=temporal) if you're interested.

Anyway, I've decided to allocate parts of the November blog spree to actually making a solid case for a Temporal change. The idea is to have something to post to p6l by the end of the month. By "something", I mean a consistent [spec](http://github.com/masak/temporal-flux-perl6syn/blob/master/S32-setting-library/Temporal.pod), comprehensive spectests, and a working Rakudo implementation.

So that's what I started on today. Here's my progress: a [test file](http://github.com/masak/rakudo/blob/master/temporal.t) and a [class](http://github.com/masak/rakudo/blob/master/src/setting/Temporal.pm).


