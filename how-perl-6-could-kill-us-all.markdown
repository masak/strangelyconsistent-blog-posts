---
title: How Perl 6 could kill us all!
author: Carl Mäsak
created: 2009-11-18T00:26:00+01:00
---
People's minds weave stories. Stories weave people's minds.

My mind is pretty tied to the Perl 6 story nowadays, and I haven't minded thePerl 5 story much. Until this week, that is. Last Sunday, I backlogged over a discussion of someone coming in from #perl asking people on #perl6 why Perl 6 claimed to be Perl [when it clearly isn't](http://strangelyconsistent.org/blog/the-perl-6-is-not-perl-meme). My resulting blog post basically asked the #perl people to stop telling outsiders biased things.

A few days later, mst++ [grabbed me on IRC](http://strangelyconsistent.org/blog/november-12-2009-some-serious-history-awareness) and informed me in what ways the blog post I'd written was biased by *my* views. In particular, these two statements from my post are less-than-objective:

<div class='quote'><p>Surely no-one in the Perl community wants to confuse newcomers. Yet
that is clearly what happened here.</p><p>Whatever replies emma got, she did not get the balanced views that the
#perl6 folks gave.</p></div>

As mst pointed out, the post comes from the assumption that *my* views are balanced. It does — I bet there's a psychology term for that kind of phenomenon, where people [unconsciously assume that their own assumptions are correct](http://en.wikipedia.org/wiki/Bias_blind_spot).

A more useful view to take is perhaps that there's a Perl 5 story and a Perl 6 story. Had emma got the Perl 6 story from #perl6 before she got the Perl 5 story from #perl, she probably wouldn't have become annoyed at the people on #perl6 the way she did. But that doesn't mean that the #perl people were spreading untruths or badmouthing Perl 6. It could be as simple as emma asking "Is Perl 6 different from Perl?", and getting a "yes". (Well, it is different.) According to mst, that's pretty much what happened.

Then I learned a bunch of interesting things about the Perl 5 story, things which I would probably have figured out by myself sooner, had I not been so influenced by the Perl 6 story already. Here I make a partly vain attempt to summarize the Perl 5 take on Perl 6:

<div class='quote'><ul>

<li>There's Perl 5, and it's pretty awesome. It's fast, well-tested, and has the
  CPAN, which rocks. People who weave the Perl 5 story are generally not
  sitting around waiting for Perl 6, nor are they particularly interested in
  making it arrive sooner.</li><li>From the outside, and generalizing, Perl 6 looks like a big failure. It
  hasn't "arrived" yet, and in the process it has generated massive amounts of
  bad PR for itself and for Perl in general. Parts of the bad PR come from not
  arriving yet, parts of it come from the version thing.</li><li>The version thing, in essence, is this: 6 is larger than
  5. <em>Everyone</em> knows what this means; releases with bigger version
  numbers (especially major ones) are better than, and are meant to replace,
  releases with smaller version numbers. Perl 5 replaced Perl 4, which
  replaced Perl 3, etc. Perl 6 lambdacamels don't think much about this, but
  Perl 5 folks fielding questions from outsiders about Perl 6 sure do.</li><li>To make matters worse, those Perl 6 people are actually <em>encouraging</em>
  the delusion that Perl 6 is replacing Perl 5. They have early alpha releases
  out, and they're making statements about Perl 6 being the "next version of
  Perl", as if they were standing there with the finished product already.</li><li>When people try what the Perl 6 people have, the find it slow, buggy, and in
  almost total lack of libraries. If these people are outsiders, they might well
  associate those bad things with "Perl", and never get to trying out Perl 5,
  the "real" Perl.</li></ul></div>

After putting the pieces together, I understand a little better where the, um, strong dislike may be coming from.

But it's worse than that. This is a conflict that, assuming us Perl 6 people are really onto something, will only get worse. Perl 6 was conceived at a time when Perl 5 was at a slump, but Perl 5 is not at a slump anymore. It's [alive and kicking, and stronger than ever](http://mdk.per.ly/2009/08/06/perl-is-alive-kicking-and-stronger-than-ever/). Large parts of its community do *not* consider Perl 6 the hot next version. They consider Perl 6 a threat, more or less. If and when Perl 6 takes off, it will be a bigger threat. A threat made worse by the Perl 6 story, as it was once woven: that Perl 6 is the obvious successor to Perl 5.

I think it's time to try and mitigate the conflict now. I understand the best we can do from the perspective of the Perl 5 story is to change the name "Perl 6" to something else. I don't see that happening, and [neither does Larry](http://irclog.perlgeek.de/perl6/2009-11-13#i_1725354). But what we *can* change, and right away, is the dissipation of the idea that Perl 6 is the next major version of Perl. We should be focusing on consistently telling another story; something like this:

Perl 5 and Perl 6 are two languages **in the Perl family, but of different lineages.**

It won't entirely remove the damage that the "6 > 5" phenomenon causes the Perl 5 crowd, but it will at least mitigate it. Also it shows some serious good will from the Perl 6 side.

What we do *not* want is a conflict, where the Perl 5 community feels the need to assert itself against the impostor Perl 6, which threatens it simply by being Perl 6. "Rewrite" does not mean "overwrite". Perl 5 isn't going away anytime soon. This is actually something we say on the channel a lot, but it hasn't been evident in our PR.

The Perl 6 community is taking steps to rectify that. Where the [perl6.org](http://perl6.org/) page used to say "Perl 6, the next major version of the Perl programming language", it now says "Perl 6, the spunky little sister of Perl 5. Like her world-famous big sister, Perl 6 intends to carry forward the high ideals of the Perl community." With this re-wording, things are suddenly much more on equal grounds.

While I'm posting this, [mst++ has made a blog post](http://www.shadowcat.co.uk/blog/matt-s-trout/f_ck-perl-6/) in his inimitable style over at his blog, where he defends the Perl 6 story to his peers. I've read it, and while it does contain the usual amount of four-letter words, it also makes points very similar to this post's, but in a sort of mirror-like Perl 5 universe. Understanding needs to flow in from both sides in this matter. [You should read it, too.](http://www.shadowcat.co.uk/blog/matt-s-trout/f_ck-perl-6/)

In an ideal world, we will have Perl 5 and a mature Perl 6 existing side by side, not feeling threatened by each other, and competing on cool tech. The Perl 5 and Perl 6 communities could recognize each other's Perl version as Perl, go to the same conferences (as we do already), and feel confident about the other group's assertions of superiority, because they're so clearly misguided, while still perfectly fine individuals, of course.

Let's try to steer in that direction, and not towards mutual assured destruction. That's just mad.


