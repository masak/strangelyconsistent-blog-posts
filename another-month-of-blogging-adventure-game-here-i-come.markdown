---
title: Another month of blogging: adventure game, here I come!
author: Carl Mäsak
created: 2012-07-01T23:06:01+02:00
---
Last summer I spent a month [blogging about basic
programming](http://strangelyconsistent.org/blog/a-month-of-blogging-about-programming-fundamentals),
building stuff up out of small building blocks into bigger and bigger games.
The exercise was modeled on some books I liked when I was a teenager and
curious about programming.

The last game was a text adventure game. It became... big. More ambitious than
I had planned.

It's not that it had that many rooms or was that long. It didn't even
experience feature creep. It was just a fairly big and ambitious game. I
finished it, but more than half a month after I had planned. In retrospect, it
was insane to assume I would be able to write it in the last two days of that
month of blogging. Dedicating a whole month to writing the adventure game would
have been more appropriate.

So, that's what I will do. I will write *exactly the same game* over the scope
of a month. Well, functionally it will be the same. The details of the
implementation will be different. Better.

We will learn a lot together, dear reader. Things that got lost in a sea of
details last year.

What's more, there are a couple of other advantages with doing this all over
again:

* There's a certain amount of artistic suffering in writing text descriptions
  and coming up with a plot and all that. Since I'll be doing the same game, I
  won't have to do that, I can just re-use everything from last year. Might put
  in the odd improvement, of course.

* I [realized last year](http://strangelyconsistent.org/blog/what-i-learned-from-the-june-blogging)
  that I really should have written this game with Test-Driven Development. I
  will this time around. Not only that, you will see the domain-oriented TDD
  that I prefer nowadays, and that really should be more well-known. This
  approach really makes you *understand* object orientation. At least that's
  the effect it has on me.

* In retrospect, I don't think I should have leaned so hard on type
  system. I used roles a lot last year, like `Openable`. And then mixed them
  into classes, like `Door`. It seemed like a good idea at the time, but it
  does make the whole object model a little inflexible and, well, static. This
  year around I'll try something else, and I think it'll come out better and
  more dynamic, while at the same time more light-weight. I think you'll like
  it better.

There will be zillions of spoilers for the actual game. If *you* know a way to
give a detailed public account of how to implement an open-source adventure
game without giving away the whole plot, get in touch.

Until I conclude this blogging month, there will be an ongoing mini-contest
known as "Devastate The Adventure Game". Basically, you download the game and
run it on Rakudo. If at *any* point during its development you manage to get
the game to behave outside of its intended parameters, you're in the contest. I
want to hear about all instances of this, and I'll do my best to log them all
publicly. The most insane, creative, simply up-the-wall fruit-bat bananas
mis-use of the game (decided by me; any number of entries per person) wins an
Amazon book worth about 50 EUR. Crazy stunts may get honorable mentions in
the blog posts, too.

Suddenly this feels pretty exciting. 哈哈 Come on folks, nail the masakbot!

Oh, and I wrote a plan for the whole month. Subject to change, but the basic
structure is hopefully there.

* [2012-07-01](http://strangelyconsistent.org/blog/july-1-2012-hanoi-as-a-black-box): describing the hanoi subgame; methods/events/exceptions
* [2012-07-02](http://strangelyconsistent.org/blog/july-2-2012-implementing-hanoi): implementing the hanoi subgame
* [2012-07-03](http://strangelyconsistent.org/blog/july-3-testing-the-adventure-game-looking-around): testing the adventure game, looking around
* [2012-07-04](http://strangelyconsistent.org/blog/july-4-moving-around-i-compass-directions): moving around I (compass directions)
* [2012-07-05](http://strangelyconsistent.org/blog/july-5-moving-around-ii-up-down-in-out): moving around II (up/down, in/out)
* [2012-07-06](http://strangelyconsistent.org/blog/july-6-room-descriptions-look): room descriptions (look)
* [2012-07-07](http://strangelyconsistent.org/blog/july-7-saving-and-restoring): saving and restoring
* [2012-07-08](http://strangelyconsistent.org/blog/july-8-blocked-exits): blocked exits
* [2012-07-09](http://strangelyconsistent.org/blog/july-9-things-and-descriptions): things and descriptions
* [2012-07-10](http://strangelyconsistent.org/blog/july-10-2012-things-which-can-be-opened): things which can be opened
* [2012-07-11](http://strangelyconsistent.org/blog/july-11-2012-things-which-contain-other-things): things which contain other things
* [2012-07-12](http://strangelyconsistent.org/blog/july-12-2012-platform-things): platform things
* [2012-07-13](http://strangelyconsistent.org/blog/july-13-2012-things-which-you-can-read): things which you can read
* [2012-07-14](http://strangelyconsistent.org/blog/july-14-2012-hidden-things-which-can-be-revealed): hidden things which can be revealed
* [2012-07-15](http://strangelyconsistent.org/blog/july-15-2012-things-which-can-be-carried-around): things which can be carried around
* [2012-07-16](http://strangelyconsistent.org/blog/july-16-2012-things-which-are-part-of-the-scenery): things which are part of the scenery
* [2012-07-17](http://strangelyconsistent.org/blog/july-17-2012-getting-things-from-the-car): getting things from the car (car, rope, flashlight)
* [2012-07-18](http://strangelyconsistent.org/blog/july-18-finding-the-door-in-the-grass): finding the door in the grass (grass, bushes, door)
* [2012-07-19](http://strangelyconsistent.org/blog/july-19-filling-your-car-with-leaves): filling your car with leaves (trees, leaves)
* [2012-07-20](http://strangelyconsistent.org/blog/july-20-putting-the-leaves-in-the-basket): putting the leaves in the basket (sign, basket, walls)
* [2012-07-21](http://strangelyconsistent.org/blog/july-21-2012-its-too-dark-in-here): it's too dark in here! (can't see, turn on the flashlight)
* [2012-07-22](http://strangelyconsistent.org/blog/july-22-2012-playing-the-hanoi-game): playing the hanoi game (walls, all the disks)
* [2012-07-23](http://strangelyconsistent.org/blog/july-23-2012-being-blocked-by-the-fire): being blocked by the fire (fire, walls)
* [2012-07-24](http://strangelyconsistent.org/blog/july-24-2012-fetching-water): fetching water (brook, water, helmet)
* [2012-07-25](http://strangelyconsistent.org/blog/july-25-2012-putting-out-the-fire): putting out the fire
* [2012-07-26](http://strangelyconsistent.org/blog/july-26-2012-doom-and-cavern-collapse): doom and cavern collapse
* [2012-07-27](http://strangelyconsistent.org/blog/july-27-2012-triggering-doom-and-dying): triggering doom and dying (pedestal, butterfly, walls)
* [2012-07-28](http://strangelyconsistent.org/blog/july-28-2012-moving-around-iii-movement-synonyms): moving around III (movement synonyms)
* [2012-07-29](http://strangelyconsistent.org/blog/july-29-2012-verb-synonyms): verb synonyms
* 2012-07-30: tying up various loose ends
* 2012-07-31: the finished game

Stand by, blog post number one is already in the pipeline.
