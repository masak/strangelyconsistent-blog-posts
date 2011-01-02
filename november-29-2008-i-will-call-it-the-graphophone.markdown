---
title: November 29, 2008 &#8212; "I will call it 'the graphophone'!"
author: Carl Mäsak
created: 2008-11-30T00:25:00+01:00
---
131 years ago today, Thomas Edison demonstrated for the first time a device he called a "[phonograph](http://en.wikipedia.org/wiki/Phonograph)", which plays back recorded sound. [Wikipedia](http://en.wikipedia.org/wiki/Phonograph#First_phonograph):

<blockquote><div><p>Edison's early phonographs recorded onto a tinfoil sheet phonograph cylinder using an up-down ("hill-and-dale") motion of the stylus. The tinfoil sheet was wrapped around a grooved cylinder, and the sound was recorded as indentations into the foil. Edison's early patents show that he also considered the idea that sound could be recorded as a spiral onto a disc, but Edison concentrated his efforts on cylinders, since the groove on the outside of a rotating cylinder provides a constant velocity to the stylus in the groove, which Edison considered more "scientifically correct". Edison's patent specified that the audio recording be embossed, and it was not until 1886 that vertically modulated engraved recordings using wax coated cylinders were patented by Chichester Bell and Charles Sumner Tainter. They named their version the Graphophone. Emile Berliner patented his Gramophone in 1887.</p></div></blockquote>

Dang, I kinda liked the sound of "graphophone". 哈哈

<p class='separator'>&#10086;</p>

Today, in order to reduce some of the [repetitiveness](http://github.com/viklund/november/tree/d58040420ba34561cf8213dfa96455cb5e7b5c7c/p6w/t/markup/mediawiki/07-italic-and-bold.t) of the MediaWiki markup parser test suite (set input, set expected output, calculate actual output, test, rinse, repeat), I created the module [`Test::InputOutput`](http://github.com/viklund/november/tree/mediawiki-markup/p6w/Test/InputOutput.pm), which resembles CPAN's `Test::Base` a bit, minus the syntactic relief. Some of the MediaWiki tests now look [like this](http://github.com/viklund/november/tree/f882a653455e6eb3370f4c997fac6b52d02a2726/p6w/t/markup/mediawiki/07-italic-and-bold.t) instead. Typical example of separating the algorithm from the specifics.

Since that wasn't what I set out to do today, I'm going to stop here, and do that instead. I want to continue passing tests concerning italic and bold.

Also, ihrd++ merged his dispatch branch today into master, so I'll review that merge.

In short, if you haven't downloaded November yet, you should. If nothing else, there's a lot of working Perl 6 code to look at. It's not even hard: look, [a link](http://github.com/viklund/november/)! The kind folks over at github even provide a [zipball](http://github.com/viklund/november/zipball/master) and a [tarball](http://github.com/viklund/november/tarball/master) for those who don't have git yet. That's what I call service.

Enjoy!


