Citizens!

In our last instalment of "thing X doesn't really exist", I [refuted the existence of `private`](http://strangelyconsistent.org/blog/privacy-is-an-illusion-epoq-diary-009) to &mdash; what I can only assume was &mdash; everyone's utter satisfaction.
Today, we'll tackle `else if`, an easy target for refutation.

Think of [the Principle of Compositionality](https://plato.stanford.edu/entries/compositionality/) the idea that meaning comes from the _structure_ of an expression, and from the meaning of its _parts_.
That is, to understand something like `2 * 3`, you need to understand the meaning of multiplication (`something * something`), and the meaning of `2` and `3`.
And that's it.

That may sound trite, but the power of this principle is that it works so well _because_ it's so boring.
Things that _aren't_ compositional in that way (like, uhhh, continuations, or mutable state) are all the harder to pin down, because we essentially have to zoom out forcefully until we find a wide enough context in which they can be considered to _be_ compositional.
It's almost as if compositionality is all we've got, and so when we lose it, we desperately want it back.

In programming language grammars, I see this in action quite often.
The upshot seems to be that grammar rules are simpler and more straightforward than you first expect:

* You're parsing a for loop construct, something like `for [1, 2, 3] -> $e { ... }`. Great, so you just need to fish the elements out of that array literal, and loop over them, right? Of course not! In general, the expression is not an array literal, but could be _any_ expression &mdash; variable, function call &mdash; and we just hope it evaluates to `Array` in the end. No, wait! We generalize, and hope it evaluates to `Iterable` in the end. Anyway, we've postponed most of the complexity to runtime.

* You're parsing a function call, like `foo()`. Great, so you just need to get the literal before the parens, right? Of course not! In general, that could be `foo.bar()` or `foo().bar()` or `(new Foo())()` or really _any_ expression, as long as the precedence is right. Again, the grammar is actually _simpler_ for it, by being more general than we might first think.

That's the principle of compositionality at work: grammar rules end up simpler than you think, because being too specific means you're not properly relegating meaning to the constituent parts.

We're now ready to look at how two major languages define their `if` statements.
First up, [Java](https://docs.oracle.com/javase/specs/jls/se15/jls15.pdf):

<pre><code>IfThenElseStatement:
  `if` `(` Expression `)` StatementNoShortIf `else` Statement</code></pre>

There's no `else if` here, because the `if` could simply belong to the nested rule `Statement`.
So, you see, `else if` is not real, or at least not _more_ real than other mirages like `else while`, or `else switch`.

(Parenthetical ad: do read the excellent historical review [if-then-else had to be invented](https://github.com/ericfischer/if-then-else/blob/master/if-then-else.md).)

Surely this is a fluke, though?
Well, let's look at another popular language and its language definition.
Ladies and gentlebots, I give you [EcmaScript](https://262.ecma-international.org/11.0/#sec-if-statement):

<pre><code>IfStatement:
  `if` `(` Expression `)`
    Statement `else`
    Statement
  `if` `(` Expression `)`
    Statement
</code></pre>

(Weird and irrelevant cyan footnotes elided.)

It's the same!
There's no mention of `else if`!

To summarize: `else if` feels like a single concept to us, but it's not; they belong to different parts of the grammar's call tree.
We keep up the illusion that `else if` is really a thing by collectively agreeing to indent chained `if`-`else if`-`else` statements in a really odd way.

Now, there _are_ langauges that buck the trend here, by not choosing the straightforward path of compositionality.
I'm looking at you, Perl (`elsif`), and you, Raku (also `elsif`), and you, Python (`elif`).
This may seem rebellious and principled, until you realize that it's really only a missed chance at simplifying the grammar to make it more regular.

Just for reference, here's [perldoc perlsyn's summary of `if` statement syntax](https://perldoc.perl.org/perlsyn):

<pre><code>if (EXPR) BLOCK
if (EXPR) BLOCK else BLOCK
if (EXPR) BLOCK elsif (EXPR) BLOCK ...
if (EXPR) BLOCK elsif (EXPR) BLOCK ... else BLOCK</code></pre>

Hm! I realize now that I look at this, that part of the reason Perl does not have that simple recursion on `Statement` that Java and JavaScript have, is that Perl requires curly braces around statements. (And so there is no longer a nice Strange Consistency to latch onto between the `else` case and the `else if` case.)

Still, that only raises more questions: did Larry decide to go with `elsif` first, and then decide to go with mandatory curly braces &mdash; or was it the other way around?

[Python](https://docs.python.org/3/reference/compound_stmts.html#the-if-statement), it turns out, has much the same reasons for not leaning on the Principle of Compositionality, but with famously different syntax:

<pre><code>if_stmt ::=  "if" assignment_expression ":" suite
             ("elif" assignment_expression ":" suite)*
             ["else" ":" suite]</code></pre>

Python may not require curly braces, but the colon and the indentation amounts to the same thing, and so here too, there is no recursion on `Statement`, because there can't be.

Ok, ok.
This blog post did not end up quite where I expected it to, but perhaps the conclusion is a bit more interesting than I expected.
Languages that can lean on the Principle of Compositionality do so, quite rightly.
Languages that can't, don't (natch), but just to hammer that in, they also choose weird spellings for `else if`, like `elsif` and `elif`.
(Clearly, we need a hipster language to take the trend one step too far and just use `eif`.)
Cause and effect here are not too clear; the whole thing feels a little bit like Structuralism in literary theory; form follows function, somehow, but who is really following whom?
Is the egg a vessel for chickens here, or is the chicken a tenant of eggs?

To top it all off, I now imagine Larry's reply to all of this: (caveat lector: imagined Larry, not real one) "Well, you have to ask yourself, are we doing all of this grammar stuff to achieve some nice theoretical simplicity, or are we doing it to make things more straightforward for the developer?"
And, I hate to say it, but imagined Larry has a point.

Summary: `else if` doesn't exist, but in some languages it doesn't exist a bit more strongly than in others.
