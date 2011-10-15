---
title: Macros &mdash; what are they, really?
author: Carl Mäsak
created: 2011-10-15T16:13:00+02:00
---
Apparently, if you schedule all of my talks at YAPC::EU 2011 on the first day, I will spend the remaining time of the conference thinking intently about how macros work. (I did some socializing too, don't worry. I even distinctly remember talking to people about other things than macros on at least one occasion.)

Like most of the rest of you, I'd heard about C preprocessor macros (and how they're both useful and kinda dangerous if you don't know what you're doing), and [Lisp macros](https://c2.com/cgi/wiki?LispMacro) (and how they're part of what makes Lisp the awesomest programming language in the universe forever). Which one of these types does Perl 6 specify?

Both, duh. 哈哈

But I'm going to talk about the latter kind. I'll call them "AST macros", to differentiate them from "textual macros". ("AST" simply means ["Abstract Syntax Tree"](http://en.wikipedia.org/wiki/Abstract_syntax_tree). Forget the "abstract" part, it's just been put there to scare you into thinking this is tricky.)

When the complexity of a codebase increases, it inevitably [becomes a part of them problem it is trying to solve](http://steve-yegge.blogspot.com/2007/12/codes-worst-enemy.html). We need to combat the complexity in the code itself, and we need to start talking about the code in the code. There are three broad ways we can describe code:

* **Code as strings.** Humans can read it, but the computer can't, because it's just strings.
* **Code as ASTs.** It has a lot of structure, which both humans and computers can use.
* **Code as executable code objects.** It's opaque to humans, but the computer run it, because it's just code.

These three forms &mdash; string, AST, code block &mdash; reflect what a compiler does when it prepares your source code for execution:

* Code starts out as **text**, as source code.
* Compiler parses the text and builds an **AST**. (And some other information, such as a symbol table.)
* Compiler serializes the AST into executable **code**.

The *reason* the compiler takes the detour through ASTs when creating your code is that [trees are much easier to reason about and manipulate](http://strangelyconsistent.org/blog/its-just-a-tree-silly) than the "flat" representations of code. An AST contains a lot of explicit relations that don't stand out in the original or final, "flat" representations of code. ASTs can be manipulated, stitched together, optimized, etc. It's this strength that AST macros make use of.

Macros are a way to *transform code*. AST macros transform code by giving you the tools to *build your own AST*.

How to construct an AST in Perl 6? Using the `quasi` keyword:

    quasi { say "OH HAI" }

What this evaluates to is a `Perl6::AST` object holding a tree structure representing the program code `say "OH HAI"`. Exactly how that tree structure looks may or may not be implementation-dependent.

`quasi` stands for "quasi-quote", a concept invented by Quine, the logician, who liked to think about self-reference, paradox, and words starting with the letter Q. Just as we quote code with a string literal and the result is a `Str`, so we can quote code with the `quasi` keyword and the result, in the case of Perl 6, is a `Perl6::AST` object.

Macros work just like subroutines, but AST macros are expected to return a `Perl6::AST`. How the AST is created is the macro author's business. But we can use `quasi` to create them:

    macro LOG {
        quasi {
            $*ERR.say(DateTIme.now, ": some logging information here");
        }
    }
    
    # Meanwhile, later in the code:
    LOG();

You see, it looks just like a subroutine call. But the call is made by the *compiler*, not by the runtime as with ordinary subroutines. And the return value is a `Perl6::AST` object containing the code to print something to `$*ERR`.

But wait, there's more!

It's a pretty useless `LOG` macro that doesn't take an argument with a `$message`. We'll fix that. There's one twist, though: AST macros deal in AST, so the `$message` that gets passed to the macro won't be a `Str`. It'll be a `Perl6::AST`:

    macro LOG($message) {
        quasi {
            $*ERR.say(DateTime.now, ": ", {{{$message}}});
        }
    }
    
    LOG("Evacuation complete.");

When we call `LOG`, we do it with a `Str`, just as with a usual subroutine. The parser sees the string literal and does its thing with turning stuff into ASTs. The compiler then calls the macro with one argument: the resulting `Perl6::AST` object. In the quasi, we make sure to take this object and *stitch* it right into the code that says "print a bunch of stuff to `$*ERR`". It's right there, at the end of that line, enclosed in triple curly braces.

What do the triple curly braces do, exactly? They allow you to say "I want you to incorporate this already-parsed AST into this currently-being-parsed code". Triple curly braces are only recognized inside of quasi-quote blocks. In fact, this is what quasi-quotes specialize in: allowing an escape hatch from code to ASTs, so we can mix them. (This is what Quite used quasi-quoting for too, except in the domain of logic.)

Right, so AST macros take ASTs, allow us to manipulate ASTs, and return ASTs. Fine. We get the message. But what makes them so powerful?

The real power comes from the fact that we can steer this process any which way we want. For example, maybe we'd like to turn logging on and off at the switch of a constant:

    constant LOGGING_ENABLED = True;
    
    macro LOG($message) {
        if LOGGING_ENABLED {
           quasi {
               $*ERR.say(DateTime.now, ": ", {{{$message}}});
            }
        }
        else {
            quasi {}
        }
    }
    
    LOG(crazily-expensive-computation());

Turn `LOGGING_ENABLED` off, and the `crazily-expensive-computation()` call will be parsed, but never executed.

This is the essence of AST macros. There's much more to it than that, but we'll get to the other parts in later posts.
