---
title: Adding 'goto' to your Perl 6 program
author: Carl Mäsak
created: 2010-01-08T06:22:00+01:00
---
Today I tested something that jnthn++ and I had discussed during a walk in thenon-tourist parts of Riga after Baltic Perl Workshop.

    $ cat test-goto
    Q:PIR {
      line_10:
    };
     
    say "OH HAI!";
     
    Q:PIR {
      goto line_10
    }
     
    $ perl6 test-goto
    OH HAI!
    OH HAI!
    OH HAI!
    OH HAI!
    [...]

Oh. My. Wow.

The 1980's called; they want their infinitely looping toy BASIC idiom back.

I half-expected that not to work, but I'm glad it does. I can even imagine it being of actual, code-simplifying use in some applications. The reports of the harmfulness of GOTO have been greatly exaggerated, if you ask me. Like everything else, the `goto` keyword shouldn't be overused, but a well-placed `goto LABEL` can actually improve readability. Often these masquerade as `next LABEL` or `last LABEL` or `redo LABEL` in Perl loops. But those are `goto`s with a nicer accent, a briefcase, and a better salary.

Unfortunately, the trick doesn't take us very far. Since we're using PIR `goto`s, we can only jump around within the same sub. Not just the same Perl 6 sub, that is, but the same PIR sub. Since every block in Perl 6 corresponds to a sub in PIR, we can't jump outside of the block.

    $ cat test-goto-loop
    loop {
        say "OH HAI!";
        Q:PIR {
          goto line_10
        }
    }
     
    Q:PIR {
      line_10:
    };
     
    $ perl6 test-goto-loop
    e_pbc_emit: no label offset defined for 'line_10'
    in Main (file <unknown>, line <unknown>)

Well, that certainly makes it less useful. Shame.

Now, how about them PIR-based continuations...? ☺


