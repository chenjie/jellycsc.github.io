---
layout: post
title:  "XOR using NAND, AND, OR"
date:   2018-04-13 14:12:00
categories: ML
tags: Logic
---

* content
{:toc}

This is a quite interesting problem. It's not difficult but requires some effort. As long as what I wrote inspires you, you are no longer afraid of trying different logic gates and manipulating them. This is the essence of teaching and learning.





## Logic gates
To prove the simple equivalence of wiring different logic gates, the first thing I would try is to build a truth table. If the result of two truth tables matches exactly, then two sets of gates wiring are guaranteed to be equivalent.

### XOR
Here is the truth table for XOR gate.

| A | B | XOR |
|---|---|-----|
| 0 | 0 |  0  |
| 0 | 1 |  1  |
| 1 | 0 |  1  |
| 1 | 1 |  0  |

### NAND, OR
Combine them and figure out a way yourself to wire them together so that it is equivalent to XOR.

| A | B | XOR | `←` | NAND | OR  |
|---|---|-----|-----|------|-----|
| 0 | 0 |  0  | `←` |  1   |  0  |
| 0 | 1 |  1  | `←` |  1   |  1  |
| 1 | 0 |  1  | `←` |  1   |  1  |
| 1 | 1 |  0  | `←` |  0   |  0  |

LOL, that's really obvious. You just AND last two column and you get XOR!

## Wait!?
How the hack do you come up with NAND and OR at the beginning when you start? Long story short, there is no magic:(. You have to memorize some basics plus write all of them out and try to find a pattern. This is what I've actually done:

| A | B | AND | OR | `→` | XOR  |
|---|---|-----|----|-----|------|
| 0 | 0 | 0 | 0 |  `→`   | 0 |
| 0 | 1 | 0 | 1 |  `→`   | 1 |
| 1 | 0 | 0 | 1 |  `→`   | 1 |
| 1 | 1 | 1 | 1 |  `→`   | 0 |

If I AND them, the last three row's bits would be exactly flipped. OK, I have some idea, what if I flip the AND bits? The first row's resulting bit will not be affected!! Let's try it out!

| A | B | AND | NAND | OR | `→` | XOR  |
|---|---|-----|----|-----|-----|------|
| 0 | 0 | 0 | 1 | 0 |    `→`   | 0 |
| 0 | 1 | 0 | 1 | 1 |    `→`   | 1 |
| 1 | 0 | 0 | 1 | 1 |    `→`   | 1 |
| 1 | 1 | 1 | 0 | 1 |    `→`   | 0 |

That's all my effort. Thank you for your patience. A quick Google search can give you the answer immediately, but your problem solving skill won't generalize unless you try it `YOURSELF`! 
