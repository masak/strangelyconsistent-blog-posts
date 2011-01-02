---
title: The April List: things I think ought to work in Rakudo Star
author: Carl Mäsak
created: 2009-09-08T11:56:00+02:00
---
I took an hour or so to go through the RT bug list of known Rakudo issues, searching for things that I think should be fixed by April. Here they are.

There are currently over 400 new/open bugs in RT. These only form a slice of those. My criteria for slicing have been, more or less, "what would hurt more, fixing this bug, or having to explain to the hordes of new users in Q2 2010 why this particular bug exists?". Whenever the latter weighs heavier, I include the ticket in the list.

(This also shows a different statistic, and quite a positive one: while there are many bugs in RT right now, many of them pertain to corner cases, or minor issues. In other words, Rakudo is getting more usable.)

For those in a hurry, I've marked the most egregious issue in each category in bold.

## Command line

On the command line, Rakudo...

- ([#52242](http://rt.perl.org/rt3/Ticket/Display.html?id=52242)) **thinks that unknown command line options are files** 
- ([#58592](http://rt.perl.org/rt3/Ticket/Display.html?id=58592)) cannot combine the --target and -e command options
- ([#62462](http://rt.perl.org/rt3/Ticket/Display.html?id=62462)) has no -V flag
- ([#68752](http://rt.perl.org/rt3/Ticket/Display.html?id=68752)) has a --version flag, but it's useless

## Parsing

During parsing, Rakudo...

- ([#53804](http://rt.perl.org/rt3/Ticket/Display.html?id=53804)) parsefails on adverbial blocks
- ([#61494](http://rt.perl.org/rt3/Ticket/Display.html?id=61494)) **gives different errors depending on the amount of whitespace between some things** (*Update 2010-03-20:* This particular parsefail is no longe plaguing us.)
- ([#61838](http://rt.perl.org/rt3/Ticket/Display.html?id=61838)) doesn't complain when a variable is used before it's declared
- ([#64170](http://rt.perl.org/rt3/Ticket/Display.html?id=64170)) uses the old 'is also' instead of the new 'augment' (*Update 2010-03-20:* Now uses 'augment'.)
- ([#64526](http://rt.perl.org/rt3/Ticket/Display.html?id=64526)) doesn't complain when a variable is used in the statement where it's declared
- ([#64688](http://rt.perl.org/rt3/Ticket/Display.html?id=64688)) can't import other modules inside class declaration
- ([#65904](http://rt.perl.org/rt3/Ticket/Display.html?id=65904)) dies horribly when declaring a sub with an invocant
- ([#65962](http://rt.perl.org/rt3/Ticket/Display.html?id=65962)) doesn't allow multiple 'my' declarations in loop init (*Update 2010-06-22:* Has been fixed a while now.)
- ([#66034](http://rt.perl.org/rt3/Ticket/Display.html?id=66034)) parses dotty and infix '.=' differently (*Update 2010-05-04:* Parses them the same now.)
- ([#66498](http://rt.perl.org/rt3/Ticket/Display.html?id=66498)) two words: snowman. comet. (*Update 2010-03-20:* No more.)
- ([#66948](http://rt.perl.org/rt3/Ticket/Display.html?id=66948)) should parse reduce metaops as listops
- ([#66996](http://rt.perl.org/rt3/Ticket/Display.html?id=66996)) doesn't parse all pair constructors correctly (*Update 2010-03-20:* Now it does.)
- ([#68086](http://rt.perl.org/rt3/Ticket/Display.html?id=68086)) allows the same positional to be declared multiple times in a param list (*Update 2010-05-04:* Fixed since quite a while back.)
- ([#68918](http://rt.perl.org/rt3/Ticket/Display.html?id=68918)) ignores decimal points in base conversions (*Update 2010-05-04:* Doesn't ignore them anymore.)

## Runtime

At runtime, Rakudo...

- ([#58258](http://rt.perl.org/rt3/Ticket/Display.html?id=58258)) **oddly thinks that each line of interactive code is in an invisible block** 
- ([#58372](http://rt.perl.org/rt3/Ticket/Display.html?id=58372)) dies when indexing undef
- ([#61566](http://rt.perl.org/rt3/Ticket/Display.html?id=61566)) doesn't do type checking on binding
- ([#61732](http://rt.perl.org/rt3/Ticket/Display.html?id=61732)) dies horribly on the return value of empty statements
- ([#63126](http://rt.perl.org/rt3/Ticket/Display.html?id=63126)) treats junctions combined with indexing not-so-good
- ([#63430](http://rt.perl.org/rt3/Ticket/Display.html?id=63430)) can't handle CATCH and &return together
- ([#63912](http://rt.perl.org/rt3/Ticket/Display.html?id=63912)) can't &return a list of values (*Update 2010-05-13:* Now it can.)
- ([#64522](http://rt.perl.org/rt3/Ticket/Display.html?id=64522)) allows constants to be modified (*Update 2010-05-13:* Also fixed now.)
- ([#64888](http://rt.perl.org/rt3/Ticket/Display.html?id=64888)) tries (and fails) to redeclare anonymous classes when same code is re-run (*Update 2010-05-04:* Now tries and succeeds.)
- ([#66890](http://rt.perl.org/rt3/Ticket/Display.html?id=66890)) dies horribly in for loops when args don't match up with list
- ([#67142](http://rt.perl.org/rt3/Ticket/Display.html?id=67142)) ranges aren't immutable (*Update 2010-03-20:* They are now.)
- ([#67234](http://rt.perl.org/rt3/Ticket/Display.html?id=67234)) dies [during [TimToady's keynote](http://videos.sapo.pt/yapc/50dllc2fnsM7phJy24fP)] when an undef is sent to PGE (*Update 2009-10-20:* now fixed)
- ([#67372](http://rt.perl.org/rt3/Ticket/Display.html?id=67372)) doesn't give line numbers with &warn (*Update 2010-06-22:* It does now.)
- ([#68116](http://rt.perl.org/rt3/Ticket/Display.html?id=68116)) doesn't accept subs as &-sigilled parameters (*Update 2010-05-04:*Does now.)

## TODOs

Perhaps the most subjective part of my list. What finally made it on this list are features which are "almost there", and which people will probably ask about if they're not fully implemented.

- ([#60672](http://rt.perl.org/rt3/Ticket/Display.html?id=60672)) %C in sprintf
- ([#62476](http://rt.perl.org/rt3/Ticket/Display.html?id=62476)) **:by on ranges** (*Update 2009-09-22:* :by is now deprecated by recent spec changes)
- ([#63292](http://rt.perl.org/rt3/Ticket/Display.html?id=63292)) triangle reduce metaoperator (*Update 2010-05-04: *We can haz it!)
- ([#65654](http://rt.perl.org/rt3/Ticket/Display.html?id=65654)) texas quotes not yet awesome


