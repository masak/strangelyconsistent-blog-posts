---
title: Macros: allowing two terms in a row
author: Carl Mäsak
created: 2014-10-18T23:15:32+02:00
---
Perl 6 syntactic constructs are privileged in that they can break the TTIAR rule in Perl 6.

    if 2 + 2 == 4 {
        say "math!";
    }

What's "TTIAR"? The acronym stands for "Two Terms In A Row", and the parsing tradition behind it all actually goes back all the way to Perl 5. When the Perl 5 parser sees a `/`, how does it know whether it's a division operator, or the start of a regex literal?

The first part of that answer is that the parser constantly keeps track of whether it's expecting a term or an operator. And so it can tell those two things apart just fine. The second part of the answer involves all the things that participate in that dance between expect-term and expect-op: everything from prefix and postfix ops to so-called listops. A few examples help show the complexity of this dance:

    2 + 3 * 4;               Ⓣ2 Ⓞ+ Ⓣ3 Ⓞ* Ⓣ4 Ⓞ;
    $x++ + ++$y;             Ⓣ$x Ⓞ++ Ⓞ+ Ⓣ++ Ⓣ$y Ⓞ;
    say "OH HAI";            Ⓣsay Ⓣ"OH HAI" Ⓞ;

The concept of "TTIAR" says that there is a rule the code author cannot break: it's not allowed to put a term where the parser expected an operator.

    "will " "concatenate?";  Ⓣ"will " Ⓞ<<<PARSE ERROR>>>

This is all good and well, and works to our advantage. TimToady has described it on several occasions as Perl 6's "self-clocking" mechanism: if a term shows up where an operator was expected, we give a parse error. Often we're able to give a more specific parse error than just "Confused" &mdash; in fact, many of the excellent errors we give are improved versions of the TTIAR error.

    # excerpt from inside of the `panic` method in STD.pm6
    if self.lineof($startpos) != self.lineof($endpos) {
        $m ~~ s|Confused|Two terms in a row (previous line missing its semicolon?)|;
    }
    elsif @*MEMOS[$startpos]<listop> {
        $m ~~ s|Confused|Two terms in a row (listop with args requires whitespace or parens)|;
    }
    elsif @*MEMOS[$startpos]<baremeth> {
        $here = $here.cursor($startpos);
        $m ~~ s|Confused|Two terms in a row (method call with args requires colon or parens without whitespace)|;
    }
    elsif @*MEMOS[$startpos]<arraycomp> {
        $m ~~ s|Confused|Two terms in a row (preceding is not a valid reduce operator)|;
    }
    else {
        $m ~~ s|Confused|Two terms in a row|;
    }

