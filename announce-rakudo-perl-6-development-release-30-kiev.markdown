---
title: Announce: Rakudo Perl 6 development release #30 ("Kiev")
author: Carl Mäsak
created: 2010-06-18T01:46:00+02:00
---
On behalf of the Rakudo development team, I'm pleased to announce the June2010 development release of Rakudo Perl #30 "Kiev". Rakudo is an implementation of Perl 6 on the Parrot Virtual Machine (see [www.parrot.org](http://www.parrot.org/)). The tarball for the June 2010 release is available from [Github](http://github.com/rakudo/rakudo/downloads).

Rakudo Perl follows a monthly release cycle, with each release named after a Perl Mongers group. This release is named after the Perl Mongers from the beautiful Ukrainian capital, Kiev. They recently helped organize and participated in the Perl Mova + YAPC::Russia conference, the хакмит (hackathon) of which was a particular success for Rakudo. All those who joined the Rakudo hacking — from Kiev and further afield — contributed spec tests as well as patches to Rakudo, allowing various RT tickets to be closed, and making this month's release better. Дякую!

Some of the specific changes and improvements occurring with this release include:

- Rakudo now uses immutable iterators internally, and generally hides their existence from programmers. Many more things are now evaluated lazily.
- Backtraces no longer report routines from Parrot internals. This used to be the case in the Rakudo alpha branch as well, but this time they are also very pleasant to look at.
- Match objects now act like real hashes and arrays.
- Regexes can now interpolate variables.
- Hash and array slicing has been improved.
- The REPL shell now prints results, even when not explicitly asked to print them, thus respecting the "P" part of "REPL".
- Rakudo now passes 33,378 spectests. We estimate that there are about 39,900 tests in the test suite, so Rakudo passes about 83% of all tests.

For a more detailed list of changes see [docs/ChangeLog](http://github.com/rakudo/rakudo/blob/master/docs/ChangeLog).

The development team thanks all of our contributors and sponsors for making Rakudo Perl possible, as well as those people who worked on parrot, the Perl 6 test suite and the specification.

The following people contributed to this release: Patrick R. Michaud, Moritz Lenz, Jonathan Worthington, Solomon Foster, Patrick Abi Salloum, Carl Mäsak, Martin Berends, Will "Coke" Coleda, Vyacheslav Matjukhin, snarkyboojum, sorear, smashz, Jimmy Zhuo, Jonathan "Duke" Leto, Maxim Yemelyanov, Stéphane Payrard, Gerd Pokorra, cognominal, Bruce Keeler, Ævar Arnfjörð Bjarmason, Shrivatsan, Hongwen Qiu, quester, Alexey Grebenschikov, Timothy Totten

If you would like to contribute, see ["How to help"](http://rakudo.org/how-to-help), ask on the [perl6-compiler](mailto:perl6-compiler@perl.org) mailing list, or ask on IRC #perl6 on freenode.

The next release of Rakudo (#31) is scheduled for July 22, 2010. A list of the other planned release dates and code names for 2010 is available in the [docs/release_guide.pod](http://github.com/rakudo/rakudo/blob/master/docs/release_guide.pod) file. In general, Rakudo development releases are scheduled to occur two days after each Parrot monthly release. Parrot releases the third Tuesday of each month.

Have fun!


