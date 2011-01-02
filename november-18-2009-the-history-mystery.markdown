---
title: November 18 2009 &#8212; the history mystery
author: Carl Mäsak
created: 2009-11-19T00:55:00+01:00
---
146 years ago today, King Christian IX of Denmark signs the so-called [November constitution](http://en.wikipedia.org/wiki/History_of_Schleswig-Holstein#The_November_Constitution) that declares Schleswig to be part of Denmark. As a consequence, Germany attacks to reclaim that area a few months later.

<div class='quote'><p>Denmark's new king, Christian IX, was in a position of extraordinary difficulty. The first sovereign act he was called upon to perform was to sign the new constitution. To sign was to violate the terms of the London Protocol which would probably lead to war. To refuse to sign was to place himself in antagonism to the united sentiment of his Danish subjects, which was the basis of his reign. He chose what seemed the lesser of two evils, and on November 18 signed the constitution.</p></div>

So, wow, it turns out there's this whole [Schleswig-Holstein Question](http://en.wikipedia.org/wiki/Schleswig-Holstein_Question) of which, despite thorough history schooling, I was previously completely unaware. I will spend some time reading up on it, as well as trying to say "Schleswig-Holstein Question" many times in a row, as rapidly as possible.

<p class='separator'>&#10086;</p>

Ok, hm. So today, in order to fix [lichtkindbug 10](http://github.com/viklund/november/issues#issue/10), I added back the [view history](http://github.com/viklund/november/commit/8075afb2390f598a3f8e3ff20c476677766665f0) action into November. I say "added back", because all signs indicate that it had been there before. All the plumbing necessary to do a history action was there, and I only needed to write the method.

(No, I haven't done any source code archeology thing with `git log` and `git blame`. Yes, I probably should.)

However, when I'd done this, and compiled, [it still fails](http://november-wiki.org/history/Perl). And in exactly the same way.

I have a feeling there's something I'm missing here. Do I need to add the new 'history' action somewhere else in the system? I don't think so. The Dispatcher should just use the new action directly.

In other words, what could have been a ridiculously easy addition turned into a mystery instead, whose solution will inevitably have to come another day. Oh well.


