---
title: "OSINT(Exercise 10)"
date: 2025-03-11
description: This is a writeup on OSINT EXERCISE 10 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 10

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-010/) we are required to analyze the photos in the tweet to answer the questions that are in the task briefing as detailed below.

```bash
A Twitter user shared three photos of an event. 
Please answer the following questions:

a) Which event is being celebrated in the photos? 
b) Which two photos were taken by the same person?
c) The two photos mentioned above were taken in the same city. The photographer was previously in a different city. Find out the name of 
that city.
```

![osintexercise010.webp](osintexercise010.webp)

## Which event is being celebrated in the photos?

For this section we can read the tweet to see if we can find some interesting information that can help us identify the event.We have some information we can search to find the event `zangbeto dolls` , `African voodoo festival and mysticism` .

![image.png](image.png)

Searching online, the zangbeto doll is a traditional  Vodún guardians of the night among the Ogu (or Egun) people of Benin, Togo, and Nigeria. This tells us the festival is usually conducted in west African countries. Looking at the images section, I found an image which on clicking I got other images and the name of the festival as shown below.

![image.png](image%201.png)

`Answer: Benin voodoo festival`

## Which two photos were taken by the same person?

From the previous question, we saw one image from which we can search the photographer and see if he has more images for the same event.

![image.png](image%202.png)

Clicking on the highlighted image above, I got more images for the same but there was one which provided more details from getty images as shown below.

![image.png](image%203.png)

Found the name of the photographer, we can search the name in the getty images gallery and see more photos that he has.

![image.png](image%204.png)

and we have found another photo by the same photographer. Hence the two photos that were taken by the same photographer are as highlighted below.

![image.png](image%205.png)

## The photographer was previously in a different city. Find out the name of that city.

For this section we can search for the photographer’s social media and see if he has posted some information of his whereabouts, I will start with my favorite platform, twitter.

![image.png](image%206.png)

I found a post by him, clicking on it we get details of where he was as shown below.

![image.png](image%207.png)

But for us to verify we shall search the place in google maps and see where it is .

![image.png](image%208.png)

We now have our answer, he was in Cotonou Benin.

`Answer:Cotonou, Benin`