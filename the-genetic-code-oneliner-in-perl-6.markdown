---
title: The Genetic Code one-liner in Perl 6
author: Carl Mäsak
created: 2009-07-06T06:16:00+02:00
---
Today on #perl6:

<pre><code>&lt;masak&gt; rakudo: subset DNA of Str where { all(.uc.comb) eq any &lt;A C G T&gt; }; my DNA $dna = "gattaca"; say $dna;
&lt;p6eval&gt; rakudo 0e8a86: OUTPUT&#171;gattaca&#9252;&#187;
&lt;jnthn&gt; rakudo: subset DNA of Str where { all(.uc.comb) eq any &lt;A C G T&gt;}; my DNA $dna = "lolnotdna"; say $dna;
&lt;p6eval&gt; rakudo 0e8a86: OUTPUT&#171;Assignment type check failed [...]
&lt;jnthn&gt; (just checking :-))
&lt;TimToady&gt; course, where not /&lt;-[ACGTactg]&gt;/ might beat all those
&lt;moritz_&gt; but it involves no junction, so it can't be any good :-)
&lt;TimToady&gt; ttaggg &amp;
&lt;masak&gt; halp, TimToady is speaking in DNA bases!
&lt;masak&gt; is my hunch right, and that actually means something as amino acids?
* masak checks
&lt;pyrimidine&gt; yes
&lt;masak&gt; TimToady++
&lt;masak&gt; rakudo: my $dna = "ttaagg"; sub translate($dna) { "FFLLSSSSYY!!CC!WLLLLPPPPHHQQRRRRIIIMTTTTNNKKSSRRVVVVAAAADDEEGGGG".comb[map { :4($_) }, $dna.trans("tcag" =&gt; "0123").comb(/.../)] }; say translate($dna)
&lt;p6eval&gt; rakudo 0e8a86: OUTPUT&#171;LR&#9252;&#187;
&lt;masak&gt; that's what I got too.
&lt;pyrimidine&gt; masak: nice!
&lt;masak&gt; I know! :)
&lt;masak&gt; I should blog about it.
&lt;masak&gt; "The <a href='http://en.wikipedia.org/wiki/Genetic_code'>Genetic Code</a> one-liner in Perl 6"
</code></pre>

A hyper-short summary of what that one-liner does:

- Convert each base into an integer. `$dna.trans("tcag" => "0123")` 
- Break up the DNA numbers in triplets. `.comb(/.../)` 
- Interpret each three-digit number as a [quaternary](http://en.wikipedia.org/wiki/Quaternary_numeral_system) integer between 0 and 63. `map { :4($_) }` 
- Use these numbers as indexes into an array of 64 one-character strings, each with the character of one amino acid.

Perl 6 feels more like a power tool every day.


