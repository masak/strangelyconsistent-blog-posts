---
title: November 11 2009 &#8212; nobody said it was going to be easy
author: Carl Mäsak
created: 2009-11-12T00:34:00+01:00
---
129 years ago today, an Australian outlaw, was [hanged for murder](http://en.wikipedia.org/wiki/Ned_Kelly).

<div class='quote'><p>Kelly was born in Victoria to an Irish convict father, and as a young man he clashed with the police. Following an incident at his home in 1878, police parties searched for him in the bush. After he murdered three policemen, the colony proclaimed Kelly and his gang wanted outlaws. A final violent confrontation with police took place at Glenrowan. Kelly, dressed in home-made plate metal armour and helmet, was captured and sent to jail. He was hanged for murder at Old Melbourne Gaol in 1880. His daring and notoriety made him an iconic figure in Australian history, folk lore, literature, art and film.</p></div>

He died aged 25 or 26 years old.

<p class='separator'>&#10086;</p>

I fixed a bug today. Without knowing what caused it.

A week ago, I tried to fix the [login problem](http://strangelyconsistent.org/blog/november-4-2009-no-log-in-for-you), but I only ran into a gigantic question mark.

Today I donned a lance, jumped on a horse and assaulted the question mark. It stood firm. In fact, it seemed to grow bigger and more scary.

Finally, [an improbable suggestion](http://irclog.perlgeek.de/perl6/2009-11-11#i_1717952) by jnthn [solved](http://irclog.perlgeek.de/perl6/2009-11-11#i_1717992) the whole issue. But didn't quite explain it.

    <masak> it seems the bug is only triggered by something occurring within November.
    <masak> the strings containing user name and password look as they should, but they produce the wrong digest.
    <masak> this could presumably be because they are in a strange encoding [on the Parrot level].
    <masak> I'm not sure of this. it could be some other non-obvious difference.
    <masak> anyway, re-building the strings with join '', split '', $string did the trick.


[Here's](http://gist.github.com/232360) the smallest version of the bug that I've managed to produce so far.


