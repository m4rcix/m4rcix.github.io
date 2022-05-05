---
layout: post
title: Monoalphabetic MadMax
tags: crypto secchallenge
permalink: /SecChall21/crypto/mono_madmax
hidden: 1
---

## Description

You love it, you hate it, it doesn’t matter this is monoalphabetic.

- Author: Suma
- Difficulty: easy
- Attachment: [enc.txt](/media/SecChall21/Crypto/Mono/enc.txt)

## Solution

Just reading the title of the challenge, it was clear we are facing a monoalphabetic substitution cipher. A quick google search led me to this [site](https://www.dcode.fr/monoalphabetic-substitution). Pasting the text and decoding it, gives us readable information: the script of Mad Max 2: The Road Warrior. One "feature", this site has, is it only gives a fixed number of characters as output, so I had to move the flag from the middle of the file to the top. (It was easy to find, thanks to the curly brackets)

The acquired flag is:
```
cd21{HIDDEN}
```

[&#8592; Back to SecChallenge21](/SecChall21)
