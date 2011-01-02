---
title: Weeks 8..12 of GSoC work on Buf &#8212; not packing it in yet
author: Carl Mäsak
created: 2010-08-14T00:35:00+02:00
---
*I was reproached by my colleague because of the lack of "no cake"-style jokes in this last grant update. So what can I do to amend the situation? Firstly, let me concede that the below blog post is almost tear-inducingly boring. Secondly, let me remind you that when accosted by boring material such as that in this post, the most important thing to have is a positive outlook on life. Thank you.*

The past couple of days have been really eventful. The coming couple of days probably will be, too. It seems fitting to punctuate all this eventfulness with a status-updating blog post, something that I apparently haven't gotten around to in a few weeks.

So, what's been happening?

- In the [last update](http://strangelyconsistent.org/blog/weeks-6-and-7-of-gsoc-work-on-buf-roundtrip), I was at a point where I needed to encode to different encodings, not just UTF-8. Tonight I had [an enlightening discussion](http://irclog.perlgeek.de/parrot/2010-08-13#i_2701004) with the Parrot people, which gave all the pieces of the puzzle. Now it's just a Simple Matter of Programming. More precisely, I need to apply it to [this spot](http://github.com/rakudo/rakudo/blob/0839993ed01c816dc8b3459fa7b79608be4fbf3a/src/core/Str.pm#L17) in the code base.
- By that discussion, I was once again made aware that Parrot differs between *encodings* and *charsets*. For example, `utf8` is an encoding, but `iso-8859-1` is a charset. It's confusing me slightly, but that's OK. I can make the code work so that both are treated as "encodings" on the Perl 6 level.
- I got unexpected assistance from oha++, who took me aside in private discussions to discuss new wild ideas for `Buf`. It all culminated in a [p6l thread](http://www.mail-archive.com/perl6-language@perl.org/msg32689.html). Oha is writing code that uses Buf for network traffic, and has been preparing a patch for making `IO::Socket::INET` return `Buf`s instead of `Str`s. In trying out this patch, oha++ found that some tests [will never work](http://irclog.perlgeek.de/perl6/2010-08-12#i_2694900) when `Buf`s are used. Those tests will probably need to be rewritten.
- During the past week or so, I've been implementing `pack` and `unpack` in a branch. I'm making steady progress, and trying to take in documentation such as Perl 5's [ `perldoc -f pack` ](http://perldoc.perl.org/functions/pack.html) and [ `perlpacktut` ](http://perldoc.perl.org/perlpacktut.html) and [Erlang's](http://www.erlang.org/doc/man/ei.html) [ `ei` ](http://www.trapexit.org/How_to_use_ei_to_marshal_binary_terms_in_port_programs) and [Tcl's `binary` ](http://www.tcl.tk/man/tcl8.5/TclCmd/binary.htm). It's a lot to take in, and I won't aim for full functionality — look, [Perl 5 has 14699 tests](http://github.com/mirrors/perl/blob/blead/t/op/pack.t) for pack! — but rather a reasonable subset of the directives from Perl 5.

There's a lot to be said about the interplay between Perl 6's high level of abstraction and the almost reckless to-the-metal feeling of `&pack`/`&unpack`. Add to this that the "metal" in this case is Parrot, which in some regards is there to abstract away the real metal. I think the details of this interplay might well be the subject of a separate blog post. When I'm not busy finishing up the code itself. 哈哈

The hard pencils-down deadline for GSoC is on Monday. I'm pretty sure I will have tied up the remaining loose ends by then, but I also foresee a couple of fairly focused hours programming before that. Time to dig back in; see you on the other side.


