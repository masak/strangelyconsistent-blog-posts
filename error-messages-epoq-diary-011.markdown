---
title: Error messages (epoq diary 011)
author: Carl Mäsak
created: 2020-11-25T21:54:00+08:00
---
I've been thinking about error messages lately.

Error messages from programming language implementations need to be good.
Informative.
There are a couple of big no-nos that you simply never do, but also a number of "moments of charm" that are easy to overlook.

Here is the prototypical bad error:

<pre><code>Error: invalid input</code></pre>

The trifecta is complete in this one:

* Computer says _no_
* ...while blaming the user ("invalid")
* ...but offering no hints or details.

One of the things I've been proud of with Raku's error messages is that they almost never do this.
(For years, the required minimum in Raku error message quality has been "awesome", and if an error message is less than that, it's a bug.)

Just to be clear, here's the Raku ideal for errors:

* Computer says _sorry_
* ...using terminology that puts the blame on the program ("unrecognized", or "cannot")
* ...giving all available contextual information (see below)
* ...and possibly giving a hint based on a statistically likely guess.

Now _that's_ service!

Instead of digging up an example of an excellent Raku error &mdash; some other time &mdash; let me just say this:
Raku doesn't have a monopoly on good error messages, nor is it even the undisputed Chosen One in that regard.

That title, I believe, goes to Elm.

<pre><code>-- TYPE MISMATCH ---------------------------- Main.elm

The 1st argument to `drop` is not what I expect:

8|   List.drop (String.toInt userInput) [1,2,3,4,5,6]
                ^^^^^^^^^^^^^^^^^^^^^^
This `toInt` call produces:

    Maybe Int

But `drop` needs the 1st argument to be:

    Int

Hint: Use Maybe.withDefault to handle possible errors.</code></pre>

(Not included in my blog, but quite evident on [Elm's web site](https://elm-lang.org/): syntax highlighting.)

I love the hint at the end!
That one is really hard to pull off, but when you do, that's _delightful_ to a user in need.
This is along the lines of "don't just show the error, show _possible next steps_".
Don't just say "no", or even just "sorry", but say what might come next.

Phew!

<p class="separator">❦</p>

I have an issue open for my Bel implementation, [#18](https://github.com/masak/bel/issues/18).
No need to go look at it, because I'm going to reproduce most of it here.

Currently, I type in some command with a deliberate mistake in it, and here's what I get:

<pre><code>&gt; (srrecip '(+ nil (t t)))
'mistype</code></pre>

Sad.

I mean, on the bright side, it doesn't spend much time blaming the user.
But that's mainly because it doesn't utter much at all.
Silence might be golden in some cases, but not in this one.

Skipping right to the error message I would like to get (with an example that tries to use the built-in function `srrecip` to unlawfully create a rational number with a zero as the denominator):

<pre><code>&gt; (srrecip '(+ nil (t t)))
'mistype

In the following function call, the `n` parameter did not satisfy its type
predicate:

                   '+   nil          '(t t)
                   |    |            |
                   v    v            v
    (def srrecip ((s (t n [~= _ i0]) d))
      (list s d n))

The type predicate is: [~= _ i0]    (`i0` has the value `nil`)
The type checker reduced the predicate to: "any value except `nil`"

But the value passed in to `n` was: nil
In order to make the call work, `n` needs to bind to a value which is not `nil`.

The value `nil` was passed to `srrecip` from the REPL:

                   here
                   |
                   v
    &gt; (srrecip '(+ nil (t t)))</code></pre>

I dunno, maybe this is a bit _too_ good.
Certainly details would differ when this became a real error message, generated by a machine instead of me dreaming.

I may or may not add a splash of color (as long as it still works fine on non-ANSI terminals).

I also added in the OP of that issue what I value in an error message:

> * The source code that went wrong
> * An arrow or some kind of marker pointing to the operator that couldn't be applied
> * Line and column (hm, maybe only important when it's not the REPL)
> * Some more detailed explanation in _polite, empathic English_
> * A suggestion of what to do to make things right

(Emphasis mine. Uh, I mean, mine today.)

Let's go through these.
I'd like to get a good feeling for what this would take.

Oh! First off, I just noticed that the error starts off by focusing on the _callee_ first.
That both does and doesn't make sense.

From one perspective, once this error happens, you're already calling the function, and so you're already "in" the function in some sense, even though you haven't started executing the body.

On the other hand, the "blame" quite clearly rests with the _caller_, that is, the code sending in the argument that doesn't type-match against the parameter.
If anything, it's probably better to front-load that information.

(Really, I could argue both ways about that one.
Python shows its stack traces innermost-last, on the theory that people will be looking at the bottom part first for clues about what went wrong.)

The biggie on the list is "the source code that went wrong".
Stating the obvious, source code is one thing, while whatever internal representation that's running once your interpreter/VM is running, is quite another.

In the case of my simplistic Bel interpreter as of today, we're in luck, because the internal list structures really aren't that far from the source code itself.
(In the future, that might change.)

So all that needs to happen is Bel needs to mark up the cons pairs in the source code with file-line-column information.
It should of course work both for REPL code, for code in files, and for all the built-in code (like the `srrecip` call above).

(Here's where we once again use the "pocket universe" style of programming in the interpreter.
It's quite doable to make those cons pairs representing the code look completely normal, while also carrying hidden information that only the interpreter has access to.)

I consider that part to be a matter of tedious but straightforward programming.
What comes next will be quite the opposite &mdash; uh, immediate but gnarly?

The `mistype` error is thrown from the depths of the Bel interpreter, in a utility function called (predictably) `typecheck`.
It's from this function we need to build/emit the excellent error message.

Looking at what information we _have_ at that point: nothing.
Only the fact that the typecheck failed.

We need:

* The function we are trying to call (and its file+line+column)
* The code that is trying to call it (and its file+line+column)
* The predicate used for typechecking, which just failed (and its file+line+column) &mdash; hey, we do have that one!
* The _value_ that failed the typecheck &mdash; seems we have this one too

Ok, so not quite nothing. Two out of four.

We _might_ even be able to reconstruct the surrounding function from the location information in the predicate, but... I'm not sure how robust that is.

And either way, there's no way we can get at the information about the caller without doing something at this point.

Eyeing the interpreter, I'd say the point to register who the caller is would be `evcall`.
Of course, who "the caller" is needs to be juggled in a delicate way, because making function calls as part of evaluating arguments is definitely a thing.

Anyway, ok.
This seems possible.
Even seems possible to evolve incrementally in some way.

<p class="separator">❦</p>

One thing that sort of shows up here is the separation between "compile time" (when we have full access to the source program) and "run time" (when we are more interested in interpreting whatever format the target program is in).

Error messages squish together these two worlds; while the runtime error definitely happens in the target program, what's user-friendly is _showing_ it happening in the source program.

The reason for that is simple: while it's definitely convenient for the computer to pretend that the target program is the only thing that exists, for the human user it's the opposite &mdash; the source program is the only thing they expect to deal with.

It's for the same reason "source mapping" is a thing in the compile-to-JavaScript world &mdash; when things to go south, the causes of the problem will be found in the source, not in any code generated from that.

It's a bit of an illusion, but a very helpful one.

