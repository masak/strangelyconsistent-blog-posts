---
title: Lovely patchers
author: Carl Mäsak
created: 2008-09-03T19:44:00+02:00
---
The past week has been a flurry of patches. moritz++ contributed the code to what will eventually become the new `HTML::Template`, with a corresponding test suite. Development is stuck in a branch for the time being, blocking on [#58392](http://rt.perl.org/rt3/Ticket/Display.html?id=58392) which causes many tests to fail. (Dear parrot folks: help appreciated. Incorrect call stack semantics is one of the few things that are hard to temporarily work around.)

moritz++ continued by contributing his own `Text::Escape` module, which will replace the simplistic solution used right now, as soon as we figure out why [it crashes when loaded](http://irclog.perlgeek.de/perl6/2008-08-30#i_451399).

Later in the week, Илья Беликин (ihrd++) of [Vladivostok.pm](http://vladivostok.pm.org/) joined forces with us, sending several patches for the `CGI` module. Илья asked many sensible questions, and I reached the realization that there's too little readily available information out there for people who want to hack. The rest of this post will try to remedy that.

(But first, a short commercial break. If you find yourself in the vicinity of Vladivostok on 26 September, consider joining [FEPW 2008](http://event.perlrussia.ru/fe2008/) where I hear they might even do some November hacking.)

## 5 things you might be helped by if you plan to hack on November

1. First, feel free to **ignore these rules** if they inconvenience you. They are there to help you, not to restrict you.
2. The three files **`FEATURES`, `JANITORS`** and **`LOOKINTO`** together constitute our roadmap right now. Note that these files are found in `p5w/`, the Perl 5 implementation of the wiki. **[Update 2008-09-21: Now they're found in `docs/` instead.]** 
3. As a rule, we implement feature-size things in `p5w` before we try them out on p6w. This is because bugs easily distract from the goal in `p6w`. The Perl 5 version is a sort of **live spec** for the Perl 6 version.
4. We're currently doing quite a bit of our work in **branches**. Currently, there are two branches: `new-html-template` aims to replace the `HTML::Template` with a new grammar-using module, and `tests` is a playground for new test files for other modules. (Things like branches change quickly. [github](http://github.com/viklund/november/) is the ultimate reference.)
5. Patches are very welcome, nowadays through the **mailing list** [november-wiki@googlegroups.com](mailto:november-wiki@googlegroups.com), or the **IRC channel** `#november` over at `irc.freenode.org`.

Enjoy!


