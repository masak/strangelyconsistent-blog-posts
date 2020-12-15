Here, have a picture of space.

<img src="http://strangelyconsistent.org/blog/images/cga-space.png">

This picture, along with the rest of this post, gets to represent a few things I haven't done yet.
The whole "epoq diary" series is all about doing small things, but doing them regularly, iteratively, and they end up being a big thing.
This picture is one of those small things, a return to simplicity.
I don't care if you don't like the colors; they make me happy.

How did I create it?
There's some code which culminates in these lines:

```javascript
for (let i = 0; i < 100; i++) {
    setPixel(Math.random() * 320, Math.random() * 200, white);
}

drawCircle(260, 199, 120, { fill: magenta, stroke: magenta });
drawCircle(50, 50, 40, { fill: cyan, stroke: magenta });
```

(Not the actual code.
The code has been changed to look slightly more competent than my current API.)

The coolest thing about it (at least I think so) is that the picture has been generated directly as a PNG image by some JavaScript code.
After my umpteenth frustration with the Canvas API and trying to render things without antialiasing, I found [this guy's PNG JavaScript library](https://www.xarg.org/2010/03/generate-client-side-png-files-using-javascript/), and it's exactly what I always wanted.
Now I can write some JavaScript code, and render a PNG image!

But that's not even what I want to blog about!
I wanted to stage a blog post where I ended up with a cute little API for drawing things onto this CGA canvas, but then realized that my API was all wrong because it was like jQuery, when it should really be like React.
(That would still be a pretty neat post.)

Or a whole blog post where I extol the virtues of CGA, this hopeless format with no redeeming qualities whatsoever, and yet it makes me warm and happy to look at and to draw with.
I have this weird vision of a speculative-fiction alternate timeline where screens never evolved beyond those 320x200 pixels, and that ill-chosen palette of four colors.

In fact, much of the drawing API reminds me of old Turbo Basic, a dialect of basic where `LINE` and `CIRCLE` weren't just API calls, but dedicated statement forms.
Just the thought of that feels wasteful today, decadent.

And would you believe it, yesterday I wrote almost a whole meta-object protocol in Bel?
That would make a pretty darn nice blog post as well.
It was effortless to write, and kinda slow to run &mdash; which sums up the current Bel experience, I think.

I also had this idea about deriving a good n-queens solver from first principles, starting with just a loose description of the problem and then refining down to a fast algorithm.

And easily a few dozen other ideas right now.
The above are just the ones from the past week or so.

But the ethos of these eopq diaries is that ideas on paper aren't enough.
Theory and practice need to meet.
Sparks need to fly.
And it should be possible to get somewhere in 30-minute increments, during those precious evening and weekend moments when inspiration visits.

Hence the picture of space today.
