---
layout: post
title: Note Keeper
description: It\'s a meee, Marcix
tags: web secchallenge22
permalink: /sc22/web/notekeeper
hidden: 1
---

## Description

There is a secret on `flag.secchallenge.crysys.hu`, but it is not accessible. Luckily `notekeeper.secchallenge.crysys.hu` is served from the same host. You might find a vulnerability and access the secret portal. Good Luck!

[https://notekeeper.secchallenge.crysys.hu/](https://notekeeper.secchallenge.crysys.hu/)

- Author: Pepe

## Solution

Fiddling around the website, we can register a user, login with the created user, upload a profile picture, and leave notes. Since the title of the challenge was Note Keeper, I spent way to much time with trying to exploit the `/notes` endpoint, without success.

Luckily I stumbled back to the `/profile` endpoint, and started playing with the profile upload. I realized, that I could upload any file, and it would be hosted on the site. I've also read about a PHP vuln, that let's you upload images, and inside the images you can hide PHP code, which will be executed.
This wasn't the case however. Later Pepe told me (after solving the challenge), the challenge wasn't created using PHP.

It felt like I was on the correct path, so I diged deeper. Scanning through the description of the challenge, a lightbulb moment came. I cannot access the `flag.secchallenge.crysys.hu` but maybe the script, that downloads the file can. So I gave it `https://flag.secchallenge.crysys.hu` and it died, Response 500.

This also took some time, but finally inputting `http://flag.secchallenge.crysys.hu` into the URL field didn't make my profile unusable, and my profile picture got updated. Following the link to the "picture", I am prompted to download the `avatar`. Looking at it, its clearly an HTML source, so we finally hit jackpot. Rendering the HTML page, we can see a hint, that we are on the right path, just not quite yet there.

![image](/media/sc22/web/note/mario_meme.png)

Again, based on an educated guess, the flag will suuuuuurely be on `http://flag.secchallenge.crysys.hu/flag` right? Correct. Querying the aforementioned endpoint updates our profile picture yet again. Downloading that avatar yields us the flag:
```
cd22{HIDDEN}
```

[&#8592; Back to SecChallenge22](/sc22)
