---
title: GSoC, contextuals, and intolerance (three posts in one)
author: Carl Mäsak
created: 2010-04-29T17:01:00+02:00
---
This is three thirds of a post. The three parts will simply have to combine into one big [mechbot](http://pbfcomics.com/?cid=PBF186-Guntron_Alliance_Force.jpg) in order to pass as a whole post.

## I'm a GSoC 2010 student

I [sent in a proposal](http://socghop.appspot.com/gsoc/student_project/show/google/gsoc2010/tpf/t127230763829), and it got accepted. Yay!

## Use contextuals for "process data", not attributes

Sometimes you both want do subdivide methods into small manageable parts, all the while keeping some sort of data between them. Most people reach for the instance attributes to do this, since these shared between all the methods of one object.

But this can also be done with contextuals, as is now [done in Yapsi](http://github.com/masak/yapsi/commit/5fa4545f571c883ba1c1058e50c478301a3aa462). This keeps the lifetime of the variables as short as possible, and keeps the objects light-weight. And it still works with inheritance.

I like how Perl 6 allows me to mix OO and contextuals this way. And I really like that it works in Rakudo already.

## The mechanics of intolerance

I've blogged before about how #perl6 is [a small, friendly community](http://strangelyconsistent.org/blog/barefoot)which turns even flame wars into [interesting discussions](http://strangelyconsistent.org/blog/how-perl-6-just-sells-itself) and which works on being [kind and open](http://strangelyconsistent.org/blog/how-can-we-scale-kindness). We've had a bit of troll activity since I wrote those posts, as well as a few borderline trolls.

The other day I treated someone with undue impatience. The whole episode ended well, and I've apologised. In short, what looked like nagging questioning may simply have been someone's slightly different means of communicating.

Mean and evil acts are often committed by people convinced that they're actually doing good. mst++ writes about the [reasons for being harsh](http://www.shadowcat.co.uk/blog/matt-s-trout/on-being-a-bastard/), and I thought this was one of those times when that was called for. It wasn't.

It taught me to look out for situations when I might be mistaking variations in personality and communication skills for truly annoying behaviour. In many ways, that's a refreshing reminder: a really open community requires that we recognize that everyone doesn't operate exactly the same as we do. That sounds terribly clichéd, I know; but forgetting it is also far too easy.


