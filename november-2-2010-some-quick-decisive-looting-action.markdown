---
title: November 2 2010 &#8212; some quick, decisive looting action
author: Carl Mäsak
created: 2010-11-02T23:01:00+01:00
---
248 years ago today, the [Rt. Hon. Dawsonne Drake](http://en.wikipedia.org/wiki/Dawsonne_Drake) became governor of Manila in the Philippines. Ruling for 20 years, he was the only British governor of Manila during the British occupation of the [Seven Years' War](http://en.wikipedia.org/wiki/Seven_Years'_War).

At this point, I realized I knew far too little about the Seven Years' War, so I went and read a bit about that too. Take-away points: it was so big that Winston Churchill called it the [first "world war"](http://en.wikipedia.org/wiki/Seven_Years'_War#Nomenclature). Also, [the Last of The Mohicans](http://en.wikipedia.org/wiki/The_Last_of_the_Mohicans) takes place during this war.

Anyway, governor Drake. Wikipedia writes about his inglorious return from Manila:

<div class="quote">Upon his return to India in 1764, he was tried by the Madras Council on charges filed by his enemies such as bribery, misappropriation of public funds, and violation of orders from the Company. He was found guilty and was sentenced to be dismissed and shipped back to England. The directors of East India Company at London, however, in consideration of his previous services, modified the sentence by simply demoting his rank.</div>

I hope they did the thing where they rip the medals off his chest.

<div class="quote">Drake continued to serve as member of the Madras Council until his death in 1784. He left a large fortune, including some valuable Spanish paintings which were part of the loot of Manila.</div>

Ok, so he got to keep his job *and* the loot? Sounds almost like the CEOs of today...

<p class='separator'>&#10086;</p>

I decided to start simple today, and see if `pls` still passes its tests. It didn't.

Turns out Rakudo has [bitrotted](http://rt.perl.org/rt3/Ticket/Display.html?id=78510), and moritz++ had [fixed](http://github.com/moritz/json/commit/a1351a41f1a0245e13912c61f061f47eeb27044d) the `json` repo. `pls` carries its own copy of the exact same module files (for bootstrapping purposes), and so I had to apply moritz' commit to `pls` as well.

A nice thing was that git allowed me to `git format-patch` moritz' commit and then `git am` it to the `pls` branch of `proto`. It wasn't a big commit at all, but it's nice to know that that works (as long as the directory structures match).

Anyway, now `pls` passes all tests again. \o/

I'll stop here for today, but I'll note what the next step for `pls` is: the tests need a slight refactoring, because they currently pretend that a non-fetched project knows about its dependencies. That's not wrong, because the dependency metadata (contained in a `deps.proto` file, for example) is fetched along with the rest of a project, so we can't know dependences until after fetching. [This test file](http://github.com/masak/proto/blob/pls/t/subcommands/fetch.t) is So Full Of Wrong for that reason. Last time I tried to fix it though, I didn't manage to mend things after I'd broken them.

Maybe next time I'll be more lucky. We'll see!
