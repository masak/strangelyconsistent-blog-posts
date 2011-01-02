---
title: Some Perlmonks password statistics
author: Carl Mäsak
created: 2009-07-29T21:00:00+02:00
---
PerlMonks [has been hacked](http://www.perlmonks.org/?node_id=784123), and someone (or more likely a group of people) will perhaps feel the requisite brand of shame over the fact that a lot of people's passwords were leaked, because they were stored in clear text. Not only does that constitute a poor technological solution, it's also putting other people's entrusted private information, and parts of their digital identity, at risk. With people's privacy comes [great responsibility](http://www.guardian.co.uk/technology/2008/jan/15/data.security).

Anyway, I took the leaked passwords and ran them through a script to get a bit of statistics on the different types of passwords used by a representative slice of the Perlmonks users:

    total                 567  (100.00%)
      alphanumerics-only  517  ( 91.18%)
        digits-only         9  (  1.59%)
        letters-only      233  ( 41.09%)
        letters&u-score     2  (  0.35%)
        letters&digits    277  ( 48.85%)
          letters&1digit  103  ( 18.17%)
          letters&2digits  89  ( 15.70%)
          letters&3digits  39  (  6.88%)
          letters&4digits  36  (  6.35%)
          letters&5digits   9  (  1.59%)
          letters&6digits   1  (  0.18%)
      with non-alnums      50  (  8.82%)
        1 non-alnum        34  (  6.00%)
        2 non-alnums       14  (  2.47%)
        3 non-alnums        2  (  0.35%)

[Here's the source code](http://gist.github.com/158336), a simple Perl 6 script. The source data is easy to find, but I'm not going to link to it.


