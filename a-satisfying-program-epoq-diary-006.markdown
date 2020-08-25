---
title: A satisfying program (epoq diary 006)
author: Carl Mäsak
created: 2020-08-25T23:10:00+08:00
---
A month or so ago, I finally got [Knuth's fascicle 6](https://www-cs-faculty.stanford.edu/~knuth/fasc6a.ps.gz) in book form, and digested it over the course of two-three weeks.
The whole book (thinner than a regular TAoCP volume as it weighs in at 310 pages, since it's only _part_ of Volume 4) is about _satisfiability_, a topic I was aware of but didn't know so much about before.

Satisfiability has the appearance of a stereotypical 1950's computer problem:
Given a _formula_, the computer is meant to find values to plug into the formula to make it true.
(In other words, it's a search in the space of all possible combinations of values for one lucky combination.)

It's not just _any_ kind of formula &mdash; it's a boolean formula of a certain shape.
On the top level, there can only be `&&` (and) operators, on the second level there can only be `||` (or) operators, and on the third level we have our variables, optionally negated with `!` (not) operators.
That's it.

Oh, an example? The one given just at the start of the book is this one:

<pre><code>(x1 || !x2) &amp;&amp; (x2 || x3) &amp;&amp; (!x1 || !x3) &amp;&amp; (!x1 || !x2 || x3)</code></pre>

If it were your job to satisfy the above formula, you could do it &mdash; after all, there are just 2*2*2 combinations of true/false values three boolean variables could have.
But strong believers in progress and efficiency as we are, we throw such problems to the electronic brain.
Get with the times, progress marches on!
If we're lucky, we can stand and watch while lights blink seductivly on the front panel, and maybe some magnetic tape gets spun around too.

(The solution, in case you care, is `x1 = false, x2 = false, x3 = true`.)

Knuth quickly describes how doing such a search takes _forever_ (or, more precisely, its worst-case time is exponential in the number of variables; every new variable doubles the search space).
But on the other hand, he writes, "enormous technical breakthroughs in recent years have led to amazingly good ways to approach the satisfiability problem".
The rest of the fascicle concerns itself with these recent techniques.

The whole text has a little bit of everything.
It's filled with diverse puzzles and problems, from Game of Life (running it backwards!) to Minesweeper (solving it perfectly!) to various letter puzzles, chess problems, and graph problems.
Knuth shows this technique which magically opens the gate to solving a number of unrelated problems, while also explaining what little we know about the really good solvers.
His joy at doing so is palpable.

Oh, I forgot to handwave why this technique is so versatile.
Well, kind of similar to how everything is bits if you go far enough down the abstraction stack on a computer, we can turn most constraint satisfaction problems into satisfiability formulas.
Want to solve a graph-coloring problem?
How about I give you one boolean variable for each node/color pairing, sound good?
And then we can describe the rules that these must follow as clauses in the formula.

Later parts of the fascicle talk about the heavy-hitters, Algorithm L and Algorithm C.
I hope to implement them later, but making sure to begin on the ground floor, I started with Algorithm A.
This one does the simplest thing possible: search by brute force through the whole space.
Oof!
Well, gotta start somewhere.

In fact, I did even less than that.
Knuth's Algorithm A has _zero allocations_ once it gets going.
It's easy to see why that would be desirable: heap allocations kill performance, and we have an exponentially-sized job in front of us.
Knuth makes an algorithm which clearly takes a leaf from his [dancing links](https://arxiv.org/pdf/cs/0011047.pdf) algorithm (and he says as much himself in the book).
I decided to make life easier (but slower) for myself, and start with The Simplest Thing That Could Possibly Work.

The resulting code is small and neat.
I thought about commenting inline, but I'll let the code speak for itself and add some comments at the end:

<pre><code>type Variable = number;

type Literal = {
    variable: Variable,
    isPositive: boolean,
};

function literal(variable: Variable): Literal {
    return { variable, isPositive: true };
}

function negatedLiteral(variable: Variable): Literal {
    return { variable, isPositive: false };
}

function not(v: Literal): Literal {
    return {
        variable: v.variable,
        isPositive: !v.isPositive,
    };
}

type Clause = Literal[];

function clauseIsEmpty(c: Clause) {
    return c.length === 0;
}

type Formula = Clause[];

function formulaIsEmpty(F: Formula) {
    return F.length === 0;
}

function pickLiteralIn(F: Formula): Literal {
    return F[0][0];
}

function literalNotEqualTo(l1: Literal) {
    return (l2: Literal) => (
        l1.variable !== l2.variable
        || l1.isPositive !== l2.isPositive
    );
}

function doesNotContain(l: Literal) {
    return (C: Clause) => C.every(literalNotEqualTo(l));
}

function removeLiteral(l: Literal) {
    return (C: Clause) => C.filter(literalNotEqualTo(l));
}

function formulaGiven(F: Formula, l: Literal): Formula {
    return F.filter(doesNotContain(l))
        .map(removeLiteral(not(l)));
}

type Solution = Literal[];
type NoSolution = undefined;

function emptySolution(): Solution {
    return [];
}

function noSolution(): NoSolution {
    return undefined;
}

function isSolution(L: Solution | NoSolution): L is Solution {
    return typeof L === "object";
}

function unionSolution(L: Solution, l: Literal): Solution {
    return [...L, l];
}

function satisfy(F: Formula): Solution | NoSolution {
    if (formulaIsEmpty(F)) {
        return emptySolution();
    }
    else if (F.some(clauseIsEmpty)) {
        return noSolution();
    }
    else {
        let l = pickLiteralIn(F);
        let L = satisfy(formulaGiven(F, l));
        if (isSolution(L)) {
            return unionSolution(L, l);
        }
        else {
            L = satisfy(formulaGiven(F, not(l)));
            if (isSolution(L)) {
                return unionSolution(L, not(l));
            }
            else {
                return noSolution();
            }
        }
    }
}

function parseClause(signedNums: number[]): Clause {
    return signedNums.map((n) => {
        return n &gt; 0
            ? literal(n)
            : negatedLiteral(Math.abs(n));
    });
}

function parseFormula(...clauses: number[][]): Formula {
    return clauses.map(parseClause);
}

let F: Formula = parseFormula(
    [1, -2],
    [2, 3],
    [-1, -3],
    [-1, -2, 3],
);

console.log(satisfy(F));</code></pre>

* If you're looking for where my expensive copying is done, it happens in `removeLiteral`, `formulaGiven`, and `unionSolution`.
* Knuth's data structure is slightly bigger, but he gets by with _zero_ allocations, just moving a lot of pointers around super-quickly. They're not even pointers, they're small integers.
* Oh, and my solution is recursive (on the variables), but Knuth "unrolls" this recursion into a kind of state machine [like I enjoyed doing here](https://github.com/masak/taocp/tree/dc4826f1f99dc0b0fcad35a5aeb606e0f81b73f7/ch2.3.1-algorithm-t) with another tree traversal. Anyway, the reasons are similar: it's more efficient not to make calls.
* If you compare the `satisfy` function with the "recursive procedure" description on page 27, you'll see that they match up exactly.
* Writing this out as TypeScript was really pleasant and ergonomic. I like TypeScript. TypeScript is the hero Gotham needs.
* `pickLiteralIn` does the Simplest Thing Possible. Algorithm A in the book is slightly more sophisticated, and picks the negated literal if it occurs more often.
* I keep thinking there should be a nice illustrative way to transform my version to Knuth version, preferably in small steps. When I figure out how to do that, I might write about it.

There are so many fun small problems in this little book.
If you want something to chew on, check out Exercise 2 &mdash; it's about dancing aliens.
It can be solved with pen and paper, or in my case, Notepad on my phone.
