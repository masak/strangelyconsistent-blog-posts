---
title: Week 2 of GSoC work on Buf &#8212; the power of Swedish beer
author: Carl Mäsak
created: 2010-06-05T15:03:00+02:00
---
Last week's summary was short out of choice; this week's is short out of necessity. I'm traveling a bit, and my access to the net isn't the best.

- Changed the spec to contain only an array constructor for Buf, and removed the slury-array constructor.
- The Parrot people are discussing a long-term solution for the conversion between strings and bytes. They are (rightly) worried about exposing the wrong bits, and want things to be right.
- For the short term, jnthn++ hacked together a Parrot type called ByteView in my branch. I tried it, and it was almost enough for encoding a string. The whole thing is clearly doeable.

Where does Swedish beer enter into it? Well, jnthn's example string was "öl", the Swedish word for "beer". So the Buf implementation is definitely helped along by Swedish beer. 哈哈


