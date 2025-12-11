---
title: "OSINT(Exercise 6)"
date: 2025-03-06
description: This is a writeup on OSINT EXERCISE 6 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 6

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-006/) we are required to verify if the image posted on the journalist’s twitter page is true. In this lab we shall be learning the skill for data verification as the internet is filled with a lot of misinformation, no matter who says something on the internet, the data has to be verified as in the case above, the journalist is a verified journalist but the photo posted has to be confirmed even though the information seems to be true. In an OSINT investigation, data found has to be verified to be factual be fore coming up with a report.

Below is the task briefing.

```bash
Task briefing: 
On January 19, 2023, a journalist 
with almost 140k followers on Twitter shared an image of a destroyed 
vehicle amidst a large cloud of smoke and fire. The tweet said: “BREAKING: TTP carried out a suicide attack on a police post in Khyber city of Pakistan that killed three Pakistani police officers.“

The photo is not of the event described by the journalist.
a) Verify the statement above.
```

 

![image.png](image.png)

From the tweet the post was made on `January 19, 2023` but when I did a reverse image search I got more than one result form [tin-eye](https://www.tineye.com/search) as shown below some images indexed dating as old as 2009.

![image.png](image%201.png)

With the above findings it is clear the post was a false information source for the case. However doing further research, the bombing scene identified above happened in Baghdad, the image metadata info found form this [link](https://upload.wikimedia.org/wikipedia/commons/e/e4/WaziriyaAutobombeIrak.jpg) form the Tineye reverse image searches. 

![image.png](image%202.png)

The incident happened in august 7, 2006 in Waziriya, Iraq as seen above.