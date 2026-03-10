---
title: "Cyrillic Confusion"
date: 2026-03-08
description: This is a writeup on Bellingcat OSINT challenge series of Maritime Mysteries
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Bellingcat" # Here you can write a small summary of the post if needed
series: [Maritime Mysteries]
series_oredr: 3
tags: [OSINT]
categories: [Bellingcat]
---
# Cyrillic Confusion

Challenge description

```jsx
InNovember 2024, two underwater data cables in the Baltic Sea experienced
faults, disrupting communications between European nations and igniting
suspicions of sabotage. A Chinese cargo vessel, the YUI PENG 3, quickly
became a focus for both investigators and the online open source 
research community.

Some mistakenly suggested that this port log showed the vessel's captain wasa Russian citizen 
(they confused a temporary harbor pilot with the ship's captain).
 
But a key question emerged: could the authenticity of the log be verified?

What is the original URL for this log? (don't include the "http://" or "https://")
```

The image can be accessed from the following [link](https://challenge.bellingcat.com/assets/YuiPeng3Log-CJv3Y2-b.png).

First I began with a reverse image search and stumbled upon the following [article](https://sotaproject.com/news/90176). We have a Russian port called `Ust-Luga.` We could search for the logs of the port using Yandex as it is Russian but we will have to translate it to Russian for easy search as **`журналы порта усть-луга`** which translates to `Logs of the port of Ust-Luga.`

The first link I found looks promising but it was the wrong answer, we can search for it on the wayback machine.

![image.png](image.png)

From the wayback machine, the captures from 27th November to the 2nd of December, show a capture containing a partial table.

However, playing around with the url, I found one capture that contained the details of the ship log as shown below.

![image.png](image%201.png)

Answer: [`skap.pasp.ru/Move/InOutMoveList/165759?harb=UL`](http://skap.pasp.ru/Move/InOutMoveList/165759?harb=UL)