---
title: "OSINT(Exercise 23)"
date: 2025-03-25
description: This is a writeup on OSINT EXERCISE 23 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 23

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-023/), we are required to find the details of the photo used as wallpaper as indicated in the task briefing below.
```
Sometimes online footage contains more information than meets the eye. In October 2023, I uploaded a video to my YouTube channel where, for a split second between minute 3 and 4, my desktop wallpaper is partially visible.

**Your task is to:**  
a) Find the title of the illustration, as given by the artist.  
b) Find how much it would cost for the artist to create a similar piece, in size and detail.
```
Here is the [link](https://www.youtube.com/@gralhix/videos) to the youtube page. From the task briefing, we are told the video was uploaded on `October 2023` and for a split second between minutes 3 and 4, the desktop photo was partially visible. With this information, it was quite clear we are required to play with the `,` and `.` on the youtube video as this symbols help in moving the video ahead or behind by a frame, that is the `,`moves the video behind a frame and `.`moves the video ahead by a frame.

![](Pasted%20image%2020250324141801.png)

Watching the video from minute 3 to 4 while adjusting the frames, I was able to identify the wallpaper as shown below.

![](Pasted%20image%2020250324142127.png)

At `3 minutes and 38 seconds`,we are able to identify the partial wallpaper image after adjusting the frames with `.`to move the video ahead. 

##  Find the title of the illustration, as given by the artist
Below is the image we are required to find the title of the illustration.

![](Pasted%20image%2020250324142358.png)

We can do a reverse image search of the image and see what results, we shall get.

![](Pasted%20image%2020250324143022.png)

As seen above, we have been able to identify the exact image, we can follow the [link](https://www.reddit.com/r/Art/comments/z3ogno/involuntary_rat_queen_me_digital_2022/) and see what is the title of the image.

![](Pasted%20image%2020250324143224.png)

We have been able to identify the illustration but we have to verify if that is truly the title of the illustration, as we have the name `Adam-Scythe`,we can search for the name and see if we can find our image.

![](Pasted%20image%2020250324143734.png)

As shown above, this was the result after searching for the artist at [DeviantArt](https://www.deviantart.com) with the name `Involuntary Rat Queen` and we have been able to verify that trully `Adam-Scythe` is the artist behind the art and the name of the illustration `Involuntary Rat Queen`.

`Answer: iNvoluntary Rat Queen`

## Find how much it would cost for the artist to create a similar piece, in size and detail
For this section, I decided to search for prices of the arts by `Adam-Scythe` on DeviantArt profile of his and got the following details and the price for the image that is similar to the one we found.

![](Pasted%20image%2020250324145025.png)

As seen above, we have been able to find the details of the prices for images in the same class as ours.

`Answer:$190`
