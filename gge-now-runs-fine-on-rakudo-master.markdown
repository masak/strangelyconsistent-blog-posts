---
title: GGE now runs fine on Rakudo master
author: Carl Mäsak
created: 2010-05-31T17:44:00+02:00
---
After [admitting that I had a problem](http://strangelyconsistent.org/blog/step-1-on-the-road-to-recovery-admitting-you-have-a-problem), I decided that perhaps it would be good to take one single application and just beat it into submission, making it run on Rakudo master.

I chose [GGE](http://github.com/masak/gge). That was a mistake, but perhaps a positive one.

GGE is *big*, and has many tests, 750 of them. Running through them all takes almost 40 minutes. (The first "G" in GGE stands for "Glacial".) I keep thinking of GGE is "the biggest Perl 6 application out there", even though it's probably not that big, just fairly complex and well-tested. The mistake was attacking GGE first, and not something smaller.

The way the mistake might turn out to be a positive one is that I'll probably feel that converting other applications from alpha to master is relatively painless. 哈哈

Here's a summary of things to think about, if you're planning to port your Perl 6 application from alpha to master:

## Improvements

- Rakudo master has better interpolation of things like `"@lol[]"`. I found a lot of cases where I "cheated" and used symbols which weren't magical in alpha, but which are now. Beware.
-  `is also` is now `augment`, and requires `use MONKEY_TYPING;`.
- If you had put `sub`s in your class, chances are you'll need to `our` them now.
- Rakudo master is stricter about not changing the *contents* of readonly containers, such as arrays and hashes. You'll get a slap on the fingers if you did this.
-  `undef` is gone. You probably meant `Nil` or `Any` or something.
- If you were using `postcircumfix:<( )>`, remember that Rakudo master now requires the parameter list to consist of just a single Capture parameter. In practice, that means another layer of parentheses compared to alpha.
-  `break` in `when` blocks is now called `succeed` (and `continue` is called `proceed`)

## Regressions

- Named enums work in master, but they're not really up to spec yet. If you used them as sets of constant, things might still work. If you used methods like `.name` and `.pick`, you'll need to [work around](http://rt.perl.org/rt3/Ticket/Display.html?id=75296) for the time being. (And re-read the spec.)
- The `<->` syntax for pointy blocks [is no longer implemented](http://rt.perl.org/rt3/Ticket/Display.html?id=74182).
- Backslash escapes [don't work](http://rt.perl.org/rt3/Ticket/Display.html?id=73698) in character classes.
-  `Str.trans` doesn't work.
-  [ `handles` ](http://rt.perl.org/rt3/Ticket/Display.html?id=75386) doesn't work.
- Using an optional hash parameter with `is copy` semantics? Well, [it won't work](http://rt.perl.org/rt3/Ticket/Display.html?id=74454). Sorry.
-  `Str.subst` works, but using `$_` in a closure in its second argument doesn't.
- Sometimes you have to clone closures using `pir::clone`, or [they won't close properly](http://rt.perl.org/rt3/Ticket/Display.html?id=73034) around their surrounding environments.
-  `"\e"` [doesn't work](http://rt.perl.org/rt3/Ticket/Display.html?id=75244) at the moment. Use `"\c[27]"` or `"\x[1b]".`
- If you were general before and wrote `List` rather than `Array` because, well, it's shorter and more inclusive... that won't work now. `Array` no longer subclasses `List`.
- There are some situations when you might wind up with [double [] in arrays.](http://rt.perl.org/rt3/Ticket/Display.html?id=74336)
- There's something wrong with [list assignment and hashes](http://rt.perl.org/rt3/Ticket/Display.html?id=74302).

It took two months of leisurely refactoring and debugging to bring GGE through the needle's eye. But I must confess that it was pretty sweet to subsequently rid it of all of alpha's workarounds. Clearly a net win.


