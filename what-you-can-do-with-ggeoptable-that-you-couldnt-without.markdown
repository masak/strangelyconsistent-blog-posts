---
title: What you can do with GGE::OPTable that you couldn't without
author: Carl Mäsak
created: 2009-11-22T16:31:00+01:00
---
In order to explain what I think is exciting about the OPTable parser in GGE,and what you can suddenly do in Rakudo with it that you couldn't before, I put together a small script that parses and evaluates simple algebraic expressions.

<pre><code>$ <b>perl6 examples/algebra</b> 
Redeclaration of variable $top
Redeclaration of variable $topprec
Use of uninitialized value
&gt; <b>2 + 2</b> 
4
&gt; <b>2 + 2 + 2 + 2 + 2 + 2 + 2</b> 
14
&gt; <b>2 2</b> 
Could not parse the arithmetic expression
&gt; <b>10 * 10 + 4</b> 
104
&gt; <b>4 + 10 * 10</b> 
104
&gt; <b>0x20 * 0x4</b> 
128
&gt; <b>0123</b> 
Leading 0 does not indicate octal in Perl 6
123
&gt; <b>0b1101110 + 0o412</b> 
376
&gt; <b>4e3 + :4&lt;301&gt;</b> 
4049
&gt; <b>^D</b> 
$
</code></pre>

Sorry about the warnings in the beginning, I'll track them down eventually. 哈哈 (**Update 2009-12-01:** Found them. Leaving them in for historical authenticity.)

Anyway, you'll see from the above that the script undestands the addition and multiplication operators, it can take long chains of them, it gets their precedence right, and it understands hexadecimal, binary, octal, scientific notation and arbitrary radices.

And it weighs in at [56 lines of code](http://github.com/masak/gge/blob/master/examples/algebra)! (48 if you don't count the empty lines.) No, that was not me going mad trying to compress a lot of code into those ~50 lines, I'm just leveraging `GGE::OPTable` (for generating the parse tree) and `Perl6::Grammar` (for parsing the different types of numbers). Even the warning about the leading 0 for the non-octal `0123` gets through intact from the Perl 6 parser. Unintended but kinda nice.

In a sense, Perl 6 (and Rakudo) already has this kind of expressive awesomeness built in, because you can define your own operators as subs and methods. But in another sense, it doesn't, because in order to use those operators, you have to put them in your program, or eval them. With `GGE::OPTable`, you can create your own domain-specific expression language, feed in an expression, and get an AST back.

I'm sure other bloggers can run with this and produce even more impressive examples than just the parsing of `+` and `*`. Happy hacking!

(**Update:** After thinking about it a bit, I refactored the parsing sub in `examples/algebra` into `GGE::OPTable`, so that people don't have to write their own. Now the code is cleaner, and only 45 lines long, or 39 if you don't count the empty lines.)


