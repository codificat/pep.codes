+++
title = "Advent of Emacs Lisp"
author = ["Pep Turró Mauri"]
date = 2022-12-22T21:35:00+01:00
tags = ["lisp", "emacs"]
draft = false
+++

I have been using Emacs for quite some time now, but so far I have never invested time to learn Emacs Lisp.

I thought I could give a try at learning some basics using this year's [Advent of code](https://adventofcode.com/). I am posting my progress in a [repo in codeberg](https://codeberg.org/codificat/advent-of-code/src/branch/main/2022).


## Day 1 {#day-1}

Hey, it's my very first Emacs Lisp program. And I don't have much time. And by
one of these coincidences of the world, right when I had this thought of giving
this a try, I saw [this blog post](https://sachachua.com/blog/2022/12/2022-12-19-emacs-news/) pop up in my RSS feeds, pointing to a [cool video
](https://youtu.be/N1PAC5vs15Y)by Gavin Freborn explaining how to solve this challenge.

So, I basically used that video to learn a bit, and for the [solution](https://codeberg.org/codificat/advent-of-code/src/branch/main/2022/aoc01.el) I took
[his code](https://gist.github.com/Gavinok/1631fd138fc91a08a33c4b66afe15f39) and only replaced a couple of common-lisp calls for (what I think) more
emacs-lispy ones.


## Day 2 {#day-2}

I applied the principles I learnt from Day 1 here, using a temp buffer as a sort
of _powerful string_.

I am quite happy with [the result](https://codeberg.org/codificat/advent-of-code/src/branch/main/2022/aoc02.el), from a "learning elisp" perspective. It's
likely not the most efficient way of solving this, but it feels to me that it
hopefully captures the expressiveness of Lisp. Re-using the solution to part 1
to add adding part 2 rules resulted, I think, in a nice application of that.
