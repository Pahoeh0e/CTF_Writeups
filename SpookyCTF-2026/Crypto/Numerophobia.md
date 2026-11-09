# Numerophobia
Category	Crypto
Technique	ASCII


### Challenge

I went exploring the house of the crazy old man on the corner of the street. He disappeared last weekend ... Rumor has it that he was last seen walking out at midnight screaming random numbers in tongues never spoken before ... On the way to the stairs I almost tripped over a stack of books, one was about dyslexia I think. For some reason every other step was missing and marked with a 13 on the wall. I finally reached the top and headed towards the light at the end of the hallway. Taped on the wall was a piece of paper…

The paper held three lines of numbers (separated by 10, which is ASCII newline). The first line was 23 followed by 50 65 repeated, and the last line was 63 52 repeated. Both seemed like filler. The payload in the middle line was much more interesting:

```138 50 65 95 123 51 64 98 108 53 134 52 132 108 65 95 66 52 117 95 127 51 132 53 123 52 108 51 117 116 136 121 120 111 124 112 128 ```


### Solution
Each clue in the story maps to one step:

Story clue	Meaning
"random numbers"	The numbers are ASCII codes
"every other step was missing"	Take every other number: two interleaved streams
"marked with a 13"	One stream has +13 added to each code
"a book about dyslexia"	Both streams are reversed (ouch are they saying dyslexic people are all backwards)
Split the lines. 10 is a newline, so there are three lines. Lines 1 and 3 are padding.
Split into two streams by taking the 1st, 3rd, 5th ... numbers (stream A) and the 2nd, 4th, 6th ... numbers (stream B).

Stream A: subtract 13 from each number and read as ASCII. 138 - 13 = 125 = }, so the stream begins with the closing brace, which tells us the text is backwards. Reversed it reads sok{h_nwrh54wy_3n4}.
Stream B: no offset at all. 112 = p, 111 = o, 121 = y, 116 = t. Reversed it reads poyt3453_4_l45b3_2.

Interleave one character from A, then one from B, and repeat:
 p o y t 3 4 5 3 _ 4 _ l 4 5 b 3 _ 2
s o k { h _ n w r h 5 4 w y _ 3 n 4 }

```s p o o k y { t h 3 _ 4 n 5 w 3 r _ h 4 5 _ 4 l w 4 y 5 _ b 3 3 n _ 4 2 }```

My first attempt used one offset for every number and then ROT13 on alternate letters. That produced spooky{th_njrEhE_jy_UaR}, which looks almost right but is not a real sentence. The giveaway was that it only worked because ROT13 on c, b, l happens to give p, o, y (adding 13 and then rotating 13 again are not the same thing). Splitting into two streams was the actual decoding method.

### Coding 

Solve script
```#!/usr/bin/env python3
"""Decode the Numerophobia note."""
payload = [138,50,65,95,123,51,64,98,108,53,134,52,132,108,65,95,66,52,117,95,
           127,51,132,53,123,52,108,51,117,116,136,121,120,111,124,112,128]

stream_a = payload[0::2]   # 1st, 3rd, 5th ... number  -> ASCII + 13
stream_b = payload[1::2]   # 2nd, 4th, 6th ... number  -> plain ASCII

a = [chr(n - 13) for n in stream_a][::-1]   # undo the +13, then un-reverse
b = [chr(n) for n in stream_b][::-1]        # un-reverse

flag = ""
for i, ch in enumerate(a):                  # weave the two streams back together
    flag += ch
    if i < len(b):
        flag += b[i]
print(flag)
```

```flag: spooky{th3_4n5w3r_h45_4lw4y5_b33n_42}```

The flag reads "the answer has always been 42", noice.

### Takeaways
- When a decode is almost readable, the structure is probably wrong, not the key. Look for interleaving
- Filler numbers that follow a clean repeating pattern (50 65, 63 52) are padding. Find the line that breaks the pattern
- You may think you have done all the work but look out for the structure of the answer you want we knew it would be spooky{} so when you get the right values when decoding it might just be a matter of rearranging
