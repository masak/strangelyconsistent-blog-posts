**Tell me about continuations. Explain what they are.**

Aww, that warms the cockles of what passes for my heart.
Only a fictional reader substitute could guess I was in the mood to explain continuations today.

**You're welcome, dear author insert. So, how about it?**

Sure thing.

Unfortunately, no-one can be _told_ what continuations are.

**I see... a decades-old pop culture reference. Disappointing. By the way, Morpheus could have just said "The Matrix is a machine-made virtual world that your real body, currenly residing in a vat full of snot, has been plugged into and receiving stimuli from all your life."**

Right.
There's no need to get overly mystical, when good explanations are all that more satisfactory.

**Ok, so I challenge you &mdash; nay, I _curse_ you &mdash; to be similarly down-to-earth and non-mystical with your explanation of continuations.**

All right.
The continuation represents a running program at some point during its run.

**That's it? That sounds almost too easy.**

There are some details involved, like how the stack is usually considered to be part of the continuation and the heap isn't, but... yeah, that's it.

**I thought continuations were these magical fairy-like beings, whose contours one could only make out near pools of water in dark forests, and in FP languages.**

They sure get hyped up a lot.
Maybe it's the name, or possibly some kind of guilt by association.

**Do you think they have the wrong name, somehow? Kind of like with monads, with that really technical name (Leibniz, seriously?) &mdash; maybe if they did name them "warm fuzzy things", they'd have been easier to understand?**

Somehow I doubt monads are hard to grok mostly due to an unfortunate name.
Not saying monads couldn't have been named better, mind you.

Continuations, on the other hand, might have the perfect name.
They are named _exactly_ after the thing they represent: what to continue with.

**...Oh no!**

What?

**I think I forgot what they were. I think I just stopped understanding continuations again.**

Ok, don't worry.
Take it easy.
This happens the first few times.

Let me tell you about the Alma implementation.

**Will that help?**

I believe it might.

Alma is implemented in the Raku language.
It's a simple interpreter, not a compiler.
There's a lot of ways you can arrange for the interpreter to work, but one of the questions you need to face is: how do you represent the Alma stack?
The call stack, the thing that grows when you call functions, and shrinks when you return from them.

The Alma interpreter does the simplest thing imaginable:
it ties Alma's stack to Raku's &mdash; the implementation language.

**Ok, so you implement a stack using... a stack? Sounds both meta and kind of appropriate.**

It sounds appropriate, but it's actually kind of limiting.
Bit of a straitjacket.

I mean, it gets you fairly far, but for any advanced, "fun" control flow, you have very limited options.

In Alma, it felt like a relevation when I realized that `return` and exceptions could be implemented within the stack model, by throwing exceptions up Raku's stack, and catching them higher up.

But that's about as far as you get with that model. You can't use a similar trick to throw yourself _down_ the Raku stack.

**I sense you're building up to some kind of punchline here, possibly one involving continuations.**

Yes.

Imagine a better way, where instead of tying yourself to the call stack of the implementation language, you let each primitive operation in the implemented language "decide what to do next". Usually, under normal circumstances, what to do next is just "grab the sequentially next statement and execute it".

But sometimes it isn't. What to do next in an `if` statement &mdash; after evaluating the condition &mdash; depends on whether the condition was truthy or not. What to do next after the body of a loop might involve going back to the top of the loop and running it another iteration.

What to do next after a function returns is the thing to do next after the function call in the caller.

These "what to do next" pointers, they are the continuations.
Making your interpreter use them internally leaves you with a lot of flexibility, and doesn't tie you down to the implementation language's call stack.

**Neat! So why didn't you change Alma to work that way?**

I started out making a big refactor once, but I didn't make it all the way.
I seem to recall I just ran out of tuits. It was definitely possible to do.

Anyway, the Bel interpreter works much more along this idea of continuations.
Which is why it was <del>trivial</del> <del>easy</del> not a massive amount of work to expose continuations as first-class values in the language.

**Ok, so continuations allow you to do... weird things with control flow? Is that it?**

Yes!

Well, no.

Well, yes.

**Do you have an example of a weird thing with control flow? Just so we're on the same page here.**

I guess exceptions are the classic example.

Here, let me Bel it for you.

```
(def foo ()
  (prn "Called foo")
  (bar))

(def bar ()
  (prn "Called bar")
  (throw 'some-error)
  (prn "This line never reached"))

(catch
  (foo))

;; "Called foo"
;; "Called bar"
;; some-error
```

**Let me guess... `catch` and `throw` uses a continuation somehow?**

Yep.
`catch` captures its current continuation (the one that expects a return value from the `catch` combination), and `throw` invokes it.

I was going to show how `catch` is implemented, but that might actually be more harmful than helpful than the above high-level explanation.

**Any other examples?**

Hm, coroutines.
You know, things with `yield` and stuff.
Or `gather` and `take` if you prefer.

Something like `await` of `async`/`await` fame.

I guess the common demoninator here is "things that break out of the regular stack-based paradigm".
Continuations are good at that.

**Who came up with this continuations concept?**

