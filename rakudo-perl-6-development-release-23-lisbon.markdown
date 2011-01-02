---
title: Rakudo Perl 6 development release #23 ("Lisbon")
author: Carl Mäsak
created: 2009-11-19T20:42:00+01:00
---
Announce: Rakudo Perl 6 development release #23 ("Lisbon")

On behalf of the Rakudo development team, I'm pleased to announce the<br>
November 2009 development release of Rakudo Perl #23 "Lisbon".<br>
Rakudo is an implementation of Perl 6 on the Parrot Virtual Machine<br>
(see http://www.parrot.org). The tarball for the November 2009 release<br>
is available from http://github.com/rakudo/rakudo/downloads

Due to the continued rapid pace of Rakudo development and the frequent<br>
addition of new Perl 6 features and bugfixes, we recommend building Rakudo<br>
from the latest source, available from the main repository at github.<br>
More details are available at http://rakudo.org/how-to-get-rakudo.

Rakudo Perl follows a monthly release cycle, with each release code<br>
named after a Perl Mongers group. The November 2009 release is code<br>
named "Lisbon" for Lisbon.pm, who did a marvellous job arranging this<br>
year's YAPC::EU.

Shortly after the October 2009 (#22) release, the Rakudo team<br>
began a new branch of Rakudo development ("ng") that refactors<br>
the grammar to much more closely align with STD.pm as well as<br>
update some core features that have been difficult to achieve<br>
in the master branch [1, 2]. Most of our effort for the past month<br>
has been in this new branch, but as of the release date the new<br>
version had not sufficiently progressed to be the release copy.<br>
We expect to have the new version in place in the December 2009 release.

This release of Rakudo requires Parrot 1.8.0. One must still<br>
perform "make install" in the Rakudo directory before the "perl6"<br>
executable will run anywhere other than the Rakudo build directory.<br>
For the latest information on building and using Rakudo Perl, see the<br>
readme file section titled "Building and invoking Rakudo".

Some of the specific changes and improvements occuring with this<br>
release include:

* Rakudo is now passing 32,753 spectests, an increase of 171 passing<br>
    tests since the October 2009 release. With this release Rakudo is<br>
     now passing 85.5% of the available spectest suite.

* As mentioned above, most development effort for Rakudo in November<br>
     has taken place in the "ng" branch, and will likely be reflected<br>
     in the December 2009 release.

* Rakudo now supports unpacking of arrays, hashes and objects in<br>
     signatures

* Rakudo has been updated to use Parrot's new internal calling conventions,<br>
     resulting in a slight performance increase.

The development team thanks all of our contributors and sponsors for<br>
making Rakudo Perl possible. If you would like to contribute,<br>
see http://rakudo.org/how-to-help , ask on the perl6-compiler@perl.org<br>
mailing list, or ask on IRC #perl6 on freenode.

The next release of Rakudo (#24) is scheduled for December 17, 2009.<br>
A list of the other planned release dates and codenames for 2009 is<br>
available in the "docs/release_guide.pod" file. In general, Rakudo<br>
development releases are scheduled to occur two days after each<br>
Parrot monthly release. Parrot releases the third Tuesday of each month.

Have fun!

[1] http://use.perl.org/~pmichaud/journal/39779<br>
[2] http://use.perl.org/~pmichaud/journal/39874


