---
title: November 11, 2008 &#8212; the calm after the storm
author: Carl Mäsak
created: 2008-11-11T06:59:00+01:00
---
90 years ago today, at eleven in the morning, World War I ended. [Wikipedia](http://en.wikipedia.org/wiki/World_War_I#End_of_war):

<blockquote><div><p>Following the outbreak of the German Revolution, a republic was proclaimed on 9 November. The Kaiser fled to the Netherlands. On 11 November an armistice with Germany was signed in a railroad carriage at Compi&#232;gne. At 11 a.m. on 11 November 1918 &#8212; the eleventh hour of the eleventh day of the eleventh month &#8212; a ceasefire came into effect. Opposing armies on the Western Front began to withdraw from their positions. Canadian George Lawrence Price is traditionally regarded as the last soldier killed in the Great War: he was shot by a German sniper and died at 10:58.</p></div></blockquote>

<p class='separator'>&#10086;</p>

Started actually implementing the MediaWiki markup parser today. Fun work.

Almost immediately I ran into [a strange bug](http://rt.perl.org/rt3/Ticket/Display.html?id=60482) in Rakudo. Apparently, strings lose their Rakudohood when passed through PGE — for example by a call to `.split`.

I should do this more often. Write actual code, I mean. Even when I run into bugs, it feels like I'm accomplishing something.

Will be back tomorrow, hopefully with an adequate workaround.


