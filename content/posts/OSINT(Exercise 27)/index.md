---
title: "OSINT(Exercise 27)"
date: 2025-03-29
description: This is a writeup on OSINT EXERCISE 27 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 27

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-027/), we are required to find the following details outlined in the task briefing below.

```jsx
The image below shows a group of people sitting in front of a large 
screen that reads “Lectura en Movimiento en Lima”. A speaker can be seen
standing on the left-hand side in front of three large flags.

Your task is to:

a) Name the speaker.
b) Identify what he was wearing on his lapel.
c) Find footage of his speech.
```

Below is the image we are required to investigate.

![osintexercise027.png](osintexercise027.png)

From the above image we can see some clear details we can use. First the event name `lectura en movimiento en lima` . This information is not sufficient enough we can start by doing some reverse image search.

## Name the speaker.

We shall start first by performing a reverse image search to identify this event.

![image.png](image.png)

we have found some interesting sources, clicking on this [link](https://oei.int/oficinas/peru/noticias/lectura-en-movimiento-en-lima-una-iniciativa-que-une-a-14-entidades-para-promover-la-lectura-en-el-pais/) from one of our identified sources, I got the blog shown below with some images and details about the members who were in the event whom we can identify and features that are similar.

![image.png](image%201.png)

![image.png](image%202.png)

Reading through the blog, we get our person `Juan Carlos Ruiz` but we shall verify if truly he is aour speaker.

![image.png](image%203.png)

![image.png](image%204.png)

As seen above, we have been able to put some details that makes the person identified be the true person as from the image form the blog it is just a clearer zoomed version of the challenge image.

We found his full name on Linkedin as shown below.

![image.png](image%205.png)

`Answer: Juan Carlos Ruiz Rodriguez`

## what he was wearing on his lapel

For this section, we shall search for our person and try to identify what he was wearing on his lapel.

![Juan Carlos OEI Perú.JPG](Juan_Carlos_OEI_Per.jpg)

This is the image from the blog page we found earlier showing our person of interest more clearer.

![image.png](image%206.png)

After searching our person online, we are able to identify him more closer showing the item on his lapel as shown below from this [link](https://www.infobae.com/america/agencias/2023/07/19/peru-y-la-oei-promocionan-la-lectura-en-las-calles-y-el-transporte-publico-de-lima/)

![image.png](image%207.png)

`Answer: OEI logo` 

## Find footage of his speech

I searched for the videos of the event and found a youtube [link](https://www.youtube.com/watch?v=WVU-ei4Dunk) with the speech of the director starting at `9:30` and ending at `18:14`

![image.png](image%208.png)