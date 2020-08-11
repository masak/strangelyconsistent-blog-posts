## The why of it all

The Y combinator is one of those arcane things in computer science, a bit like monads, that seems to attract gushing tutorials about how _incredibly awesome_ it is.
This is yet another one of these tutorials.
As everything in this series, it is necessarily imperfect, fleeting.
If it works as an explanation for you, great.
If not, then we might have a meeting of the minds on some future topic, perhaps.

I'm going to let [Paul Graham](https://sep.yimg.com/ty/cdn/paulgraham/bellanguage.txt?t=1595850613&) take a first stab at explaining the Y combinator:

> Next comes a familiar name, yc. This is the Y combinator, which is
> used to generate recursive functions. It's used in rfn to make a
> recursive function with a name,
>
> <pre><code>&gt; ((rfn foo (x) (if (no x) 0 (inc:foo:cdr x))) '(a b c))
> 3</code></pre>
>
> and rfn in turn is used to define afn, which lets you refer to a
> function within itself as self.
>
> <pre><code>&gt; ((afn (x) (if (no x) 0 (inc:self:cdr x))) '(a b c))
> 3</code></pre>

Ok, the Y combinator can be used to generate recursive functions.
Good, now we have a "true north"; this is its purpose.
At this point, we can focus on how, and... why.

In fact, let's approach the generation of recursive functions via a detour.
As an apéretif, I'm going to show you how you can hang your computer.

Now, you're probably thinking at least two things.
And I know.
But let's talk about that.

The **first** thing you're likely thinking that causing your computer to hang is (a) stupid, (b) a waste of your time, and (c) not something you will want to add to your CV.
And you're mostly right, except for this: sometimes it's good to know the limits of your machine, and the limits of software.
We can agree that it is _undesirable_ for the computer to hang, but instead of shying away from it, we can embrace it and maybe learn something from it.
Also, who knows, maybe we can even homeopathically dilute the useless hanging behavior into something useful?

The **second** thing you're likely thinking (assuming you're a professional programmer) is that you already have some methods for causing your programs to hang.
Good!

Let me guess: **Method A**: infinite loop.

<pre><code>&gt; (while t)
*hangs*</code></pre>

A classic.
The condition &mdash; `t` &mdash; will (by nature and strong habit) never be false.
Even though the loop body is empty, it will keep executing for ever.

Raku has an elegant way to write this as well: `loop {}`.
In fact, in most languages, you can reach for something like `while (true);` or `for (;;);`.

But no, I wasn't thinking of anything to do with explicit loop constructs or explicit `goto`s.

"Aha!" you say.
"Then you were thinking of my **Method B**: recursive function without a base case!"

<pre><code>&gt; (def foo () (foo))
&gt; (foo)
*hangs*</code></pre>

And yes, I was thinking about recursion, but my way of doing it doesn't name the function.

At this point, you-my-dear-reader has either seen this trick before, and you're going "oh, that one? well, fine", or you haven't seen it, and you're going "doesn't name the function? but that's _impossible_!"

Let's first discuss why it's impossible, and then we'll do it anyway.

It's impossible because you need a name to be able to "access"/pinpoint the function when you call it.
If you don't have a globally defined function with a name, then you can't name it and then you can't call it.

Now, the above is subtly wrong, but instead of explaining exactly how it's wrong, let's just make a **Method C**: recursive anonymous function which passes itself as an argument!

<pre><code>&gt; ([_ _] [_ _])
*hangs*</code></pre>

I, ok, I'm being pedagogically clumsy here by conjuring up the `[...]` syntax in the middle of showing the cool trick.
That's just because it's very short, but it's really only syntactic sugar for this:

<pre><code>&gt; ((fn (_) (_ _)) (fn (_) (_ _)))
*hangs*</code></pre>

Which, as you can now see, is a pair of anonymous twin functions taking one parameter (called `_` in both cases), and _calling it on itself_.
The body of the first anonymous function has a call of something on itself, but this something is the _second_ anonymous function, and so the `(_ _)` part ends up being functionally identical to the whole.
Phew!

(Let's call a `[_ _]` thing a "looper", just to have a way to &mdash; yes, irony not lost on me &mdash; refer to it.)
The first looper only really runs once, and then we're in the second looper, forever.

That's it.
That's the trick.

Ok, ok, wait.
Let's analyze this.
In all the three methods, there's some underlying computational mechanism that allows the program to keep looping infinitely:

* Method A: by the power of a LOOP (or, on a lower level, a `goto` backwards in the program).
* Method B: by the power of a PREDECLARED GLOBAL.
* Method C: by the power of DUPLICATING A PARAMETER.

Being able to create an infinite loop through parameter passing alone is what makes this a powerful method.
We're using tools straight from the lambda calculus, the fundamental substrate of all of computing, and nothing else.
Registering functions and talking about global names is an extra layer of functionality on top of the lambda calculus, but it turns out not to be necessary.
Using loops and `goto`s is more natural on the Turing machine, but here it also turns out to not be needed.

What we've just seen is the Ω combinator.
("Ω" as in, I guess, "that's it, show's over".)
Like we said, that much infinite looping is more than most people bargain for; kind of like having a _perpetuum mobile_, but everything it can do is power itself.
Fortunately, the Y combinator is quite a small step away from the Ω combinator.
Here's how it's defined in bel.bel:

<pre><code>(def yc (f)
  ([_ _] [f (fn a (apply (_ _) a))]))</code></pre>

The ingredients of the Ω combinator are still in there, but we've added a layer to harness the infinity.
There's a new anonymous function around the second looper, which means it doesn't loop into infinity right away, but only when you call it.
We don't have to name this new anonymous function, but in order for us to keep track of it, it might help to think of it as `self` (which is also what `afn` names it).

<pre><code>&gt; ((yc (fn (self) (fn (x) (if (no x) 0 (inc:self:cdr x))))) '(a b c))
3</code></pre>

Maybe a visual metaphor will help settle the matter.
The Ω combinator is like a circular arrow, pointing all the way around to its own beginning.
The Y combinator is like 90 percent of a circular arrow; the last 10 percent are provided by the `(fn (self) ...)` part above.
Separately, neither the Y combinator and `self` make a whole loop around the circle.

But when they combine, Hollywood-style, they just happen to magically activate each other and create... recursion.

Paul Graham likes the Y combinator so much that he named his incubator company after it.
It's punny because an incubator does exactly what the Y combinator does: it keeps generating new things, iteration after iteration, while staying the same.
Of course, when the pun is so obscure it's basically a shibboleth, that can work to your advantage as well.
Now you know why.
