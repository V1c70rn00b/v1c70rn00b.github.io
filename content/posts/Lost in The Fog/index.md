---
title: "Lost in the Fog"
date: 2026-03-05
description: This is a writeup on Bellingcat OSINT challenge series of globetrotters
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Bellingcat" # Here you can write a small summary of the post if needed
series: [Bellingcat Globetrotters]
series_oredr: 5
tags: [OSINT]
categories: [Bellingcat]
---
# Lost in the Fog

Challenge description

```jsx
For this photo Nick was stood in a field shrouded in a thick fog. 
At least it looks like there's an airport nearby.But what airport is it? 

It's not complex if you know how to look!

What is the ICAO code for the airport?
```

The image can be found from this [link](https://challenge.bellingcat.com/assets/foggy_field-CVCShq-A.jpg).

Since the image is quite difficult to find  a lot of stuff, we could use extra [tool](https://onlineexifviewer.com/) to view the exif data of the image to find location of the image as shown below.

![image.png](image.png)

Following the GPS coordinates, we are led near to `Lundy Island` as shown below.

![image.png](image%201.png)

Looking closely to the image, we can see a figure looking like an airplane as shown below.

Searching online what ICAO is, we get to find that it is `International Civil Aviation Organization.` This has made it easier for us to search online for ICAO code for airport in Lundy island

![image.png](image%202.png)

The search result online.

![image.png](image%203.png)

The result found is from this [link](https://airportguide.com/airport/info/EGZV).

Answer: `EGZV`