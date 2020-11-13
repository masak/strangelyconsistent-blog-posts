Debugging is weird.

It's a fundamentally contradictory activity.
I mean... you, the developer (by which I clearly mean "I, the developer", but let's commiserate together) obviously didn't make a _mistake_ when coding up the thing.

Why would you make mistakes?
That makes no sense.
You coded the thing the way you did, because that's what you meant, and so, ipso facto, it's correct.
And now you're looking for the mistake in it, going over the code _again_, even though _clearly_ it's correct and the universe just doesn't realize it.

Maybe it's the compiler.
Maybe some hardware error?
Could be the laws of physics are broken.

And so, in a magnaminous show of good will, you're going over the code a third time, just to check all your assumptions.
You look at a line of code, confirming that...

...oh.

Ok, there's the mistake.

You discretely discard your draft email to the compiler vendor.

Ok, so it was human error this time!
PEBKAC.
One of those rare occasions.
Programming is easy!

(Fair warning: this post is more of an expressionist painting than a full tutorial.
It's best thought of as a retroactively recreated core dump of my thoughts as I found a bug.)

<p class="separator">❦</p>

In my increasingly exciting [race to the finish line](https://github.com/masak/bel#state-of-completion), I'm implementing continuations in Bel.
Well, I was, already back in June, but reality's stubborn refusal to _run my code correctly_ prevented me from making any progress.
The code, needless to say, looked fine.
(Duh.)

Some other time, I'll have a go explaining what continuations _are_, philosophically.
It does not matter for this post.
I'll give a short list of what they enable in the language, though:

* Abrupt exit from loops and blocks, like `break` and `continue`
* ...even several layers deep (like labeled `break` and `continue`)
* Various error handling, à la `try`/`catch`
* `return`-like behavior from anywhere in a function
* Pausing and _resuming_ a function, in the vein of `yield` and coroutines

(In summary, continuations mess with your control flow.)

If we ignore the metaphysics and just look at what it takes to implement a continuation, it needs to be an object that captures a moment in execution.
It needs to preserve the execution state at that moment.
The execution state, for our purposes, consists of two things:
what we've yet to compute (`s`), and what we've already computed (`r`).
`s` as in (expression) "stack", and `r` as in "result" (values).
Easy!
We save those in a Bel-native object, and later when we restore it...

...it doesn't work‽

As a side quest, I realized this is the point where I really shouldn't leak any internal implementation details out of the Bel interpreter (written in Perl).

Back in June, I wrote:

> Interesting unexpected consequence of this work:
> because the value stack now gets "exposed" by being stored in a continuation,
> a number of non-Bel values would need to be changed into pure Bel values.
> This includes our three types of `smark` so far: `fut`, `loc` and `bind`.
> That is probably a good change to make anyway.

I realize I have some explaining to do, with those special terms.
It'll all make sense, I promise.

Anyway, to make a long story short before the guided tour through the Bel interpreter:
I fixed the above internals wardrobe malfunction, rebased my branch with the continuation work, and voilà:
it _still_ didn't work!

<p class="separator">❦</p>

Imagine you're the person in the [Searle's Chinese Room](https://www.plato-philosophy.org/teachertoolkit/searles-chinese-room-computers-think/).
(That one's been on my mind lately; can't imagine why.)
All day you get passed notes with symbols on them, which you look up according to rules to take the appropriate actions and produce new notes with symbols on them.

But there's one additional complication: you seem to have task-induced amnesia, and can't seem to trust yourself with remembering the steps of _multi-step_ procedures.
Carrying out a task confined to a single note is not a problem at all, but...
any note that requires handling additional subtasks, and then saying "right; where were we?" _completely_ stumps your abilities as a filing clerk.
Let's say the in-world explanation for this is that your workspace gets messy, you lose track of your original note, and it's too long ago since you read it.

Eventually, you come up with a brilliant solution to this:
you start to leave notes _to yourself_ in your inbox.
The notes explain exactly where you were in your previously put-on-hold task, allowing you to smoothly recover your context.
(To be honest, this sounds like a trick out of the "inbox zero" crowd's playbook.)

This type of self-note is what the Bel interpreter calls a `fut`.
It's a mechanism for keeping long-term memory and context around while still separating the work of interpretation into small, granular, non-nested subtasks.
In the original Bel code, `fut`s are thin wrappers for Bel closures.
In my Perl port, I sensibly made them thin wrappers for Perl closures &mdash;
which worked fine until this new continuation functionality _snitched_ on me and exposed them, since the expression stack `s` is now visible in userspace.

So, I had to fix that, naturally.
The `fut`s are now pure Bel objects, with no visible event horizon into Perl-land.
(Hidden away in folds in undetectable space, there's still a Perl closure. I'll need that for as long as my interpreter is actually in Perl. But now you can't see it anymore from userspace. Lies, damn lies, and implementation.)

For completeness, here are all the `smark` types of the special values that end up on the value stack:

* `fut`, which we just talked about, representing the tail end of a multi-step task in the interpreter.
* `loc`, signalling that we're interested in evaluating something's _location_ (lvalue), not its value.
* `bind`, declaring a dynamic variable &mdash; yes, these bindings live on the expression stack.

In retrospect, it's not so weird that making these `smark`s native didn't affect the correctness of the continuations one way or another.
Sure, the continuation object contained Perl values that had leaked out into Bel space, but... as long as you didn't look at them, that wasn't really an issue.
And the were to all appearances correctly put back into the interpreter later.

So the mystery remained.
I glared at the obviously correct code.

<p class="separator">❦</p>

One place where we have to use several `fut`s is when interpreting the call to a function.
This is a multi-step process:

* Evaluate the head of the form. Let's say the form is `(+ x y z)`, so the head is `+`.
* _Where were we_? Is it a function? Yes.
* Ok, time to evaluate all the arguments, `x y z`. Note that this is essentially a _recursive_ use of the interpreter &mdash; it's asking _itself_ to evaluate all these expressions, by pushing them onto the value stack.
* ...ok, now, _where were we_? Once it's evaluated all these arguments, a `fut` (which the interpreter had _also_ conveniently pushed onto the expression stack, below all the arguments) tells it how to proceed with the function call.
* For each of the arguments, cons it to a list-of-arguments `$args`, that we can send on to `applyf` which will handle the more specific things in a function call.

I looked at this code, although it had really worked fine for months at this point.
It's some of the first code I wrote, since function calls are so fundamental.
It's maybe the most involved part of the interpreter, as it needs no less than _two_ `fut`s to do its job.
It also needs _two_ iterator-like variables, which I use to step through the arguments.
For reasons that make sense in context, these are called `$es1` and `$es2`.
The code that initializes them looks like this:

<pre><code>my $es1 = pair_cdr($e);</code></pre>

`$e` being the whole expression; the arguments are in the tail, so the `pair_cdr` makes sense.

And then, a bit further down:

<pre><code>my $es2 = $es1;</code></pre>

Taking a pre-emptive backup of `$es1` before we consume it.
See, these variables really are like iterators, because we end up running them down the cons lists.
Singly linked lists, so you can't run back up again.
Not that you'd want to.

Then we use `$es1` to spool the argument expressions onto the expression stack, we evaluate them, and...
waking up from the next `fut`, we use `$es2` to spool the computed argument values into `$args`.

It all works out nicely, because we always use `$es1` first, and then we only ever use `$es2` once...

Wait.

...oh.

Ok, there's the mistake.

<p class="separator">❦</p>

The second `fut`, the one that took the evaluated arguments and bundled them up for `applyf`, used to only ever run once.
I mean, because of course it did.
No-one's ever heard of a function call that evaluates its arguments once, and then _invokes_ the function _several_ times.

As bad luck would have it though, that's the _exact_ moment the continuation gets taken, just before the second `fut`.
(Not 100% sure why, but why is not as important as the fact that it does.)
And so that once-safe assumption gets broken.
And so `$es2` would now be all used up, and so continuations wouldn't work.

Looking at the code again, though, I found it strange that my second line of code broke a dearly-held principle of mine:

<pre><code>my $es2 = $es1;</code></pre>

Namely, that variables should have the smallest possible scope.

In the case of `$es2`, it had a scope that was wider than the second `fut`, even though it was only used inside of it.

Simply moving it inside the second `fut` wouldn't work, though:
by the time the second `fut` runs, `$es1` is already consumed.
But that's easily solvable by also rewriting the line like this:

<pre><code>my $es2 = pair_cdr($e);</code></pre>

Boom!
Now continuations worked!

<p class="separator">❦</p>

All this to say, debugging is weird.

You get to ride a roller-coaster from understanding nothing one moment, to Boris "I am invincible" Grishenko the next.
If you're lucky, you get to remember that not that long ago, you were the person who made the mistake you now so clearly understand and would never make again.

And, the gods willing, the next time you're ready to entomb the same mistake in a piece of code, you'll look up, remember the long debugging session, and steer carefully around that mistake...

...towards new, more interesting ones.
