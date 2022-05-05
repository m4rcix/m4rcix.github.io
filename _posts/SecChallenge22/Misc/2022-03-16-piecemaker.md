---
layout: post
title: Piecemaker
description: Thank god there were only two
tags: misc secchallenge22
permalink: /sc22/misc/piecemaker
hidden: 1
---

## Description

There is a secret hidden deep in the DC universe. It's a conspiracy... Or is it?

- Author: tcs
- Attachments: [piecemaker.tar.gz](/media/sc22/misc/piecemaker/piecemaker.tar.gz)

## Solution

### 0x1 initial puzzle

Extracting the files from the given archive, we get tons of puzzle pieces. The task seems simple, put it together. With a bit of python magic, we can remove most of the green background, and put the puzzle pieces together using `GIMP` for example.
```python
from PIL import Image
import glob
from pwn import *

images = glob.glob("../*.jpg")

pr = log.progress("Status")

count = 0

for image in images:
    img = Image.open(image)
    img = img.convert("RGBA")
    datas = img.getdata()


    newData = []
    for item in datas:
        if item[1] > 240 and item[2] < 10:
            newData.append((255, 255, 255, 0))
        else:
            newData.append(item)

    img.putdata(newData)
    img.save(f"{count}.png", "PNG")
    count += 1
    pr.status(f'{count / len(images) * 100}')

pr.success("Done")
```

![image](/media/sc22/misc/piecemaker/puzzle.png)

This had me puzzled (haHa) for a while, but after googling some languages commonly used in CTFs, I found out about `Klingon`. Using some `ABCs` found online I could decode the following text on the image:
```
this is christopher smith known as p
eacemaker i love formula one aka f1
as well as f2 and f3 but you know wh
at would be even better f5 i also lo
ve java but scripting should be bann
ed the whole password is in gold

i cherish peace with all my heart
i dont care how many men women and
children i need to kill to get it
```

This next part required a hint from the author, since everyone was stuck on this part for a moment. The hint came:
```
We've just received an urgent transmission from Peacemaker telling us his favorite dinosaur is stegosaurus.
```
Alright, a stego challenge, nothing I haven't knew before. But this finally led me to google for `stego f5 java` insead of `f5 java`. And trust me, it made all the diference (Why would you name a firewall company after a stego tool or vice versa???). I wrote a `bash` script to extract whatever is hidden inside the images (just so you see other things on this blog besides python scripts):
```bash
#!/bin/bash

declare -i c=0

mkdir -p out/

for f in piecemaker/*.jpg
do
	java -jar f5.jar x -e out/$c.gz $f -p "i cherish peace with all my heart i dont care how many men women and children i need to kill to get it";
	c+=1	
done

cd out/
rm 91.gz 92.gz 93.gz 94.gz 95.gz 96.gz 97.gz 98.gz 99.gz 100.gz 101.gz 102.gz
rm 103.gz 104.gz 105.gz 106.gz 107.gz 108.gz 109.gz 110.gz 111.gz

for file in *.gz
do
	tar zxvf $file
done
```

### 0x2 joyride to the flag

At this point, I know, I had the flag... there was only a small problem, again, it was in pieces. Luckily the creator didn't make any more matryoshkas, or else I would have been sitting here for quite some time.
Piece by piece, I put together the puzzle, which contained the flag as an image:
```
cd22{HIDDEN}
```

[&#8592; Back to SecChallenge22](/sc22)
