# TryHackMe: Corridor Write-up

link: https://tryhackme.com/room/corridor

<img width="890" height="406" alt="corridor" src="corridor/corridor.png" />


## Initial Observation

Upon visiting the machine's IP address, you are redirected to a page featuring a picture of a corridor with several doors. Each door links to a URL leading to an empty room.

## Key Insight

Pay attention to the URLs, as the challenge description mentions IDOR (Insecure Direct Object Reference) and hints at hashes.

## Hash Analysis

Observe the URLs on the doors:

```
c4ca4238a0b923820dcc509a6f75849b
c81e728d9d4c2f636f067f89cc14862c
eccbc87e4b5ce2fe28308fd9f2a7baf3
a87ff679a2f3e71d9181a67b7542122c
e4da3b7fbbce2345d7772b0674a318d5
1679091c5a880faf6fb5e6087eb1b2dc

8f14e45fceea167a5a36dedd4bea2543

c51ce410c124a10e0db5e4b97fc2af39
c20ad4d76fe97759aa27a0c99bff6710
6512bd43d9caa6e02c990b0a82652dca
d3d9446802a44259755d38e6d163e820
45c48cce2e2d7fbdea1afc51c7c6ad26
c9f0f895fb98ab9159f51fd0297e236d
```

These are **MD5 hashes**, specifically:

- `md5(\"1\")` → `c4ca4238a0b923820dcc509a6f75849b`
- `md5(\"2\")` → `c81e728d9d4c2f636f067f89cc14862c`
- ...and so on up to `md5(\"9\")`.

Notice the missing `md5(\"0\")`?

## Solution

The hidden door is likely **0**. Compute its MD5 hash:

```
md5(\"0\") = cfcd208495d565ef66e7dff9f98764da
```

Visit `/cfcd208495d565ef66e7dff9f98764da` and **you have the flag!!**

---

*Write-up complete. Enjoy hacking!*"

