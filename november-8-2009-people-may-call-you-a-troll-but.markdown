---
title: November 8 2009 &#8212; people may call you a troll, but...
author: Carl Mäsak
created: 2009-11-08T21:39:00+01:00
---
489 years ago today, the [Stockholm Bloodbath](http://en.wikipedia.org/wiki/Stockholm_Bloodbath), an invasion of Sweden by Danish forces, had its quiet beginnings:

<div class='quote'><p>At dusk (November 8), Danish soldiers, with lanterns and torches, broke into the great hall and carried off several people. Later in the evening, the remainder of the king's guests were imprisoned. All these people had previously been marked down on Archbishop Trolle's proscription list.</p></div>

Nowadays, Trolle is known by Swedish children as [a traffic-teaching troll](http://www.last.fm/music/Trolles+Trafikskola).

<p class='separator'>&#10086;</p>

Here's the scene that I left the reader with [yesterday](http://strangelyconsistent.org/blog/november-7-2009-hasten-the-process-of-debranchification).

    $ ./proto install november
    Downloading november...downloaded
    Downloading html-template...downloaded
    Building html-template...configure failed, see /Users/masak/gwork/proto/cache/html-template/make.log
    Building november...configure failed, see /Users/masak/gwork/proto/cache/november/make.log
    Installing html-template...Method 'protected-files' not found for invocant of class 'Ecosystem'
    in Main (file src/gen_setting.pm, line 324)


So, checking on that first problem, of the two projects' builds failing. Opening a make.log file, I see

    Couldn't find perl6.pbc file in /Users/masak/gwork/rakudo/parrot_install/lib/1.7.0-devel/languages/perl6/lib, have you compiled?

Yes, I have compiled. But just to make sure, I remove the `rakudo` directory and compile again.

Nope, same error. Time to find out why.

Hm, an even stranger thing: the command that fails, `perl Makefile.PL`, has nothing to do with Perl 6. When I run it, I get

    Please set $PARROT_DIR (see README).

Which is an error I understand, and can probably fix.

But the mystery right now is, why am I getting a different error through proto, and where is it coming from?

Exhaustion prevents me from properly addressing that question. Will try again soon.


