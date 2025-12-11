---
title: "OSINT(Exercise 3)"
date: 2025-03-03
description: This is a writeup on OSINT EXERCISE 3 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 3

In this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-003/) we are required to find the name and coordinates of the location where the photo was taken from.

```bash
Task briefing: 
In April 2017 Mohamed Abdullahi Farmaajo, the then president of Somalia, visited Turkey. A news agency 
published a photo where he was seen shaking hands with Recep Tayyip Erdoğan, the country’s president. The article did not disclose where the photo was taken. 
Your task is to find out the name and coordinates of the location seen below.
```

![osintexercise003.webp](osintexercise003.webp)

To be able to download the image alone you can download it from this [link](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-003-picture.jpg).

From the  image seen in the tweet above, we can derive some crucial information that we can use to find the location. First, the date when the post was made is April 2017 and the country where the photo was taken is Turkey. With this information we can search the internet and adjust the date to range from first to the last day of the month of April as shown below.

![image.png](image.png)

From the image above after making the search Somalia Turkey, we then click on the tools then select Any time where you will get a drop down and choose custom range where you will be required to put a range of date in American format to which your search will be ranging from as shown below.

![image.png](image%201.png)

After putting the range we get the following searches.

![image.png](image%202.png)

From the image above we can click on the third search as it matches with our tweet post we got for the challenge to see if we can find some interesting information of where the image could be taken, going through the article there is some identifying information, Presidential palace.

![image.png](image%203.png)

With that we can go to google maps and search turkey presidential palace and try identify the location of the image.

![image.png](image%204.png)

From the image we have from the challenge it seems the image was taken in front of a door, we can try search it and see the location of the door.

![image.png](image%205.png)

From the image above, the door seems to be located at the entrance of the palace as identified in the images above. Therefore, from our google maps search, the location could be as indicated below.

![image.png](image%206.png)

We can copy the coordinates and go to google earth pro and find the location and try to play with the time frame in google earth to try and find a picture that was taken showing a portion of the door.

![image.png](image%207.png)

Adjusting the time to 2020, we are able to find the door where it is and with that we now have the location and coordinates of where the picture was taken.

`Answer: Location=Presidential Complex Co-ordinates=39°55'51"N 32°47'58"E`