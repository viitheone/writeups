this is the write-up for the tryhackme challenge: https://tryhackme.com/room/corridor

on visiting the machine ip, we will be redirected to a page which has a picture of a corridor and several doors.
you may notice that each door has a url which leads to an empty room.

thing to notice here is the url, since the challenge description told us about idor and hinting about hashes.

if you see all the urls on the door and observe:

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

these are md5 hashes

particularly 
md5(1) -> c4ca4238a0b923820dcc509a6f75849b
md5(2) -> c81e728d9d4c2f636f067f89cc14862c

and so on. 

so most likely the hidden door must be 0?

compute its md5 hash which is: cfcd208495d565ef66e7dff9f98764da

visit the page and you have the flag!!