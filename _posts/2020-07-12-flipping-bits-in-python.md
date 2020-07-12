---
layout: post
date: 2020-07-12 18:00 +0200v
categories: ["Python 3","HackerRank","HackerRank-easy"]
title: Flipping bits in Python
---
The first things that comes up when you google "Flipping bits" is [this coding problem on HackerRank](https://www.hackerrank.com/challenges/flipping-bits/problem). 
It is one of the basic problems in the HackerRank's bit manipulation tutorial and it's designed to teach ["bitwise or"](https://en.wikipedia.org/wiki/Bitwise_operation#OR) operation.

While flipping all bits in an unsigned 32-bit number is trivial (in a language like C), there are a few gotchas concerning Python 3.

All integers are interpreted as signed [`long` of arbitrary size](https://docs.python.org/3.7/c-api/long.html#integer-objects), so when we apply "bitwise or" to a positive number (`~n`) we set the sign bit and the number turns into negative one. Naturally, all the test cases fail.

## Masking

We can easily work around this by treating 32-bit unsigned `int` as a 64-bit signed `long` with masked bits above 32:

```python
LOWER_32 = int('0b0' + '1'*32, 2)

def flippingBits(n):
    return ~n & LOWER_32
```

`LOWER_32` is declared by parsing a binary string starting with a 0 (to make sure sign is positive) and ending with 32 ones. 
This could also be written as `2**32 - 1`, or number `4294967295`. 
If this statement is not clear, think of a smaller number with all lower bits set, e.g. 7. 
In binary seven has three lower bits set: `0000 0111`, and seven is 2³ - 1.

Then we apply "bitwise and" to the result in order to remove the sign bit and make the result positive like all unsigned numbers are.

## Two's complement

Another neat trick is to remember that negative numbers are stored as ["two's complement"](https://en.wikipedia.org/wiki/Two%27s_complement). The two's complement is calculated by inverting the bits and adding one. So we can get the negative complement first, subtract one.

```
# given
twos_complement = flipped_bits + 1
# then
flipped_bits = twos_complement - 1
# a positive number and it's complement sum up to next power of 2
positive_int + twos_complement = 2**32
# in other words
twos_complement = 2**32 - positive_int
# so then
flipped_bits = 2**32 - 1 - positive_int
```

Same in Python: 
```python
def flippingBits(n):
    return 2**32 - 1 - n
```

