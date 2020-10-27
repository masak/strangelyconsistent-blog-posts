---
title: I made the Bel reader 50 times slower (epoq diary 008)
author: Carl Mäsak
created: 2020-10-27T22:42:44+08:00
---
In the midst of [an intense race to the finishing line](https://github.com/masak/bel) in terms of basic features, I have a very promising setback to report: the other day I managed to slow down the entire test suite _dramatically_.

These days, it hovers around 14 minutes, as measured by Travis CI. I consider that to be way too long, and I'm wary of making anything significantly slower, because Travis has a timeout somewhere slightly north of 20 minutes.

With my recent improvement in a local branch, the test suite instead finishes in approximately 11 hours and 30 minutes. I say "approximately", becuase I've had to also fix some newly discovered bugs which likely interfered a little bit with the measurements of the previous runs.

Phew! Nothing like _half a day_ to make you appreciate those 14 minutes.

Dear reader, I will spend the rest of the blog post explaining why a factor-50 slowdown puts me in a good mood.

## The change before the change

In the exciting rush to get to 353 implemented globals, I recently merged a change that put the entire reader in place. ("Reader" is Lisp terminology for "parser". Lisp has the excuse of being older than most established terminology.) The reader used so far is implemented in Perl, basically a manual port form the Bel specification to Perl.

This puts us at 325 implemented globals (92%). The state of completion chart, which a colleague of mine says looks like a UI for booking seats in a movie theater, is looking more and more like no-one's booked any seats.

But more importantly, this put me in a position to try running the entire project on the new reader instead of the old one.

Rule of thumb: if you have two implementations of the same specification, running both of them for a lot of inputs (such as the test suite) is bound to teach you _something_. I think there is some wording to this effect in "Writing Solid Code".

## Why it's _so sloooow_

There are three reasons for the slowness, in increasing order of importance:

* The Bel reader is operating on cons lists of characters, not (Perl) strings. There's some overhead in doing that.

* Then there's the interpretative overhead; the original reader can run directly in Perl, whereas this new one currently runs in the Bel evaluator, running on top of Perl.

* On top of _that_, just interpreting the Bel reader as it's written is probably dog-slow. I don't have any hard numbers here, but knowing how big a difference manually optimizing away function calls has made among the rest of the globals, I'm betting it's responsible for a significant part of the slowdown here as well.

All of these are fixable. There's actually no reason the Bel version couldn't be as fast, or faster, as the Perl version.

## My new slogan

This will end up being a central topic in my presentation on Bel, written for some virtual Perl conference in the near future: _Performance is the feature_.

I originally phrased it in my mind as "Performance is a part of the feature", but no: _Performance is the feature_. If something takes 11.5 hours to complete, it might as well not run at all. This local branch should be ashamed of itself.

I'm not going to merge it, by the way! If nothing else, Travis CI would give me a distasteful stare that could wilt flowers.

In fact, the status-quo 14 minutes are not nearly fast enough.

Corollary: even when those 353 globals are implemented, I'm not really done.

## What the really slow test suite run found

So, did we shake out any bugs from running it this way? Yes! Two different root causes manifested themselves:

* The current reader plays fast and loose with unterminated lists; the new reader will have nothing of that. Even better &mdash; this change can be lifted straight back to the `master` branch.

* Two tests started failing because they contained the character `¦`, and (evidentaly) the Bel implementation does not yet switch all the Unicode settings up to 11.

## Running Bel in Bel, like Matryoshka dolls

This is a very serious diagram about the three main components of the Bel implementation:

```
                in Perl    in Bel
              +----------+--------+
   Bel reader |   used   | exists |
              +----------+--------+
Bel evaluator |   used   | exists |
              +----------+--------+
  Bel printer |   used   | (todo) |
              +----------+--------+
```

In a not-too-far future, I'm hoping it will look more like this:

```
                in Perl    in Bel
              +----------+--------+
   Bel reader |          |  used  |
              +----------+--------+
Bel evaluator |  (used)  |  used  |
              +----------+--------+
  Bel printer |          |  used  |
              +----------+--------+
```

Think of it as the project becoming more and more self-hosting.

Oh, and that's not due to some "wouldn't it be cool if...?" factor. The _reader_ is extensible, the _evaluator_ is extensible, and the _printer_ is extensible &mdash; at least that's how I read the specification, between the lines. What fun is a self-hosting Lisp if you can't also late-bindedly change absolutely everything under you?

(So one last thing I will do on this very slow branch is to put together a test file where I extend the Bel reader, and then backport it into the `master` branch as an ambitious TODO test file.)

But, as this little experiment shows, speed is an issue. That will need to be addressed unless I want hours-long test suite runs. As much fun as those have been, I really really want to bring the time of the test suite down, and performance up.

(Final exercise for the reader: why couldn't I completely remove the Perl evaluator? What happens if you try to do a recursion without a base case?)

## So, what's the plan for performance?

In the long run, compiling Bel code to a faster representation. Because I would totally kick myself if I didn't, I have decided to call this representation "Belfast".

I have some pretty far-reaching plans. There's an optimizer involved. At this point, it's a Simple Matter Of Programming. It'll be fun.

But in the short run, something that I ended up calling "fastfuncs". Think of them as a manual stopgap that exists while we're all eagerly waiting for the compiler.

A fastfunc is a Perl sub that does the same job as the Bel function, but without all the superfluous call, and without the interpreter overhead. The Bel interpreter knows to go look for a fastfunc (cleverly hidden invisibly "inside" a normal Bel function, in a kind of pocket dimension) and prefer it if it's there. The general contract is that you'll never know you're running a fastfunc; everything behaves the same, but is faster.

But a "funny" thing happened: the calling convention overhead that I set out to eliminate in the manually written Perl functions, instead manifested as massively duplicated code. You inline enough function calls, you end up doing the moral equivalent of Ctrl+C Ctrl+V all over the place.

Since I was doing this for a good cause &mdash; having things run faster by _avoiding_ call overhead &mdash; I bravely (stupidly?) hung in there. What's a little duplication between friends? Eventually the levels of manual inlining, and the accumulated complexity got the better of me, and I couldn't author any more fastfuncs.

I remember my last attempt to write one involved trying to code-generate a single huge sub using a helper Perl script, which itself was several hundred lines long. I don't remember if that attempt succeeded or not, but it did hammer home the obvious fact that I was acting against my own best interest, basically practicing the code equivalent of self-harm. Technical debt is real.

I'm happy to report that I have now deployed a solution to this issue: a barebones macro language for Perl 5. (This probably counts as burying the lede below six feet of dirt. Sorry!) It allows me to write various Bel-specific looping constructs without any extra boilerplate, but more importantly, I can write things like `INLINE(some_other_fastfunc($x, $y))`, and the macro-expander will turn that call into inlined code. Magic!

Eventually, the fastfuncs and this weird and wonderful macro expander will both go away and be replaced by a real Bel compiler pipeline. But there is plenty of functional overlap between the former and the latter, so I think the experience with the macro expander will be good.

Also, of course, it feels super-odd to be adding macros to Perl 5, even with a makeshift solution like this! (Just to be clear, it's not a source filter or anything; I read the source `.pm` file as text, parse it into a custom AST, macro-expand, and print it back out.) On the way, I had to face some hard questions I had already forgotten about from Alma, notably the exact relationship between statements and expressions.

If I had the time and the energy, I could maybe turn this macro expander into a real CPAN module. Probably it would be possible to push these ideas quite far into the Perl compiler pipeline itself. It would be an amusing irony if I ended up giving Perl 5 hygienic macros, more or less by mistake! But... I'm afraid tuits don't really work like that.

## Enough about Perl 5 macros, what about that Belfast format!?

Alright, that's all we had time for today. If there aren't any questions... anyone? No? Ok, thank you for your time.

Enjoy Bel! It's slow right now, and that's not OK, but a lot of other things are either in place, or quickly falling into place.

(A couple of days ago marks the one-year anniversary of trying to implement Bel. It's been a wild ride, and it's not over yet. _Performance is the feature_. Stay tuned.)
