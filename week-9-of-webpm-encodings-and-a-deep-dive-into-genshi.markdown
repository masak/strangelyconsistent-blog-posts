---
title: Week 9 of Web.pm &#8212; encodings, and a deep dive into Genshi
author: Carl Mäsak
created: 2009-06-17T13:50:00+02:00
---
<dl>
<dd> <i>Den all IRS d00dz an bad kittehs coem to hear Jebus. An Fariseez sez "LOL Jebus etz wit bad kittehs! Him sux!'</i> </dd>
<dd> <i>Jebus sez, "WTF? If lolcat had hundrd sheeps an won gits losted, doan him leev naintee nain sheeps an go luk fr losted won? Den him find it an iz liek 'w00t!' An dem him coem hoem, trow partee cuz him finded losted sheep. Srsly! Ceiling Cat moar happi wen a bad kitteh being maed lolcat den bowt naintee nain gud kittehs." </i> &#8212; Luke 15:1-7</dd>
</dl>

I'm working mostly on [Hitomi](http://github.com/masak/web/tree/master/t/hitomi), a Perl 6 port of the Python templating engine [Genshi](http://genshi.edgewall.org/). In the past week, I decided to dive into Genshi, looking at how data flows from the template to the finished result. I now have a pretty good understanding of this, so I thought I'd expand a bit on it here.

Genshi's fundamental data structure is called [Stream](http://genshi.edgewall.org/wiki/Documentation/streams.html) and it looks very much like a sequence of [SAX](http://en.wikipedia.org/wiki/Simple_API_for_XML) events to me: open-tag, close-tag, text, processing-instruction, etc. Different transformations are then applied to a stream to yield the final result. A transformation could be something like "remove all &lt;script&gt; elements" or "shorten all posts that are longer than 400 characters". A stream modified in-place, but combine with a transformation to produce a new stream. The nice thing is that the actual templating is also expressed as a series of this kind of transformations. But the Genshi user can easily provide her own transformation on top of the standard ones.

I like this model very much. It feels extremely clean and extensible. I decided to port as much as I can to Hitomi. My short-term goal is to make things round-trip using the streams, and to that effect, I've ported a test file with [89 tests](http://github.com/masak/web/blob/6127e91c62c1b2ac382627d27ed46972e760415b/t/hitomi/05-input.t) from Genshi to Perl 6.

It's still not totally clear to me how text is converted to a stream and then back. I can easily picture how a stream event knows how to serialize itself back into text, so the mystery on that side isn't very great; it's just that I haven't found the actual Python code for it yet. On the stream generation side, the data flow disappears into a Python-[Expat](http://www.libexpat.org/) library. Delegating XML parsing to a third party also seems like an exceedingly good idea to me.

Can we do the same thing — delegate to Expat, or some other suitable library — in Hitomi? I think so, and the [Parrot documentation](http://www.parrotcode.org/docs/pdd/pdd16_native_call.html) seems hopeful. I'd very much like to get that working. But in the short run, I'm pondering whether it might not be easier to make a small, throwaway XML parser out of the bits and pieces we developed as prototypes. I could make it a separate class and call it `Impostor`, to make sure we remember to remove it later.

Another issue I ran into is one of encoding. viklund++ has been doing heroic work in the past week making November handle UTF-8 correctly. The reason this is heroic is that Rakudo doesn't have a model for string encodings yet. The information has to be forced out of Rakudo against its will, and I've heard viklund mutter darkly about hacks and workarounds lately... It all culminated in [a good discussion](http://irclog.perlgeek.de/perl6/2009-06-16#i_1244954) on #perl6 last night, and pmichaud++ [promised](http://irclog.perlgeek.de/perl6/2009-06-16#i_1245092) to make a preliminary implementation of `.encode` (for `Str`) and `.decode` (for `Buf`), if we just sat down and wrote some tests to show what we expected these to do.

Looking forward a bit; I think there's a good chance we'll have something usable with Hitomi before my original grant period is over. After that, it might be a good idea to start looking at the port of Ruby's [Hpricot](http://wiki.github.com/why/hpricot) (for manipulating and searching HTML documents), and to start digging into the MVC quagmire. I still expect to do some preliminary MVC investigations before that, though.

I wish to thank The Perl Foundation for sponsoring the Web.pm effort.


