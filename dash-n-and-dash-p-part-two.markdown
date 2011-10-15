---
title: -n and -p, part two
author: Carl Mäsak
created: 2011-09-05T21:51:06+02:00
---
*(This blog post is part two of a series; there's also a [part one](http://strangelyconsistent.org/blog/dash-n-and-dash-p) and a [part three](http://strangelyconsistent.org/blog/dash-n-and-dash-p-part-three).)*

I wrote [last time](http://strangelyconsistent.org/blog/dash-n-and-dash-p) about how `-n` and `-p` were implemented in a text-oriented way in Perl 5, and in an AST-oriented way in Rakudo.

Afterwards, TimToady said he thought I was going to write about settings and `{YOU_ARE_HERE}`. You see, the *spec* doesn't talk about toying around with ASTs, it talks about `-n` being equivalent to having a setting that looks something like this:

    for lines() {
        {YOU_ARE_HERE}
    }

(This is completely equivalent to the current AST approach, but with code instead; the `{YOU_ARE_HERE}` gets replaced by your program. In a way, it's a nice full circle back to a text-based way of doing things, but correctly this time. Kissing eskimos still need not apply.)

The whole notion confused me, because by "setting", I generally mean the set of builtins provided by Perl 6. Here it seemed to mean "a layer of code immediately surrounding your program". I [asked](http://irclog.perlgeek.de/perl6/2011-09-03#i_4366219) on the channel. Turns out no-one else knew, either.

There was plenty of good discussion, though. Didn't make me any wiser, but it was at least interesting.

Finally jnthn suggested that maybe Rakudo actually does this right already (with the ASTs) and the spec is wrong. I can agree with that, at least to the extent that I don't see how the current setting/`{YOU_ARE_HERE}` spec is s'posed to work, but I feel pretty comfortable about those AST transformations.

So, um, yeah.
