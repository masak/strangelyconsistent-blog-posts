---
title: Step 1 on the road to recovery: admitting you have a problem
author: Carl Mäsak
created: 2010-03-29T06:37:00+01:00
---
In order to be better, we had to refactor Rakudo. It wasn't a question of 'if', it wasn't a question of 'when'. If we were to meet the goals of Rakudo Star in 2010Q2, we'd have to pull Rakudo apart and put it back together. It would lead to a lot of downstream breakage, but better now than later.

We're currently at the 'downstream breakage' part. Until now, I haven't really taken a look at the scope of the breakage. Today, I will. I suspect I'll spend much of April doing damage control.

In other words, the "admitting you have a problem" from the blog post topic refers to the fact that until now, I've been an alpha guy. "alpha" is the code name for "the old Rakudo master branch". You know, the one on which my code works. But being an alpha guy will feel more and more quaint, and I hope I don't have to be an alpha guy up until the point Rakudo Star is released. That would feel terribly inconsistent.

So. Let's try to run my favourite apps on Rakudo master.

## November

Trying to compile November, I get the error message "Unable to parse blockoid, couldn't find final '}' at line 8" on the first file I attempt to compile. After some digging, it turns out that the error isn't actually *located* in that file, but in a file used by a file used by that file. The error message is due to this regex definition.

<pre><code>token port       { (\d**1..5)                    <b>&lt;?{{</b> $I0 = match[0]
                         $I1 = 0
                         if $I0 &gt; 65535 goto fail
                         $I1 = 1
                       fail:
                         .return ($I1)
                    <b>}}&gt;</b> 
                   &lt;!before \d&gt; };</code></pre>

The trained rakudologist immediately picks out the PIR code assertion (highlighted in bold), a construct no longer supported in Rakudo after the refactor. But real, non-PIR code assertions *are* supported, so the above should simply be swapped out for `<?{ $0 < 2 ** 16 }>`.

I swap it out and move on.

Next fun error message: "You can not add a Method to a module; use a class, role or grammar". This occurs in a file where I'm decidedly in a class, and not a module. Rakudo is wrong; I am right.

So what *is* going on to make me get that error message? This:

<pre><code>$ <b>cat A.pm</b> 
grammar A {
}

$ <b>cat B.pm</b> 
class B;

method foo() {
    use A;
}

$ <b>perl6 B.pm</b> 
You can not add a Method to a module; use a class, role or grammar
[...]</code></pre>

My guess is that the parser gets confused about the kind of scope it's in when switching between files. I submit this as [a rakudobug](http://rt.perl.org/rt3/Public/Bug/Display.html?id=73886) and move on.

## Druid

Stuck at the Configure step. The error message is familiar: "You can not add a regex to a module; use a class, role or grammar". That's just pure bigotry. And it's been [discussed](http://irclog.perlgeek.de/perl6/2010-03-09#i_2083599) [before](http://irclog.perlgeek.de/perl6/2010-03-26#i_2156700), but not resolved yet.

Luckily, the regex can be easily inlined in this case, so I do that and move on to the next message:

<pre><code>$ <b>perl6 Configure</b> 

Configure.pm is preparing to make your Makefile.

Method 'postcircumfix:&lt;{ }&gt;' not found for invocant of class 'Failure'
[...]</code></pre>

Please bask in the sheer informativeness of it all while I try to find the cause.

Ah. The reason for the error message is that alpha has a `%*VM<config><build_dir>`, but master currently doesn't even have a `%*VM`. I could hack around that, but I'll submit [a rakudobug](http://rt.perl.org/rt3/Ticket/Display.html?id=73888) instead.

## proto

<pre><code>$ <b>./proto</b> 
Can't locate File/HomeDir.pm in @INC [...]</code></pre>

In all the chaotic mess that is Rakudo error messages, it feels comforting and a bit relieving to get a Perl 5 error. And one as mundane as this, to boot.

Hm, I heard mberends++ [mention](http://irclog.perlgeek.de/perl6/2010-03-28#i_2162767) that he was doing biggish changes in proto. Seems this is one of the results. I must have a look at them later they seem interesting but right now I'm looking for Perl 6 errors. So, moving on.

## Web.pm

Same regex-in-module bigotry as with Druid. And after that, same `%*VM` trouble. Not too surprising in retrospect; they use the same `Configure.pm` module.

## GGE

GGE also uses that `Configure.pm` module, so I just skip the configure step and manually replace `alpha` with `perl6` in the `Makefile`. This is the error I get:

<pre><code>$ <b>make</b> 
env PERL6LIB='/Users/masak/gwork/gge/lib:/Users/masak/gwork/gge/lib:/Users/masak/wo  k/hobbies/parrot/languages/rakudo' perl6 --target=pir --output=lib/GGE/Match.pir lib/GGE/Match.pm
Placeholder variables cannot be used in a method at line 39, near "multi meth"
[...]</code></pre>

The [method](http://github.com/masak/gge/blob/master/lib/GGE/Match.pm#L39) in question contains zero (0) placeholder variables. So what's going on here?

As usual, the answer turns out to be "something completely different". It's in the method above, which *also* doesn't contain any placeholder variables, that the error originates:

<pre><code>$ <b>perl6 -e 'class A { multi method new(*%\_) { |%\_ } }'</b> 
Placeholder variables cannot be used in a method at line 1, near "}"
[...]</code></pre>

And that's just wrong, so I submit [a rakudobug](http://rt.perl.org/rt3/Public/Bug/Display.html?id=73892) and call it a day.

## Conclusion

A good first harvest.

It's difficult to judge whether I've found 10% of the total breakage, or 1%. But regardless, this needs to be done, and sooner rather than later. I think the allure of easy-to-reap rakudobugs will urge me on in this quest.

I still haven't given up the thought of doing nightly builds of all the projects. That would amount to automatic (or at least greatly simplified) bug finding and collection.

Looking forward to the first automatically found rakudobug. 哈哈


