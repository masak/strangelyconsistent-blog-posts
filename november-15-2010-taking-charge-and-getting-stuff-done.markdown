---
title: November 15 2010 &#8212; taking charge and getting stuff done
author: Carl Mäsak
created: 2010-11-16T00:48:00+01:00
---
102 years ago today, [Empress Dowager Cixi (慈禧太后)](http://en.wikipedia.org/wiki/Empress_Dowager_Cixi) passed away. She had ruled China for 47 years from "behind the curtains", going against imperial tradition forbidding women to engage in politics.

<div class="quote">Selected by the Xianfeng Emperor as a concubine in her adolescence, she climbed the ranks of Xianfeng's harem and gave birth to a son who became the Tongzhi Emperor upon Xianfeng's death. Cixi ousted a group of regents appointed by the late emperor and assumed regency over her young son with the Empress Dowager Ci'an. Cixi then consolidated control and established near-absolute rule over the dynasty.</div>

She has a generally bad reputation in the scrolls of history.

<div class="quote">Historians from both <a href='http://en.wikipedia.org/wiki/Kuomintang'>Kuomintang</a> and <a href='http://en.wikipedia.org/wiki/Chinese_Communist_Party'>Communist</a> backgrounds have generally portrayed her as a despot and villain responsible for the fall of the <a href='http://en.wikipedia.org/wiki/Qing_Dynasty'>Qing Dynasty</a>, but in recent years other historians have suggested that she was a scapegoat for problems beyond her control, a leader no more ruthless than others, and even an effective if reluctant reformer in the last years of her life.</div>

Nevertheless, her life is full of plotting and intrigue, something which no doubt tickles the mind of the historically curious. During her nephew [Zaitian (載湉)](http://en.wikipedia.org/wiki/Guangxu_Emperor)'s rule, Cixi had him put in house arrest for nine years. He died, 37 years old, on the day before Cixi passed away, apparently from arsenic poisoning.

<p class='separator'>&#10086;</p>

To my surprise, the thing I meant to do today (converting November to work on Rakudo `ng`) had already been started. By me, no less. I had forgotten that.

Hm. Seems I did some initial work on it [in relation to this post](http://strangelyconsistent.org/blog/step-1-on-the-road-to-recovery-admitting-you-have-a-problem), and then [continued that work](http://irclog.perlgeek.de/perl6/2010-04-09#i_2212685) a few days later.

So where did that put me? I run `make` in the `ng-compat` branch of November to find out.

Ah. [#73912](http://rt.perl.org/rt3/Ticket/Display.html?id=73912) bites, and we get this bogus error message:

    ===SORRY!===
    Illegal redeclaration of symbol 'November'

Luckily, at some point pmichaud++ told me how to deal with this. (Can't find it in the backlog, but I remember him telling me.) The trick is to [stub the class](https://github.com/viklund/november/commit/d5b14b3c3ea9771e5160ef977c226f523ad81631) before you start pulling in any modules that use that namespace:

    class November { ... }

That worked! Now November gets as far as pulling in `HTML::Template` which, amazingly, still compiles. These Makefiles are old enough that I need to manage `PERL6LIB` by hand &mdash; how barbaric.

Oh, and it turned out I needed to [update the script `wiki`](https://github.com/viklund/november/commit/590d5a74a3f0381d0449dc7eef221251e18488be), responsible for actually running the November application. It had bitrotted from previous changes in module naming done in that branch.

Running `perl6 wiki`, this is what I get now:

    Cannot modify readonly value
      in '&infix:<=>' at line 1
      in 'November::view_page' at line 53:lib/November.pm
      in <anon> at line 37:lib/November.pm
      in 'Dispatcher::Rule::apply' at line 44:lib/Dispatcher/Rule.pm
      in 'Dispatcher::dispatch' at line 37:lib/Dispatcher.pm
      in 'November::handle_request' at line 49:lib/November.pm
      in main program body at line 17:wiki

Probably Rakudo has become a bit more stringent with readonly things since alpha. This will provide a good starting point for tomorrow's investigations.
