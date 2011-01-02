---
title: Rakudo: the good, the cool, the awesome, the bad, the ugly and the weird
author: Carl Mäsak
created: 2008-08-29T13:36:00+02:00
---
If you decide to use Rakudo Perl 6 today, you're in for a few pleasant surprises. Quite a number of Perl 6 features already work today. A few of these features are in themselves reason enough to check Rakudo out, and some are truly awesome, as in I-won't-ever-be-satisfied-with-conventional-coding-after-this awesome.

(And by the way, when I say "check Rakudo out" I literally mean

     $ svn co https://svn.perl.org/parrot/trunk parrot
     $ cd parrot
     $ perl Configure.pl; make; cd languages/perl6/; make perl6
     $ ./perl6 -e 'say ~<Just Another Early Perl 6 Adopter>'

You know you want to.) **[Update: Nowadays, you might want to check out [these instructions](http://rakudo.org/how-to-get-rakudo) instead.]**

Then there's the issues, the kind that inevitably exist in projects at this early stage (both Parrot and Rakudo). They are mentioned here not as criticism, only as honest commentary about the (rapidly changing) state of the Perl 6 implementation on Parrot. (They may also serve as a reminder that the project warmly welcomes testers, bug reporters, patch contributors, documenters, spec writers and early adopters.)

Current flaws in Rakudo range from missing features to actual problems, the latter which you can often work around, albeit laboriously. There are also defects which even fall outside of this spectrum; bugs that conjure up thoughts about bureaucratic red tape, sudden epilleptic seizures, or Heisenberg's uncertainty relation.

Many of the features described are in use in [November](http://github.com/viklund/november/) today. Some of the problems described are felt (with varying severity) in November today.

## The good

A good deal of basic things that you as a Perl 5 user might expect to work, mostly do. This includes `$_`, `my` declarations, `if`/`while`/`for`, `sub`, scalars, arrays, hashes (and their respective functions), `map`/`grep`, arithmetic and string operators. Since these constructs form the basis of all other coding, having them in Rakudo today is good.

String manipulation, the universal problem solver, is mostly available -- including very decent double-quoted string interpolation. As an extra bonus, `sprintf` helps with your everyday formatting needs. That's very good.

File handling, as in `open` and `readline`, is in place. You can even iterate over the lines in a file using the [prefix iteration operator '`=`'](http://perlcabal.org/syn/S03.html#line_681). That's good too.

You can put your code in separate modules. `use` works. Being able to divide code into modules is good.

You can write parameter lists for your `sub`s. Not just Perl 5 prototypes, [real parameter lists](http://perlcabal.org/syn/S06.html#Parameters_and_arguments). (With type constraints!) You can keep using `@_` and `shift` if you want, but you don't have to. Having this choice is good.

## The cool

Objects/classes work to a great extent (jonathan++). That includes [methods](http://perlcabal.org/syn/S12.html#Methods), [attributes](http://perlcabal.org/syn/S12.html#Attributes), class attributes, [constructors](http://perlcabal.org/syn/S12.html#Construction_and_Initialization), the [ `self` keyword](http://perlcabal.org/syn/S12.html#line_163), [derived classes](http://perlcabal.org/syn/S12.html#line_101)/overriding, [roles](http://perlcabal.org/syn/S12.html#Roles) and [ `does` ](http://perlcabal.org/syn/S12.html#line_1278), [enums](http://perlcabal.org/syn/S12.html#Enums), dynamic dispatch and [namespaces](http://perlcabal.org/syn/S10.html). Being able to program object-orientedly in Perl 6 today is cool!

The keyword [ `given` ](http://perlcabal.org/syn/S04.html#Switch_statements) has been implemented. So have [ `subset`s](http://perlcabal.org/syn/S12.html#Types_and_Subtypes), [chained comparison operators](http://perlcabal.org/syn/S03.html#Chained_comparisons), [junctions](http://perlcabal.org/syn/S03.html#Junctive_operators), [implicit `$_` in method calls](http://perlcabal.org/syn/S03.html#line_260) and [implicit parameters in blocks](http://perlcabal.org/syn/S06.html#Placeholder_variables). The fact that you can use these features today, is quite cool!

Having `Data::Dumper` around in Perl 5 is very useful, but even a module that comes bundled with Perl is still at most just a close relative of the family. Being able to use [ `.perl` ](http://perlcabal.org/syn/S02.html#line_1337) without a `use` statement is cool!

Since about a week ago, modules can be compiled down to standalone PIR files, and any `use` statement which finds a `Module.pir` will choose that file over `Module.pm`, eliminating needless compilation of that module. A 17-fold increase in running time resulting from these caching effects, is cool! (pmichaud++)

## The awesome

PGE, the [Parrot Grammar Engine](http://search.cpan.org/~rgrjr/parrot-0.7.0/compilers/pge/README.pod), forms a vital part of the compilation process in Parrot. It's a very usable implementation of Perl 6 rules (pmichaud++), and is used to parse source code into a parse tree. Needless to say, a parser that can understand as much Perl 6 as Rakudo can, needs to be pretty developed already.

In an almost Escher-like fashion, PGE is also being exposed in Rakudo, through the `grammar`, `regex`, `token` and `rule` keywords. Where you previously had to use special tools to create a parser for your programs, you now simply put together a grammar just like you would implement a class in any other language. A lexer and a parser are automatically generated for you behind the scenes.

The sheer power of this is beginning to be felt in the wiki project, as we move more and more of our text handling over to grammars. We've been parsing the wiki syntax itself using a grammar for weeks now, and the code for this is shorter, more nicely separated, and easier to maintain than its Perl 5 counterpart.

In the past few days, we've been [rewriting](http://github.com/viklund/november/tree/new-html-template) our `HTML::Template` module to use grammars instead of bare regexes. The code got smaller (5.4 kb before, 3.9 kb after), *and* it handles nested templates! A grammar makes a parse tree of the template, and a method with some loops and if statements traverse the tree and do the appropriate things. The future addition of the `{*}` construct (which allows methods to hook seamlessly into regexes) would make even the traversal code go away.

Whereas Perl philosophy involves making easy tasks easy and hard tasks possible, this new ability to create your own grammars effectively shifts a whole category of problems from hard to easy. It expands the boundaries of the "Practical Extraction" that Perl can reasonably do. That's pretty awesome.

## The bad

There's no list assignment yet. Calling this "bad" might actually be a bit harsh, it's just... unfortunate. List assignment is one of those features you don't realize you love until you suddenly find yourself without it. **[Update 2009-03-01: Well, we have it nowadays. It rocks.]**

There's a string substitution method called `.subst`, that can be used to produce a new string with some pattern replaced by some substring. It's a crude tool compared to a full-blown `s///` implementation, and every time we have to code something using `while` loops, variables, and `.subst` instead of a well-placed `s///`, it feels a bit bad. **[Update 2010-03-17: It took quite a while, but jnthn++ implemented it yesterday. Oh joy!]**

We miss long dots, heredocs, and dot-assignment a bit. Not having them makes code look bad at times. **[Update 2008-11-03: I missed when it happened, but we have long dots and dotty method calls now.]**

Due to as-yet unimplemented details in the area of control exceptions, `when` blocks in a `given` [do not automatically leave](http://rt.perl.org/rt3/Ticket/Display.html?id=57652) the innermost `$_`-binding block upon completion (as, per spec, they ought to). Depending on your plans for the `given` block, this may or may not be bad. **[Update 2009-03-01: Fixed somewhere along the way.]**

Things like `return if $key == 0` fail to parse, because statement modifiers [aren't recognized](http://rt.perl.org/rt3/Ticket/Display.html?id=57334) as ending a statement yet. You have to write `return() if $key == 0`. Having to cater to the parser because it doesn't completely grok the Perl 6 grammar yet is fairly bad. (Especially when you forget it, and have to find out why it doesn't work.) **[Update 2008-10-11: Fixed in r31163. pmichaud++]**

As stated earlier, you can do advanced things with the PGE stuff exposed through grammars in Rakudo. This includes creating your own regexes, tokens and rules. Just don't call one of them "text", because Rakudo will [shun you](http://rt.perl.org/rt3/Ticket/Display.html?id=57864) for no good reason if you do. Not being able to use the non-reserved word "text" as an identifier in your grammars is bad. **[Update 2009-10-22: This has been working for over six months now. pmichaud++]**

## The ugly

You can't smartmatch against pairs yet. This is used for various file tests, for example checking if a file exists, a *very* common operation. It could be as simple as `$file ~~ :e`. How do we currently check whether a file exists? We `open` it for reading in a `try` block, and set a boolean to `True` if the `open` hasn't already made the `try` block bail out. This emerged as a common pattern in our code, so we quarantined it in a `sub`. Still, it's ugly. **[Update 2008-10-18: Fixed in r32009.]**

Integers above a million (1_000_000) are represented as [floating-point values](http://rt.perl.org/rt3/Ticket/Display.html?id=57790) internally. Where binary arithmetics on 16-bit colour values (using the already available operators `?&`, `?|` and `?^`) could have been possible -- even easy -- any attempts in that direction instead soon turn ugly. **[Update 2009-03-01: No longer the case at all. Someone fixed this big time.]**

Calling a method that you defined outside of a class can easily [become ugly](http://rt.perl.org/rt3/Ticket/Display.html?id=58138). (Easily avoided, to be sure, but should it be allowed at all?) **[Update 2008-09-12: Fixed in r30990. jonathan++]**

Planning to use a regexp match in the condition of an `if` or a `while`, and then pull things out of `$/` inside the block? Sorry, [you](http://rt.perl.org/rt3/Ticket/Display.html?id=57858) [can't](http://rt.perl.org/rt3/Ticket/Display.html?id=58352), at least not without matching again inside the block. If you have nested blocks, you'll have to match once in every new block where you plan to use `$/`. (moritz prefers to [bind](http://irclog.perlgeek.de/perl6/2008-08-25#i_445858) the match variable instead of matching again.) A "tax" you have to pay as long as this bug persists, putting duplicated matches at the start of blocks makes your code ugly. **[Update 2008-09-11: Fixed in r30987. jonathan++]**

## The weird

Once you reach a certain code size, the compiler will greet you with frequent "Segmentation fault" and "Bus error" messages, even for correct code. Remove a comment, and your code compiles again. Half an hour later, it collapses again; add an empty print statement, and you're good to go again. Effects like this clearly fall in the "weird" department. The will also most likely be sorted out as Parrot's garbage collector grows more stable. **[Update 2009-03-01: I'm going ahead and marking this as fixed, since a big "double free" segfault was found by jonathan++ and fixed by him and pmichaud++ the other day. Haven't had a "Bus error" in months, I think.]**

Having a subroutine calling itself recursively inside a `for` loop will summon some [strange bugs](http://rt.perl.org/rt3/Ticket/Display.html?id=58392) indeed. What's even stranger, this bug appeared in a cleanup commit to Rakudo a couple of weeks back, a commit that shouldn't have had any effect on programs at all. On the other hand, the behaviour *before* the commit was to do nothing whatsoever... so maybe this never worked. Weird. **[Update 2009-03-01: But now it works, thanks to massive work by pmichaud++.]**

## In conclusion

Play with Rakudo today! Frolic like a newborn calf in the good, the cool and the awesome. If you find something bad, ugly, or weird, send us a message via [#perl6](http://irclog.perlgeek.de/perl6/today) or [rakudobug](mailto:rakudobug@perl.org) — think of it as your way of bringing Christmas one step closer.

Enjoy!


