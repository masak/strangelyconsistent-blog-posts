---
title: Week 5 of Web.pm &#8212; more tinkering everywhere
author: Carl Mäsak
created: 2009-04-25T23:30:00+02:00
---
<dl>
<dd> <i>U iz so cute!</i> </dd>
<dd> <i>U iz so hawt!</i> </dd>
<dd> <i>Ur eyez iz liek birdz,</i> </dd>
<dd> <i>an ur hair iz liek goatz</i> </dd>
<dd> <i>comin down teh mountin. Datz hawt.</i> </dd>
<dd>&#160;</dd>
<dd> <i> Ur teefz iz liek sheep</i> </dd>
<dd> <i>dat just hadded a baff.</i> </dd>
<dd> <i>Liek twin sheep,</i> </dd>
<dd> <i>bcz non of ur teefz iz missin.</i> </dd>
<dd>&#160;&#160;&#160;&#160;&#160;&#160;&#8212; Song of Solomon 4:1-2</dd>
</dl>

This week, inspired by jnthn, I'll switch summary style and go point-by-point instead of in free discourse mode. Just to try it out.

I would like you to note, reading this, how many different names of people appear among the different items. This is definitely no longer a three-man effort, but significant parts of the Perl 6 community pushing towards a point where we can build awesome (or at the very least slightly above average) web apps on top of Rakudo and Parrot.

- Renamed the Lobster into the Nibbler, like I said I would. mpeters++ for the ASCII image.
- mberends++ insisted that we spend the hackathon putting bacek++ and Tene++'s socket work into Rakudo itself, continuing on cosimo++'s work. This, for various reasons, took an evening and a day, but it was very much worth it. Seeing the Nibbler being served fairly rapidly was a significant reward.
- One thing that had to be fixed before we could get sockets to work was that non-Rakudo objects couldn't be passed as return values any more. (There was some type checking going on which assumed that the object in question was a Rakudo object.) Passing non-Rakudo objects is vital in the long term, when we expect Parrot to be a platform for many languages communicating freely with each other. jnthn++ and pmichaud++ fixed.
- Next slight problem that turned up was that `IO::Socket::INET` turned out to be the first setting module with those double colons in it, and the underlying setting magic choked a bit on them. jnthn++ showed me how and where to fix.
- Wanting to port November over to the Request/Response model of Web.pm, I took a look at November for the first time in a while. Turns out it has bitrotted somewhat, so I had to take care of that.
- Bitrot issue #1: `join` no longer accepted only one argument. This was a new restriction accidentally added to Rakudo. I reverted the change; moritz++ added the appropriate spectest.
- Bitrot issue #2: In order to prevent autothreading, one of the loops in `Dispatcher.pm` was spelled `for @rules -> Object @pattern, $action { ... }`. This had stopped working in Rakudo due to a combination of stricter type matching in signatures and a failure to handle this particular case — the case where an `Array` is typed as containing `Object` elements. Fortunately, a workaround turned out to be simply removing the typing for now.
- Still didn't get started on porting November over. I've created the branch "web" locally, and now that the tests pass, I can start moving things over. Will probably be a while before we merge this one back into master. But being rid of `CGI.pm`, if only in a branch, will be nice.
- As a surprise bonus, azawawi++ came to me on #perl6 with a number of STD.pm parsefails that he discovered while parsing November in Padre. Hm, maybe it's time for a Perl 6 module that tests Perl 6 projects against STD.pm?
- After discussing the Genshi clone with Tene on #november-wiki, I started writing a simple prototype with a grammar that would pick apart an XML string and put it together depending on the values of `pe:` attributes. I failed, and spectacularly. On the plus side, we discovered quite a number of `.perl` bugs in bleeding Rakudo.
- Even though the prototype never did what it was supposed to demonstrate, it did give me direction to start writing some tests and a small module. This module doesn't yet do what the prototype almost did.
- Working name for the Genshi clone: Hitomi. I've understood this means "doubly beautiful". That is, once for being based on Genshi, and once for being written in Perl 6. ☺

I wish to thank The Perl Foundation for sponsoring the Web.pm effort.


