---
title: Blast from the past: E02
author: Carl Mäsak
created: 2010-02-04T16:47:00+01:00
---
SF took [my challenge](http://irclog.perlgeek.de/perl6/2010-02-03#i_1954377) to heart and started producing a "modern Perl 6" version of the example code in [E02](http://dev.perl.org/perl6/doc/design/exe/E02.html). His thought process can be seen [here](http://lastofthecarelessmen.blogspot.com/2010/02/binary-tree.html), and [here](http://lastofthecarelessmen.blogspot.com/2010/02/binary-tree-almost-complete-script.html).

After being a bystander for a few hours, I coulndn't restrain myself anymore: I produced [my own version](http://gist.github.com/294621). I should say at once that it's quite different from SF's: while he keeps close to the original [E02](http://dev.perl.org/perl6/doc/design/exe/E02.html) (which, in turn, sets out to prove that Perl 6 is/was not very different from Perl 5), my version is a bit more liberal in its interpretation. I do mix in some of my personal preferences into it. Some examples:

- We don't do `$ARGS prompts("Search? ")` anymore, but there's a nice `&prompt` function which I used instead, together with a `while` loop.
- The `&show` function now uses `gather/take`, rather than printing directly.
- Also, I avoided the statement-modifying mess from the original E02 `&show` code.
- Also, E02's `&show` makes a point of using a slurpy `@_` rather than naming the parameters. I don't. (Neither does SF.)
- It just makes sense to use a `given/when` construct in the `&insert` function. To its credit, E02 tantalizingly hints of it, but then does a MIB mind-wipe. (You don't recall that bit? Oh well...)
- In the same function, E02 puts `undef` to initiate the child nodes to some empty value. Both SF and I independently realized that just any undefined value won't work if `&insert` is to have `%tree` in the signature, because `%tree` only binds to an `Associative` value. SF solved it by putting `Hash` (an undefined `Hash` type object) in the child nodes, and changed it to `Hash.new` in the later version. I used `{}`, which should be equivalent to `Hash.new`, but IMHO more idiomatic.
- The whole traits business hadn't solidified in 2002, but I believe that the end result is both more realiable, more useful, and prettier. You'll have to judge for yourself.

I believe rewriting the exigeses in modern form is a very worthy activity. I hope we'll see more of that. Perl 6 suffers a bit from stale, outdated documentation, and having these in new versions would be valuable.

It's also a very interesting *historical* activity to read the old apocalypses and exigeses, as I increasingly find. Perl 6 has come a long, long way since 2001.


