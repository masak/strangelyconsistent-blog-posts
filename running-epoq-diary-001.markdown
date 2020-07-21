---
title: Running (epoq diary 001)
author: Carl Mäsak
created: 2020-07-21T21:15:30+08:00
---
I've taken up running in the mornings.
Well, in all honesty I run some mornings, others not.
This post is about motivation and data entry.

Right from the beginning I realized I needed some way to motivate myself to keep running.
Because of unrelated circumstances I had had exponentials on my mind, so I instituted the following points system for my running:

* I start out with 0 points.
* A _streak_ is any uninterrupted stretch of every-second-day running sessions. (Every two days feels about right to me.)
* The first running session in a streak gives 1 point.
* Each additional running session in a streak gives twice as many points as the previous.

That is, when successfully keeping up a streak, the running sessions score 1, 2, 4, 8, 16... points.
But if I miss a morning running session, I'm reset back to getting 1 point the next time I run.
The idea is to incentivize keeping a streak going.

Does this silly system of points work at all?
By way of answering that, I would say I have good news and bad news.

The good news: yes, it works, ridiculously well.
The points are entirely artificial, but once I get a good streak going, I'm _really_ motivated to keep it going, and not lose the streak.
Just as I had hoped, the motivator is a kind of weaponized sunk-cost fallacy.
Put into words, "If I stop running now, I won't the next reward: a ridiculous amount of points."

I'm happy to report I've been able to award myself 16384 points during one of the sessions!
Again, the points don't mean anything in particular, except that apparently I was able to run 15 times in a row, once every two days.

(You might well ask, is there a risk of some kind of "inflation" happening with the points?
I don't think there is, the main reason being that the points don't actually mean anything or convert to anything.
They are just used for motivation.)

The bad news?
I think I have discovered something one might call an "anti-streak"...

_Anti-streaks_ are nothing I designed into the system, and they seem entirely emergent.
While streaks are all powered by weaponized sunk-cost fallacy, the anti-streaks happen naturally, powered by... well let's just say the spirit is willing but the flesh is weak. (D'oh!)
Anti-streaks can even contain running inside of them, maybe one or two running sessions.
The inner motivational voice surveys the situation and goes, "If I stop running now, who cares? I can easily work my way back up to two or four points..."

While I'm quite proud of myself for my long streak earlier this spring, I don't yet have a fix for the (recently discovered) anti-streak effect.
Maybe there is no fix.
I don't want to mess with the rules of the points system, which have a certain clean simplicity to them.

<p class="separator">— ❦ —</p>

If you thought I was silly for having such a scoring system, wait until you hear about my data entry system.

There's an app on my phone called "Notepad".
I imagine such an app exists on most modern phones.
You can enter text into it.
Just like with the prototypical Notepad on Windows, you can't format the text or do anything fancy.
It suits me fine.

I enter each running session as one or two lines into a file in Notepad. Here are some exerpts:

<pre><code>2020-04-20: 1 point, total 16 points
  warmer today, but still bearable
2020-04-22: 2 points, total 18 points
  some rain drops, still very sweaty
2020-04-24: 4 points, total 22 points
  feel my knee a little bit, gotta be careful
... some entries omitted ...
2020-05-18: 16384 points, total 32782 points
  heavy rain outside, ran indoors</code></pre>

As ad-hoc textual formats go, I'm as pleased with this as I can be.
All running sessions in a streak are written together, without empty lines.
There's are empty lines between streaks.
This in itself creates an intuitive "visual pill" effect in the text; the streaks form paragraphs.
The indentation of the optional comment similarly makes each entry stand out a little bit.
The data is grouping itself naturally according to streaks.

I won't be asinine and say "the Notepad app sucks".
As far as I can tell, it doesn't suck &mdash; and although it doesn't quite do what I want, neither does any other app on my phone.

I just figure I can do better.

What would my favorite data entry look like in this case?
A button.
The button could say "Register run" or something like that.

After pushing the button, an entry has already been added, and

(a) a text field and a keyboard both pop up, and I'm encouraged (though not forced) to add a comment; if I do, the new running entry is updated to hold this comment.<br>
(b) there could also be a button to easily undo the creation of the new entry, just in case &mdash; mis-clicks do happen, and I've always liked Gmail's UX policy of "it's nicer to modelessly undo something after the fact than to issue an 'are you sure?' dialog box before".

As to the rest of the data, well, it's easily computable, right? The date, the points for this session, and the total points.
This is what a computer does all day, handle dates and do some simple logic and addition.

The part of me who is increasingly curious about relational modeling wants to map out this data model as two tables.
Here goes, in the form of a Postgres poem:

<pre><code>CREATE TABLE Streak (
    streak_id             SERIAL PRIMARY KEY
);

CREATE TABLE Session (
    session_id            SERIAL PRIMARY KEY,
    streak_id     integer REFERENCES Streak,
    date          date    NOT NULL,
    score         integer NOT NULL,
    description   text    NOT NULL
);</code></pre>

I'm a bit of a newbie with Postgres table definition syntax, but I don't think any of the above is all that controversial.

I initially put `total_score` in the `Session` table, but on further consideration, I think that's unwise.
It makes sense in my text file where I need to painstakingly accumulate that value by hand, but it doesn't make sense in a database table, in close proximity to a machine that can add together lots of numbers in a digital heartbeat.
It's usually a good idea to keep data normalized and avoid data dependencies between columns, just on general principle.

I have not created such an app for my phone yet.
This blog post now stands as a daring challenge to myself to do so.
To break free of the tyranny of the cute textual format, the manual computations, and the redundant data entry.
To emerge into a world where the typical use case is just the push of a button.

And... that's that.
I don't have a grand punchline, except to say that I'm very happy that I've managed to trick myself into running.
Here's hoping that I manage to find myself in a new streak soon, to end this annoying anti-streak...

<p class="separator">— ❦ —</p>

_P.S. I'm not good at blogging lately.
I'm hoping to change that.
This entry says "(epoq diary 001)", so I guess I need to say something about what "epoq" is._

_Here's what: I don't quite know.
Or, I do know, but it will take me enough little blog posts to map out exactly what.
In a way, that mirrors the ethos of the running: keep the habit, that's more important.
A good running habit doesn't consist of a single good run, it consists of running regularly.
That's what the scoring system is ultimately there to promote: regularity. Repetition._

_Coming at it from the other direction: there's a kind of small, distributed, grassroots, collaborative, effortless, playful use of computers that I really like and would like to have more of in my life.
When I was a teenager, I wanted to build my own operating system.
Looking back, I realize it would have been enough for me to build my own_ environment.
_That's what I really wanted._

_And that's what epoq is, to a first approximation.
It's a revival of that old dream, to make computing effortless, reactive, and immediate.
It's an invocation of the old gods of Smalltalk and Logo and the Lisps and HyperCard.
May they breathe life into our phones and our tablets, our laptops, our web sites, and our stationary computers.
It's about reclaiming our devices, our environments for us, the users.
It's also vaporware, and it's a little bit too big to put into words, but... gotta start somewhere.
Again, it's not about big-bang efforts, it's about iterating, and keeping the ball rolling._
