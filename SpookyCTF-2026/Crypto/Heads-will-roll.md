# Heads will Roll
Category	Crypto
Technique	Rail fence cipher

### Challenge
While chasing Ichabod Crane through Sleepy Hollow, the Headless Horseman got as close as he could, then threw his head! While rolling, rolling, rolling, rolling, through the air, his pumpkin head shouted…what? His perfect aim knocked Ichabod to the ground near the rails, but what did he say?

The attached text file contained:

```Etu_taip5_w5n_hpmnw_hlgsdso{lyh0_d}jyi_piihef_ne_oy3p0lh4oskt_i:k3l3```

### Solution
Read the hints. "Near the rails" points straight at a rail fence (zigzag) transposition. "Rolling, rolling, rolling, rolling" repeats a word four times, which suggests four rails.
After checking the ciphertext i saw it had {, } and spooky-shaped punctuation (: and _) but the letters were out of order. The character set looks like normal English, so after a little research I concluded it must have been transposition, not a substitution.
Rail count is small, so I tried 2 to 14 rails with every start offset.
4 rails as predicted gives me that sweet hit of dopamine for solving something:

```Enjoy_this_pumpkin_with_the_flag_inside:_spooky{5l33py_h0ll0w_h34d5}```

### Coding 

As I am trying to get better at python I got claude to walk me through a code to solve the rail fence cipher to better understand cryptography in python.

### Solve script
``` #!/usr/bin/env python3
"""Brute-force a rail fence cipher (all rail counts / start offsets)."""
ct = "Etu_taip5_w5n_hpmnw_hlgsdso{lyh0_d}jyi_piihef_ne_oy3p0lh4oskt_i:k3l3"

def decrypt(ct, rails, offset=0):
    cycle = list(range(rails)) + list(range(rails - 2, 0, -1))
    pattern = [cycle[(i + offset) % len(cycle)] for i in range(len(ct))]
    # ciphertext is read rail by rail, so sort positions by (rail, position)
    order = sorted(range(len(ct)), key=lambda i: (pattern[i], i))
    out = [None] * len(ct)
    for ct_index, pos in enumerate(order):
        out[pos] = ct[ct_index]
    return "".join(out)

for rails in range(2, 15):
    for offset in range(2 * (rails - 1)):
        pt = decrypt(ct, rails, offset)
        if "spooky{" in pt.lower():
            print(rails, offset, pt)
```

```Flag: spooky{5l33py_h0ll0w_h34d5}```

### Takeaways
- Repeated words and physical objects in the story ("rails", "rolling x4") are usually the cipher name and its key.
- Getting help building tools helps to understand their uses for future work e.g. pen-testing
