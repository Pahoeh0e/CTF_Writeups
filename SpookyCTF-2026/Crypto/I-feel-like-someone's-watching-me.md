# I Feel Like Someone's Watching Me
Category	Crypto
Technique	Monoalphabetic substitution (solved with cribs)

### Challenge
A letter is found beside a tombstone in a graveyard, but it seems to have been scrambled around.

The letter was a block of text starting 'Ypejpof Erpveaypj', and ending 'Odagpjprz, Fcp Lap Zlw Rptf Ipcday'. Word lengths and punctuation (apostrophes, commas, a sign-off) were intact, so I instantly saw that it must be a letter.

### Solution
1. Not a Caesar shift
The sign-off Odagpjprz had to be "Sincerely" initally I tried "Regards" but the repetition of "p" matched the two e's in sincerely. That gives S to O (4 places back) but E to P (11 places forward). Different letters move different distances, so each letter has its own mapping.

2. Cribs
Cipher word	Guess	Letters gained
Ypejpof	Dearest	y=d, p=e, e=a, j=r, o=s, f=t
Erpveaypj	Alexander	r=l, v=x, a=n
Odagpjprz	Sincerely	d=i, g=c, z=y

3. Common words
Pmpj odagp zlw rptf is "Ever since you left", giving m=v, l=o, w=u, t=f. fcp is "the" (c=h), fclwuc is "though" (u=g), xcl is "who" (x=w), e.c.t. The words iwf ("but"), bwof ("just") and kwdgn ("quick") supplied b, i and k. The cipher letter "s" never appears, so plain z is unused.

Recovered key (cipher to plain)
Cipher	a	b	c	d	e	f	g	h	i	j	k	l	m
Plain	n	j	h	i	a	t	c	m	b	r	q	o	v
Cipher	n	o	p	q	r	t	u	v	w	x	y	z
Plain	k	s	e	p	l	f	g	x	u	w	d	y

4. The letter
It is addressed to Alexander from "The One You Left Behind". She describes being followed and says she is going to the one place she has not looked, and that she will leave a clue. The key passage:

eyez are watching me. Just remember underscores instead of spaces and to wrap it correctly.

The clue sentence is the only one in lowercase.

5. flag

```flag: spooky{eyez_are_watching_me}```

### Coding

Solve script
```#!/usr/bin/env python3
"""Decode the letter with the recovered cipher->plain mapping."""
KEY = {'a':'n','b':'j','c':'h','d':'i','e':'a','f':'t','g':'c','h':'m','i':'b','j':'r',
       'k':'q','l':'o','m':'v','n':'k','o':'s','p':'e','q':'p','r':'l','t':'f','u':'g',
       'v':'x','w':'u','x':'w','y':'d','z':'y'}

def decode(ct):
    out = []
    for ch in ct:
        if ch.lower() in KEY:
            p = KEY[ch.lower()]
            out.append(p.upper() if ch.isupper() else p)
        else:
            out.append(ch)
    return "".join(out)

print(decode(open("letter.txt").read()))
```

### Takeaways:
- Look at the shape and format of a encrypted text to find clues and perhaps known words
- Work through different algorithms to rule out the most common ones first e.g. here Caesar and ROT-n were rules out because it wasn't a static length between letters
- Read the plaintext closely for flag-building instructions
