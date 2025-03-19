---
title: "OSINT(Exercise 18)"
date: 2025-03-19
description: This is a writeup on OSINT EXERCISE 18 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [GEOLOCATION, INTELLIGENCE, VERIFICATION]
categories: [OSINT]
---
# OSINT(eXERCISE 18)
For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-018/) we are required to find the details described in the task briefing as shown below.
```
In May 2023, the world witnessed the coronation of King Charles III at Westminster Abbey in London. Following the ceremony, and similarly to the previous regent, the monarch travelled through the city in a lavish carriage. The route of the 2023 royal procession differed from that of 1953.

Your task is to:  
a) Identify from which exact door Queen Elizabeth II left Westminster Abbey after her coronation.  
b) Measure the distance travelled by the Queen’s carriage, following the 1953 coronation.  
c) Estimate the average speed at which the Queen’s carriage travelled.
```

## Identify from which exact door Queen Elizabeth II left Westminster Abbey after her coronation.

For this section we shall first start by searching for the coronation celebration of 1953 on youtube, as the event was witnessed world wide, a video of the celebration can not miss as it was streamed.
![[Pasted image 20250319103059.png]]
From the video identified above, I was able to locate a scene where we see the queen leaving the Westminster abbey as shown below.
![[Pasted image 20250319103139.png]]
From the video we have been able to identify from which door she left but still there is no enough information for us to use to identify the exact door as the Westminster abbey contains many doors that leave the vicinity as shown below from google earth pro.
![[Pasted image 20250319104221.png]]
However doing further research on youtube I found another video that is a bit old but we can clearly see the queen entering the vicinity through the same door as shown below.
![[Pasted image 20250319104500.png]]
Watching the video further, we get a glimpse of some features we can use to identify from which side the queen left the vicinity as shown below.
![[Pasted image 20250319104802.png]]
After ananlysis, I was able to identify the pattern on the video on the building as shown below.
![[Pasted image 20250319105244.png]]
We can now clearly identify which door it was as shown below.
![[Pasted image 20250319105421.png]]

`Answer: 51°29'58.10"N, 0° 7'42.52"W`

## Measure the distance travelled by the Queen’s carriage, following the 1953 coronation
For this section, I had to search for the coronation walk that the queen did in order to find the streets she passed through and use them to measure distance on google earth pro.
![[Pasted image 20250319110142.png]]
From the above image, following the [link](https://www.nationalarchives.gov.uk/education/resources/significant-events/coronation-of-elizabeth-ii-1953/) ,we get to this page that has a map of the streets and how she passed through them towards the Buckingham palace as shown below.
![[Pasted image 20250319110417.png]]

![[PC-22_11_1953-Eliz-II-coronation-map.jpg]]
Using this map we can now map the landmarks where turns were made and use them to measure the distance.
![[Pasted image 20250319114108.png]]
Form the image above, we can see clearly the path followed by the queen the distance covered I got it to be as shown below in kilometers and miles.
![[Pasted image 20250319114229.png]]
`7.89 kilometers`
![[Pasted image 20250319114315.png]]
`4.90 miles`
However, searching for the actual distance covered during the return to the palace form the Westminster abbey I got it to be 5 miles and 8.2 kilometers
![[Pasted image 20250319114956.png]]
Following the link identified by AI, I got this [page](https://storymaps.arcgis.com/stories/af1e1b457b9040bb8a8c8131bc577dda) that details how the queen travelled back to the palace and the distance covered as shown below.
![[Pasted image 20250319115144.png]]
Hence the distance covered was 5 miles(8.2 kilometers)
`Answer: 5miles(8.2 kilometers)`

## Estimate the average speed at which the Queen’s carriage travelled
From the [page](https://www.nationalarchives.gov.uk/education/resources/significant-events/coronation-of-elizabeth-ii-1953/) we found initially containing the routes the queen covered back to the palace also had details about the time she arrived at each spot as shown below.
![[Pasted image 20250319115459.png]]
From this time table we can calculate the time taken to arrive at the palace by finding the difference between arrival and departure times using this online [calculator](https://www.calculator.net/time-duration-calculator.html) as shown below.
![[Pasted image 20250319115825.png]]
From the calculator we have found the time difference in different time formats from hours to seconds. Now to calculate the speed we can do it in two ways `miles per hour or kilometers per hour`  that is dividing the distance covered by time taken (Speed=distance/time) using this online [calculator](https://www.calculator.net/speed-calculator.html) as shown below.
![[Pasted image 20250319120356.png]]
Speed in miles per hour is 3miless/hour as shown above.
![[Pasted image 20250319120614.png]]
Speed in kilometers per hour is 4.92kilometers/hour

`Answer: 3 miles/hour or 4.92 kilometers/hour`




