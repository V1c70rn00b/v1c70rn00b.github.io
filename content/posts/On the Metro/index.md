---
title: "On the metor"
date: 2026-03-12
description: This is a writeup on Bellingcat OSINT challenge series of Urban Exploration
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Bellingcat" # Here you can write a small summary of the post if needed
series: [Urban Exploration]
series_oredr: 5
tags: [OSINT]
categories: [Bellingcat]
---
# On the Metro

Challenge description

```jsx
Watching the metro go past.Where is the photographer, 
what is the line, and where is the train heading?

What is the destination station for this train?
```

The image to the challenge can be accessed from this [link](https://challenge.bellingcat.com/assets/metro-BKEDT34w.jpg)

We can start by doing a reverse image search using google lens.

![image.png](image.png)

Let's focus on the natural light at the station, which indicates that the station is located on the surface. Looking at the list of metro stations in Prague, we find that **Hůrka** is one of the stations situated on the surface.

![image.png](image%201.png)

[https://en.wikipedia.org/wiki/List_of_Prague_Metro_stations](https://en.wikipedia.org/wiki/List_of_Prague_Metro_stations)

On Google Maps, a panorama view reveals the same warning stickers. The windows that emit the light are positioned on the wall opposite them.

![image.png](image%202.png)

On Street View and locate these windows; doing so reveals that the train is heading west.

![image.png](image%203.png)

An arrow indicates the direction in which the train is traveling.

![image.png](image%204.png)

The terminus of this line is Zličín

Answer: `Zličín`