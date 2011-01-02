---
title: November won a prize
author: Carl Mäsak
created: 2009-03-06T09:36:00+01:00
---
*Reproducing Conrad's [announcement on p6u](http://www.nntp.perl.org/group/perl.perl6.users/2009/03/msg964.html) in its entirety:*

On behalf of AthenaLab, I’m extremely happy to announce thewinners of AthenaLab’s $1,000 prize for a Perl 6 based wiki. The winners are Carl Mäsak, Johan Viklund, and Ilya Belikin. They are members of [the November project](http://www.november-wiki.org/), which is a wiki written in Rakudo Perl 6.

Carl Mäsak and Johan Viklund started the November project, and Ilya Belikin has been very helpful in working on later developments. I greatly appreciate what they’ve accomplished.

(Of course November crucially depends on a working version of Perl 6. So I also greatly appreciate the supporting achievements of the {Rakudo Perl 6, Parrot VM, and Pugs Perl 6 prototype} teams. Despite the relatively few people involved, and their very limited financial support, their tireless efforts have nevertheless given us the rapidly-maturing Perl 6 system. This system is now relentlessly evolving into an {exceptionally-adaptable, super-industrial-strength, super-pragmatic} platform. It’s still got a ways to go, but the growing development momentum is extremely encouraging.)

So thanks much to everyone involved for all of their great work! Please consider supporting them by whatever means you have available.

A public prototype of November is now operational. While November is still in the early stages of development, considerable progress has been made. The November developers have also been very helpful in supporting Rakudo Perl 6 development. As Carl Mäsak puts it: "I'm crashing Rakudo today, so that it won't crash for you tomorrow. That’s how I like to think of it. There’s no way around it: someone has to go first and clear the way with a machete. Software grows robust by being tested and fixed a lot."

That was one of my main objectives in selecting a prize topic back in May, 2006. November has already exceeded my hopes along these lines, so I decided to make the award back in January of this year. We delayed the public announcement to allow time for completing the latest release of November, and for making the corresponding public server upgrade.

More information about {the November wiki project, Rakudo Perl 6, and related topics} can be found at the end of this note.

Carl has kindly provided the following overview of achievements, near term plans, and long term vision.

## What has been done so far

Since it was unveiled to the world during YAPC::EU in August, the Perl 6 wiki engine called "November" has seen over 750 commits from mainly three core developers. During that time, the wiki has matured from a simple script to a family of modules, some of which have trickled into other Perl 6 projects. As soon as Rakudo could precompile modules, we added a build step, which led to a much-needed 17-fold speedup in page loads.

November’s port of CPAN’s HTML::Template was completely rewritten using Perl 6 grammars. A tags/tagcloud feature was added. A subset of the MediaWiki markup has been implemented. The beginnings of a set of Perl 6 web modules were seen with the arrival of modules for application-level URI dispatching. November was given a brand new default stylesheet, and rewritten to be compatible with mod_perl6, also currently in development. Using an improved version of Rakudo’s Test.pm, we added tests for all of our existing modules.

Today, the Perl 6 version of November can do user authentication, editing, basic wiki formatting, recent changes, and pluggable layouts and formatting engines.

## Plans for the next few months

November is usable today, but lacks many basic features usually associated with a wiki engine. Specifically, we hope see images, per-page revisions, line-by-line diffs, conflict merging, widget plugins, formatting plugins, and RSS/Atom feeds added within the next half-year or so.

During the past few weeks, a small ecosystem of Perl 6 projects has started to take shape, mainly on github. To prevent code duplication across projects, some modules will likely leave the November nest and make homes of their own. A simple installer (called "proto") has been created to install and manage dependencies smoothly -- installing and setting up Parrot, Rakudo, and November with all its dependencies can thus be done with a single command, making November (and other Perl 6 projects) more accessible for newcomers.

There are also plans to develop a one-stop Web module in Perl 6, making it easier to create and deploy all types of web applications, from almost-static pages to full-featured MVC-based web sites. When this module is mature enough, November will be refitted to run on top of it.

## Long-term vision

A fair amount of commits remain until November can be called "full-featured", but when that happens, it is our hope that people will pick it up, deploy it, enjoy it, and extend it.

November still has many, many commits to go before it can actually trump other good wiki implementations out there in terms of desirable features. However, it is our hope that it will continue to serve the Perl 6 community in terms of catching bugs, exploring new language patterns, and providing inspiration for novice Perl 6 hackers. One day, it might well sport a set of features that other wiki implementations can only dream of having. The Perl 6 language makes such a vision seem quite probable.

## End of overview

Further information

<dl>
<dt>The original version of this announcement:</dt>
<dd> <a href='http://www.athenalab.com/Perl_6_Wiki_Award.htm'>http://www.AthenaLab.com/Perl\_6\_Wiki\_Award.htm</a> </dd>
<dt>The November page:</dt>
<dd> <a href='http://www.perlfoundation.org/perl6/index.cgi?november'>http://www.perlfoundation.org/perl6/index.cgi?november</a> </dd>
<dt>Carl M&#228;sak's blog:</dt>
<dd> <a href='http://use.perl.org/~masak/journal/'>http://use.perl.org/~masak/journal/</a> </dd>
<dt>The newly-designated primary site for Rakudo Perl 6
information (in early set up stage):</dt>
<dd> <a href='http://rakudo.org/'>http://rakudo.org/</a> </dd>
<dt>The official Perl 6 wiki:</dt>
<dd> <a href='http://www.perlfoundation.org/perl6/index.cgi?perl_6'>http://www.perlfoundation.org/perl6/index.cgi?perl\_6</a> </dd>
<dt>The Long Perl 6 Super-Feature List:</dt>
<dd> <a href='http://www.perlfoundation.org/perl6/index.cgi?the_long_perl_6_super_feature_list'>http://www.perlfoundation.org/perl6/index.cgi?the\_long\_perl\_6\_super\_feature\_lis  </a> </dd>
<dt>Rakudo Perl 6 Feature Status:</dt>
<dd> <a href='http://www.perlfoundation.org/perl6/index.cgi?rakudo_feature_status'>http://www.perlfoundation.org/perl6/index.cgi?rakudo\_feature\_status</a> </dd>
<dt>The origins of this $1,000 prize:</dt>
<dd> <a href='http://www.perlfoundation.org/perl6/index.cgi?conrad_schneiker#about_that_1_000_prize'>http://www.perlfoundation.org/perl6/index.cgi?conrad\_schneiker#about\_that\_1\_000  prize</a> </dd>
</dl>

PS: Thanks to {Andy Lester of http://perlbuzz.com/ and associates} for setting up the official {Perl 5 and Perl 6} wikis in 2006, and to {Paul Fenwick and associates} at [http://perl.net.au/wiki/Perl_6](http://perl.net.au/wiki/Perl_6) for earlier versions.

Best regards,<br>
 Conrad Schneiker

*Postscript by masak: Thanks to all the people who have been involved so far: Johan and Ilya for helping carry November, Jonathan and Patrick for doing an awesome job with Rakudo, all the helpful people at #perl6 and #parrot and the mailing lists, and especially Conrad for giving the idea life in the first place. Everybody: you rock.*