[Lots of people, independently](https://homepages.inf.ed.ac.uk/wadler/papers/papers-we-love/reynolds-discoveries.pdf).
It's a fairly natural concept, almost a necessary one.

**Let's just say for the sake of it &mdash; while I still remember what continuations are and before I forget it again &mdash; that I think all this playing around with continuations seems a little pointless, and that call stacks are all you need. What would you say to convince me?**

Well, I'm not going to try to wean you off stacks, but...

...it's almost like they form two self-reinforcing and internally consistent universes, each with its own strengths and interesting points.
I mean stacks (on the one side), and continuations (on the other).

If you think in terms of composability: stacks are really good at dealing with regular functions, no funny business, no resumable exceptions or coroutines or anything like that.
When function calls nest perfectly within each other, stacks work really well.

Continuations get into their groove exactly where stacks fall short:
when the control flow gets a bit "weird and interesting" (in the above-mentioned ways).

I think what's important to realize there is that _continuations compose really well_ too;
they just happen to have a different shape than function calls do.

**Shape?**

At the front, a continuation expects zero or more values (kind of like a parameter list), and then it runs some code (kind of like a function body).

Instead of returning "up the stack" to some caller at the end, the continuation just hands over to some other continuation.

**You know, when you present it in that way, it sounds _really_ close to what I think of as a function or a closure.**

I know.
It is.

The one _crucial_ difference is that "up the stack" thing, which functions/closures have, but continuations don't.

In the stack world, there's always an "up" direction (back to the caller) and a "down" direction (further into callees).
In the continuation world, it's better to think of it as being "weightless", and each new invoked continuation just leads onwards.

Ten years ago, [this was mind-blowing to me](http://strangelyconsistent.org/blog/a-sudden-insight).
Now, I've passed through the event horizon, and it seems almost natural.
Almost.

**Can you convert code between "stack style" and "continuation style"?**

Well, yes, but...

...before we do, you have to understand that this is usually a property of the _language_ (whether it uses a stack or continuations).

Having said that, we can almost always emulate these mechanisms "one level up", so to speak, using data structures and control structures manually in the language.

When we do it with continuations, we call that "Continuation-Passing Style".

I did the transformation to CPS [with one of Knuth's tree-traveral algorithms](https://github.com/masak/taocp/blob/09082481d42dc0b87290e85101f547426bea0584/ch2.3.1-algorithm-t/README.md), and I'm quite proud of how it all came together, and helped explain (to me, at least) why the algorithm in the book could look so unlike a plain recursive function, and yet be related to it.

In order to manually convert some code to CPS, you introduce a `state` variable, like in the example, and a `switch` statement in a loop. And then you arrange to "jump around" between your continuations by changing the value of `state`.

**Cool!**

Yes, I happen to think so.

Note that this approach is limited to one function only.
It won't work if the function calls other functions, because then we conceptually leave the `switch` statement and the CPS-transformed code.

In the example, there were recursive calls, and those are quite straightforward to handle (with some extra thought to storing stack values on a dedicated value stack).

**Ok, so, let me get this. Calling other functions from a CPS-transformed function is a bad idea?**

I would put it like this: continuations do not mix well with normal stack-based function calls.

It might be more subtle than that, but this is my experience: they do not mix.

**I would say "I believe you" immediately, but I sense that if I did, you wouldn't tell me any war stories about the things that can go horribly wrong...**

Fine.
Since you insist.

A couple months ago, I was [quite happy to land continuations](http://strangelyconsistent.org/blog/continuations-epoq-diary-010) in Bel.
As usual, that unlocked some new scripts that had been waiting in the sidelines, and so I could finally try out [the Bel script that moves a robot around on a board](https://github.com/masak/bel/blob/7334798c996ad38e473eaffc9ae9853a7fd80c3d/t/board-movements.bel) that I had been writing months earlier.
It used `catch` and `throw` (demonstrated above), and so it had been waiting on continuations to be finished.

I ran it, and... [the script didn't work](https://github.com/masak/bel/issues/273).

Not because I had coded the script wrong.
It didn't work because of a bad interaction between continuations and stacks.

The whole Bel interpreter works using a mechanism that resembles continuations very much.
So `catch` and `throw` were no problem to implement; they worked with regular Bel code, and all their test passed.
Well, their _one_ test passed.

But then there were the "fastfuncs".
These are hand-written Perl versions of many of the Bel built-ins (such as `map`), which currently help run Bel code a bit faster.
These functions are stack-based, though.

The robot code had a `throw` try to jump straight through a `map` to its `catch`.
Hilarity ensued.

**How did you solve it?**

Short-term, I had to remove all the fastfuncs that call another function.
Those were the one causing the problem.

Longer-term, I'm hoping to be able to CPS-transform those fastfuncs, and re-add them! ☺
I just need to trick my brain into not looking while I accidentally get that to work in Bel.
But the principle of it is simple: make fastfuncs work with the Bel continuation-based thinking, instead of against it.

**Did Bel get slower when you removed those fastfuncs?**

Yes, and it was already quite slow.
I needed the extra slowdown like I needed a hole in the head.

**Ok, I'll try to take this lesson to heart and never mix <del>wine and beer</del> stacks and continuations in my language implementation.**

That's good to hear.

This whole episode reminded me of the phrase "inferior runloop" that I heard long ago in the Parrot community.

In fact, if you Google for that phrase, you _still_ get a lot of Parrot documentation and tickets, and not much else.

The inferior runloop, it turns out, was exactly that problem: a mixing of stacks and continuations where, causing a lot of hard-to-debug problems.
My takeaway from all that was that the mixing itself just doesn't work.
Stacks compose well, continuations compose well, but a mix of them composes really rather unwell.

**Look, it's been great catching up! I'm glad you're back to blogging after what I'm sure was a much-needed Winter Solstice sabattical. I'm going to return...**

\*ahem\*.

**...I'm going to _continue on_ with whatever it was I was doing before I was summarily recruited to be a reader substitute.**

You may pass.