Ok, great. So, back to the `if` statement from the top. It breaks TTIAR.

    if 2 + 2 == 4 {          Ⓣif Ⓣ2 Ⓞ+ Ⓣ2 Ⓞ== Ⓣ4 Ⓞ{

That last brace there introduces a block, which is a term. (A big one.) But as you see, the parser is in op-expecting mode. Boom... no, not boom! Just a regular day at the `if` statement parsing assembly line. The parser, instead of throwing a hissy fit about a term that's out of line, just turns around real quick and goes "oh, right! this is an `if` statement, so it's *fine*". (In fact, this little hiccup is the secret sauce that allows Perl 6 to drop the parentheses in `if` statements.)

The same goes for all the big-shot block-accepting control statements out there: `unless`, `while`, `until`, `repeat`, `for`, `given`, and `when`. They *flaunt* the rule, practically laughing in the face of the 99%, the less fortunate grammatical constructs who have to get all their terms and operators in the accepted order. [Ha ha!](https://www.youtube.com/watch?v=rX7wtNOkuHo)

I don't know about you, reader, but I think this tyranny should end. We should put the power of TTIAR breakage into the hands of the author, where it belongs. They clearly belong in macros. In fact, they shouldn't be allowed to *call* themselves macros if they couldn't do this, and emulate an `if` statement. In my opinion.

People have a standard response to this, and I think it's problematic as it stands. They say "well, that's OK; for such macros we just have to use the `is parsed` trait. That way, the macro can take over for a while from the compiler's parser, and supply its own, up to and including breaking TTIAR to its heart's content."

And yes, maybe that is the solution. I hope it can be. But not as it stands today. Let's look at the *only* example in S06 that declares a macro with `is parsed`:

    macro circumfix:«<!-- -->» ($text) is parsed / .*? / { "" }

I personally hope that this particular declaration will never work in Perl 6. If I squint, I can... sort of... see how it would work out. Ok, so the parser comes and finds a circumfix in an expression, yes. Maybe the expression is this:

    2 + <!-- lol an SGML comment --> 2

*Somehow* that regex `/ .*? /` gets tried again and again until it matches the whole comment, right? (Bear with me here.) The macro, full of mirth, returns the empty string for the parser to re-integrate into the source code. The parser is then apparently supposed to go "oops! what a great macro that was! but even though I am now after a `circumfix` call, which would normally have me hungry for an operator, it left me with nothing, which must *obviously* mean I'm now *back* to expecting a *term*! Yeah, that's a healthy way to modify code, oh boy!"

In other words, the one spec'd example we have of the `is parsed` trait runs on 80% magic and 20% wishful thinking. (And apparently as implementor, my way to cope is to get really sarcastic. I leave the mocking of the `is parsed` macro in [E06](http://www.perl6.org/archive/doc/design/exe/E06.html) as an exercise to the reader.)

Here's what I think is going on. In the latter half of the naughties, we got amazing STD parser technology. We basically figured out how Perl 6 is parsed. The macro spec (and the `is parsed` trait) largely comes before that. The Perl 6 chorus today sings about *grammars*, and sometimes action methods. But the `is parsed` trait still mumbles about its regexes, making itself a bit of an embarrassment, to be honest. It hasn't gotten the memo that all the rest of us are doing structured language parsing, not just text munging.

What if when I declared a macro, I got the option to play the same game as `if` and `for` and the other big cats? What if I got to effectively extend the current Perl 6 grammar being parsed? (This is also the goal of slangs.) I think a lot of the problems would be solved simply with addition.

## Implementation

At the point the macro is parsed, but before parsing its arguments, we need to give the macro a chance to do its own parsing. This parsing should be able to call into methods declared in the Perl 6 parser/grammar.

Let's look at some of the statement control rules in STD.pm6 to get a feel for whether this is realistic. Here's `if`, for example:

    rule statement_control:if {
        <sym>
        <xblock>
        [
            [
            | 'else'\h*'if' <.sorry: "Please use 'elsif'">
            | 'elsif'<?keyspace> <elsif=.xblock>
            ]
        ]*
        [
            'else'<?keyspace> <else=.pblock>
        ]?
    }

(Here, `<xblock>` expects an expression and a block (that's the TTIAR breakage right there), and `<pblock>` expects a block with a possible "pointy" `->` parameter declaration on it.)

I don't know about you, but this way of specifying how Perl 6 code should be parsed feels at the same time very uncluttered, natural, and overall a good fit. There's no waste here. Some error handling, but that's all. In terms of bang-for-the-buck, we're doing very well.

`while` statement?

    rule statement_control:while {
        <sym>
        [ <?before '(' ['my'? '$'\w+ '=']? '<' '$'?\w+ '>' ')'>   #'
            <.panic: "This appears to be Perl 5 code"> ]?
        <xblock>
    }

Again, yes. Couldn't be much simpler. `given`?

    rule statement_control:given {
        <sym>
        <xblock>
    }

The paragon of simplicity. It's a symbol, and then an expression-block. What about `when`?

    rule statement_control:when {
        <sym>
        <?dumbsmart>
        <xblock>
    }

Again, that's exac... hey! Who are you calling dumbsmart? What did I ever do to you, `when`?

An `is parsed` macro could perhaps look something like this (running with the example from [the last post](http://strangelyconsistent.org/blog/macros-nesting-macros)):

    macro transact is parsed / <sym> <xblock> / {
        # mumble handwave need to extract $conn and &block from <xblock>
    }

Inside the macro, we'd have access to the parse tree thus generated. Probably also the regular action methods of all the rules that fired should also run as they usually do during a parse. Which means that we'll have a full AST built too, QAST and all. Here my crystal ball grows blurry, because we already said we don't want to be interacting with the QAST.

We'll have to attack this in a later post. But we're already in a better position here than when we started; we can now harness the power of the Perl 6 grammar from our macro.

If after that, it turns out that it's *still* unnecessarily cumbersome to specify something as simple as the `transact` macro above &mdash; if it feels like writing boilerplate every time we do that &mdash; then clearly we should find some sugared way to write it so as to simply express "keyword, expression, block &mdash; you know what to do". It goes without saying that, for dogfooding reasons, that type of sugar should be provided through macros, maybe in module-space.

## Not addressed by this proposal

Identified in [a previous post](http://strangelyconsistent.org/blog/macros-progress-report-after-a-long-break).

* The `{{{ }}}` syntax being universally hated
* Quasi slices only being usable in term position
* Macro paramters/operands being restricted to expressions
* Manipulexity of program elements
