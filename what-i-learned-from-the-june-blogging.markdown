---
title: What I learned from the June blogging
author: Carl Mäsak
created: 2011-07-25T22:47:37+02:00
---
So, I confess: I had fun. Pushing out a blog post a day for a month sounds like a lot of work, but it was mostly enjoyable.

I've been wanting to try this kind of thing for a year or so. Now that it's behind me, it feels like the material produced begs for being put into a form other than just blog posts. Therefore, I've [created a repository](http://github.com/masak/adventure-game-book) for the chapters. From now on, changes/improvements will go into those, and the blog posts are essentially frozen. I expect to have a PDF of all the chapters soonish.

Having done three November blogging months, I guess I didn't expect to learn much new. But this kind of setup was sufficiently different, and I ended up with a few take-aways.

## What I learned from blogging once a day

* **It's easy to fall off.** More exactly, I have a full-time job now, thanks to Perl 6. I thought that, if anything, would be what would prevent me from doing this. But no, it was essentially going to a conference and do the usual last-minute dance with slide preparations. D'oh! Having fallen off, it was a bit hard to get back on again.

* **It's fun.** My regular hobby hacking more or less got put on hold, but every day I feel like I've accomplished something. It's great. There was never any real lack of things to write about; making up the topics ahead of time took more thinking than actually writing the posts.

* **I should have had a blog commenting system when doing a blogging month.** Been wanting to build one, but `ENOTUITS`. [Disqus](http://disqus.com/) looks great, and people have been pointing me to it, but I think I'd prefer in the long run to build something myself. Yeah, that means it's going to take a bit longer, I know. As it was, I got a lot of good feedback from `#perl6` (as usual). But not everyone hangs out on `#perl6`.

## What I learned from trying to bootstrap Perl 6 knowledge

* **Things can be bootstrapped fairly easily.** This surprised me. There's at least one fairly linear order for concepts to be introduced.

* **I need to go back and re-do everything.** For most games, but *especially* the last one, I found myself using concepts that I *should* have introduced in some earlier chapter, but didn't. Of course I should've mentioned bareword keys in the post about hashes. It's evident in retrospect that I should've spent more time on public accessors and constructors in the chapter about classes. And probably `.can` and the `.?method` syntax too. While the themes for each day seem to have been mostly well-chosen, there's just so much that I failed to mention.

* **It's easier to explain a concept than to explain why you wrote a game the way you did.** Or rather, it got more difficult to adequately explain the games the bigger they got. It's like small games contain all these interesting little decisions, and that's fine to explain. But then as they grow bigger they also start to contain all these overarching, architectural decisions. And there just isn't enough blog space and reader attention to focus both on the big and the small issues, so I increasingly focused on the big ones. But the little decisions are still there, even in the bigger games.

## What I learned from writing a text adventure game

* **It was just as enjoyable as I hoped it would be.** I had been looking forward to this.

* **It is a bigger project and takes more time than one might think.** How hard can it be, right? Just four, no five, no six rooms. Be done in a jiffy.

* **I wish I had written tests for it.** Not sure how it would have affected the code, but the game *definitely* is complex enough to merit tests. Henceforth, my answer to the question "Shall I write tests?" will be "Is it more than a screenful of code?".

Fun as it was, I can't think of a followup for this project for next June. Let's just see where we are at that point and decide then. 哈哈
