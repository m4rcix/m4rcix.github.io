---
layout: post
title: SQpLosIon
description: This one came with a bang
tags: web secchallenge
permalink: /SecChall21/web/sqplosion
hidden: 1
---

## Description

A manual? Coordinates?

Damn. Joe maybe already possess an anti-apocalypse machine. What is he up to? You have to get there before him.

You head out, but as you get close to the marked location, you see Joe his Warboys, and multiple war rigs. You need to access the controller of the war rigs remotely and blow them up. Hurry up the fate of the wastes is in your hands.

[https://sqlplosion.secchallenge.crysys.hu](https://sqlplosion.secchallenge.crysys.hu)

- Author: Pepe
- Difficulty: easy

## Solution

Reading the challenge name, one might suspect, that it has something to do with SQL injection, and one would be right.

After looking around for other accessible pages with `dirbuster` and finding nothing of useful, I intercepted the raw login request and grabbed a cup of coffee while letting `sqlmap` do its thing.

After some time, it dumped the whole database, with one interesting Database named `secchallenge` with a Table named `users` in it. Here we go, we got some stuff.

```
+----+----------+-------+----------------------------------+
| id | name     | role  | password                         |
+----+----------+-------+----------------------------------+
| 1  | Nux      | user  | c0ddd29267facb1bca6c370d3b918bf1 |
| 2  | Rictus   | user  | c0ddd29267facb1bcacc370d3b918bf2 |
| 3  | Slit     | user  | c0ddd29267facb1bca5c370d3b918bf3 |
| 4  | Ace      | user  | c0ddd29267facb1bc16c370d3b918bf4 |
| 5  | Morsov   | user  | c0ddd29267facb1bcd6c370d3b918bf5 |
| 6  | Furiosa  | user  | c0ddd29267facb1bcc6c370d3b918bf6 |
| 7  | Joe      | admin | c0ddd29267facb1bca6c370d3b918bf7 |
| 8  | Scabrous | user  | c0ddd29267f1cb1bcaac370d3b918bf8 |
+----+----------+-------+----------------------------------+
``` 

Looking at the password field however, nothing came to my mind. It has a bunch of random stuff in it, and clearly, it has some pre and postfix, plus the id appended at the end. But this yielded nothing so I started thinking.

If this section was injectable in the first place, then there should be something we can use. `sqlmap` was kind enough to tell us, that the `user` parameter was the injectable one.

Let's make some kind of dummy SQL statement ourselves, and try to figure out what to enter:
```sql
FROM users SELECT * WHERE user = '$user' and password = '$passwd'
```

The dumped table above tells us, which user we want to log in with (why not get admin rights right away), and the little dummy statement I just wrote, helped me figure out what to enter.

After I successfully logged in as an admin, I got the flag.

The acquired flag is:
```
cd21{HIDDEN}
```

[&#8592; Back to SecChallenge21](/SecChall21)