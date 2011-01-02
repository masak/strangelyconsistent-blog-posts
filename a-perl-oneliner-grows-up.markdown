---
title: A Perl one-liner grows up
author: Carl Mäsak
created: 2009-01-03T15:12:00+01:00
---
I wanted an overview of S29. More exactly, I wanted a list of functions. The table of contents in [S29 itself](http://perlcabal.org/syn/S29.html) didn't cut it, because it stopped short at the level of classes, and left out the functions. That's when I decided to extract the relevant information using a script of my own. I mean, how hard can it be?

Turns out it was easy.

Duh, right? We have *Perl* this is what Perl *does*: extract and report. (Practically.) Because it turned out to be a three-minute one-liner in Perl 5, I decided to turn it into a Perl 5 script file, and then into a Perl 6 script file.

The results were a bit interesting, and I thought I'd share.

First, a description of the problem:

- S29 is a POD file, which means we're in luck: all the directives we need to extract are on their own lines.
- Furthermore, all the classes are headed by `=head2 $class`, and the functions by `=item $function`.
- There's some `=over 4` and `=back` stuff that we don't care about.
- Oh, an the action doesn't start until after the line `=head1 Function Packages`.
- Now, produce a list of functions, indented under their respecitve class.

Here's my one-line solution:

    $ perl -e '$_ = <> until /Function Packages/; while (<>) { if (/^=(\S+) ↩
    (.*)/) { next if $1 eq "over"; print " " x 4 if $1 eq "item"; print $2,↩
    "\n" } }' S29-functions.pod

If you're interested in the output it produces, try running it after `cd`-ing into the directory `docs/Perl6/Spec` in the [Pugs repo](http://svn.pugscode.org/).

Here's the corresponding turned-into-a-file script (still Perl 5):

    use strict;
     
    open my $S29, '<', 'S29-Functions.pod' or die $!;
     
    until (<$S29> =~ /Function Packages/) {}
     
    while (my $line = <$S29>) {
        if ($line =~ / ^ = (\S+) ' ' (.*) /x) {
            next if $1 eq 'over';
            if ($1 eq 'item') {
                print ' ' x 4;
            }
            print $2, "\n"
        }
    }

Things are a bit more indented, a bit more Best Practices, but no revolutionary changes. It's the same script.

Now, take a look at the Perl 6 script:

    use v6;
     
    my $S29 = open('S29-Functions.pod', :r) or die $!;
     
    repeat {} until =$S29 ~~ /'Function Packages'/;
     
    for =$S29 -> $line {
        if $line ~~ / ^ '=' (\S+) ' ' (.*) / {
            next if $0 eq 'over';
            if $0 eq 'item' {
                print ' ' x 4;
            }
            say $1;
        }
    }

Noteworthy differences:

-  `use strict` is implicit in `use v6`.
- The `open` syntax is different. I stumbled on this, and had to go look in a spectest file to remind myself.
- I use `repeat {} until $cond` unstead of `{} until $cond`, because I think it looks better. The old way still works, too.
- The `while` loop is a `for` loop. Beware, young Perl 6 hatchlings! In Perl 6 land, we `for` loop over our files and other stream-like things. That way, we don't have to do an assignment (explicit as in my Perl 5 script, or implicit with `$_`). Things are more lazy in Perl 6, so the `for` loop doesn't cause the file to be slurped or anything like that. (For what it's worth, I forgot to use a `for` loop and used a `while` loop instead, with segfaults as a result. That was an indication that I was doing something wrong.)
- No parens are needed after `for` and `if`.
- Perl 6 regexes are automatically `/x`.
- The family of `$1, $2, ...` match variables are pushed down one step. For great justice. And consistency.
- I'm not sure it's kosher to use `say` like I did (i.e. to finish off a line that has been printed in several stages), but it seemed nice, so I decided to give it a try. So far, I like it.

There you have it. A Perl one-liner grows up.


