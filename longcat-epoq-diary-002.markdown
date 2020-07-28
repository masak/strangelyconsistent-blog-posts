---
title: Longcat (epoq diary 002)
author: Carl Mäsak
created: 2020-07-28T22:32:00+08:00
---
I just need to share this cute story.

A few weeks back, my son and I discovered [Fancade](https://www.fancade.com/).
Highly recommended, this iOS app consists of a number of smaller subgames.
As far as I understand, the user can even create their own levels (although we haven't been able to do this yet, possibly due to regional restrictions).
All in all, it looks like a thriving little platform, with a nice solid basis of built-in games, and lots of user contributions.

We're only at the beginning so far.
But we've already steered around the following things:

* a cute little cuboid robot (in a world which somewhat resembles Monument Valley),
* a number of badly constructed cars (jumping on platforms to reach a goal), and
* LongCat.

LongCat quickly turned out to be my favorite.
It's one of those "a minute to learn, a lifetime to master" kind of games.
Here's a typical level from the game:

<pre><code>#######
#     #
#     #
# C   #
#   # #
#     #
#######</code></pre>

Ok, so that's the cat there (`C`), and an internal wall.
Now here are the rules:

* You can control the head of the cat by pressing up/down/left/right.
* The head of the cat in the chosen direction, leaving a "cat tail" in its wake.
  Both the head and the cat tail are considered to be _occupied_ cells, just like internal walls.
* The head of the cat moves _as far as it can_ in the chosen direction, until it hits something.
* Them's the breaks. Now fill the level with occupied cells.

Incidentally, we got stuck at the level depicted above.
I think it was level 15, but I'm not sure.
We tried again and again and again, until it felt like there really wasn't a way to solve this level.

Eventually I told my son "hold on, let me try a thing".
I broke out an old code base, which has a _solver_ for generic puzzle-like problems.
Here is the solver in its entirety, written in TypeScript:

<pre><code>function solve &lt;State&gt; (puzzle: Puzzle&lt;State&gt;): void {
    let queue = [{ state: puzzle.initialState, trace: [puzzle.initialDescription] }];
    let queuedSet = new Set([hash(puzzle.initialState)]);

    let elem: { state: State, trace: string[] };

    while (elem = queue.shift()!) {
        let { state, trace } = elem;

        if (puzzle.winningCondition(state)) {
            for (let step of trace) {
                console.log(step);
            }
            console.log(puzzle.winningDescription);
            return;
        }

        for (let { newState, description } of puzzle.validMoves(state)) {
            if (queuedSet.has(hash(newState))) {
                continue;
            }
            queue.push({
                state: newState,
                trace: [...trace, description],
            });
            queuedSet.add(hash(newState));
        }
    }

    console.log("There's no solution.");
}</code></pre>

I love this code.
I'm proud of it.
Every line of it is somehow in support of the narrative.

Specifically, this is a _general problem solver_.
I can't emphasize this enough.
If you have a certain type of problem (I'll get to the specifics of this) you can enter it into this function, and it will spit out a solution.
There's something about this arrangement that just thrills me to no end.
I think it has something to do with an old, already-antiquated view of computers: as some huge behemoth that you feed a data tape with a problem, and it spits out another data tape with a solution.
Computers no longer feel that way, but... this function kinda feels that way, I think.

The function essentially says this:

* Starting from an initial state,
* follow all the valid moves from each state,
* until a solution is found.
* At that point, output all the moves that led to that solution.

There's nothing really new or revoluationary about any of this.
What we are doing is just executing a Breadth-First Search on the implicit graph formed by all the possible states and their "valid move" state transitions.
My repository is even called [puzzle-bfs](https://github.com/masak/puzzle-bfs) for this reason.

The nice thing, if I were to try and put it into words, is that the above function manages to make a pretty clean separation between the _generic_ problem solving and the _specifics_ of the problem itself.
That is, the function manages to solve _any_ problem of this kind, without knowing the specifics of the problem.
I guess it just feels like an especially successful factoring.

Again, this solver is something I wrote two years ago and haven't thought much about since.
Now, suddenly, I turned to it to help solve this seemingly impossible LongCat level.

Here's how I coded up the problem:

<pre><code>interface Level {
    catX: number;
    catY: number;
    sizeX: 5,
    sizeY: 5,
    level: boolean[][];
}

function clone2dArray(array: boolean[][]) {
    return array.map((row) =&gt; [...row]);
}

function moveUp(state: Level) {
    let { catX, catY, level } = state;
    level = clone2dArray(level);
    while (catY &gt; 0 && !level[catY-1][catX]) {
        catY--;
        level[catY][catX] = true;
    }
    return { ...state, catY, level };
}

function moveDown(state: Level) {
    let { catX, catY, level, sizeY } = state;
    level = clone2dArray(level);
    while (catY < sizeY - 1 && !level[catY+1][catX]) {
        catY++;
        level[catY][catX] = true;
    }
    return { ...state, catY, level };
}

function moveLeft(state: Level) {
    let { catX, catY, level } = state;
    level = clone2dArray(level);
    while (catX &gt; 0 && !level[catY][catX-1]) {
        catX--;
        level[catY][catX] = true;
    }
    return { ...state, catX, level };
}

function moveRight(state: Level) {
    let { catX, catY, level, sizeX } = state;
    level = clone2dArray(level);
    while (catX &lt; sizeX - 1 && !level[catY][catX+1]) {
        catX++;
        level[catY][catX] = true;
    }
    return { ...state, catX, level };
}

let puzzle4: Puzzle&lt;Level&gt; = {
    initialDescription: "The cat is in its starting position.",
    initialState: {
        catX: 1,
        catY: 2,
        sizeX: 5,
        sizeY: 5,
        level: [
            [false, false, false, false, false],
            [false, false, false, false, false],
            [false, true, false, false, false],
            [false, false, false, true, false],
            [false, false, false, false, false],
        ],
    },

    validMoves: (state: Level) =&gt; [
        {
            description: "Move up.",
            newState: moveUp(state),
        },
        {
            description: "Move down.",
            newState: moveDown(state),
        },
        {
            description: "Move left.",
            newState: moveLeft(state),
        },
        {
            description: "Move right.",
            newState: moveRight(state),
        },
    ].filter(({ newState }) =&gt; newState.catX != state.catX || newState.catY != state.catY),

    winningDescription: "The whole level is filled.",
    winningCondition: (state: Level) =&gt; state.level.every((row) =&gt; row.every((cell) =&gt; cell)),
};

solve(puzzle4);</code></pre>

I did this essentially with my son watching.
(He's five, so I give him full points on patience; even though I was explaining along the way, it was a bit too much code for his wish for instant gratification.)
Then we run it, and the Big Machine printed this output:

<pre><code>The cat is in its starting position.
Move left.
Move down.
Move right.
Move up.
Move left.
Move down.
Move right.
Move down.
Move left.
Move down.
Move left.
The whole level is filled.</code></pre>

Personally I was a little bit stunned that it had worked at all.
Next up, we tried these stepwise instructions on level 15, and...

...they worked!
The puzzle solver had solved our puzzle!

Man, that felt good! 哈哈

This is what computers are meant to be like, all the time!
You feed them an apparently impossible problem, and they immediately spit out a nice solution.
Your live quality improves, and you tip your hat to the Big Machine and get on with your day.

Other takeaways:

* TypeScript is really nice when you use it in the right way.
  Specifically, this feels like a really nice mix of generics, interface types, and concrete objects.
  It's a very modern language, and it's wonderful to have it on the web and in Node.js.
  When writing it, I do feel I get away with "just enough typing".

* A puzzle in this world has five properties: `initialDescription`, `initialState`, `validMoves`, `winningDescription`, and `winningCondition`.
  As I remember, it took me several iterations to arrive at exactly that.
  In my experience, the work to arrive at something nice like that is often hidden, and only the final "polished" result is visible.
  And yet, it takes quite a bit of effort to make something like natural and simple.

* Specifically, I think initially I had the notion of "losing moves" in my model.
  It took me a few iterations to realize that a losing move doesn't need to be its own thing;
  it's just a move that doesn't fulfill the winning condition, and doesn't generate any new moves.
  The solver will just naturally discard it.

* I used to say in the Architecture course that "a good design is one that's future-compatible".
  That is, tomorrow's needs are unknown today, but if we're skilled and lucky we can still design an interface that can stay compatible with tomorrow's needs.
  I believe this was an example of that; the LongCat puzzle could be stated simply as another instance of a puzzle, even though I didn't know about it two years ago.

I have this other simmering idea, which I might as well mention: that of taking this solver, and plugging it into a bigger system, one which can _generate puzzles_.
Because when you think about it, being able to solve these puzzles is a modular subpart of being able to _generate_ puzzles in the first place.
When you generate these puzzles, it's good to know that a puzzle has a _unique_ solution (maybe up to some isomorphism).
Unique solutions are not an absolute requirement, but they do make the puzzle itself nicer.

Other desirable properties include:

* The thing where several elements of the puzzle "interfere" with each other in some way.
* The thing where you have to "backtrack" in some non-obvious way in order to solve the problem.
* The thing where trying to solve the problem in the "obvious" way fails, and you have to go back to the drawing-board.

My feeling is that all of the above properties can be _encoded_, maybe not in a generic way but at the very least for each particular problem domain.
And at that point, we could essentially churn out interesting puzzles/levels to solve.

For a while now, I've been wanting to try that for both [Theseus](https://apps.apple.com/us/app/theseus-plus/id1047862647) and [Little Red Riding Hood](https://www.smartgames.eu/uk/one-player-games/little-red-riding-hood-deluxe), both of which I think would yield interesting new levels. Quite possibly Sokoban would also be fun to try this on.
