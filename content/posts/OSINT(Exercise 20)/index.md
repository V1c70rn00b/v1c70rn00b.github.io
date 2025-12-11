---
title: "OSINT(Exercise 20)"
date: 2025-03-21
description: This is a writeup on OSINT EXERCISE 20 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 20

For this challenge we are required to find the contents of the challenge itself as it is not in the [link](https://gralhix.com/list-of-osint-exercises/osint-exercise-020/) of the challenge as shown below.

![](Pasted%20image%2020250321120618.png)

However, as we scroll down the page we get a hyperlink on the three dots as shown below but we are redirected to an image that gives us no clue.
![[Pasted image 20250321120831.png]]
This is the image we get when we click on the link embedded on the dots.

![](so-close-i-can-almost-taste-it.png)

After Some time of thinking, I decided to check the challenge on the wayback machine on this [link](http://wayback.archive.org/) and we get our challenge content. From the wayback machine, I was bale to find a bunch of snapshots of the webpage but the snapshot of `August 28, 2023` provided me with the page with the contents of the challenge as shown below.
![](Pasted%20image%2020250321122044.png)

With that we have been able to find our challenge and the task briefing as stated below.
```
The internet is a digital ecosystem in constant transformation. Websites change appearance, domains change owners, businesses open and close, and accounts are created and deleted.  
In July 2023, x.com went from being an almost blank page to redirecting to twitter.com.  
Your task is to go back in time, until the year 2000, and find the following information within the x.com website:

b) The Frequently Asked Questions page.  
c) The list of members of the management team in July.
```

## The frequently asked question page
We shall still use the wayback machine to find the website x.com and search for the FAQ page.
From the task briefing, we are required to go to the year `2000`.  As we look at the site map section, we have different elements as shown below.

![](Pasted%20image%2020250321122900.png)

We can try look at the pages as they are arranged in alphabetical order to see if we shall find the page. This was a tiresome path, I therefore went to the URLs section and searched for the faq page but got nothing of use. Checking the calendar and clicking at different snapshots, I got a faq page which looked as shown below.

![](Pasted%20image%2020250321135022.png)

The page, `secure.x.com/help_faq.asp`  is not there it has an error of 404, I tried searching for the word help as a page on x.com without the subdomain secure as shown below and I was able to find a faq page for the year 2000.

![](Pasted%20image%2020250321145010.png)

Clicking on the link, we are redirected to a page containing more details as shown below.

![](Pasted%20image%2020250321145142.png)

Clicking on the snapshot of `March 4, 2000`, we are redirected to the page shown below.

![](Pasted%20image%2020250321135853.png)


## The list of members of the management team in July
For this section I searched under the URLs section for management but I was unable to find a page on the year 2000, but after modifying my search to search for about as shown below, I was able to find our page.

![](Pasted%20image%2020250321143829.png)

Clicking on the link, we are redirected to the details of when the page was archived as shown below.

![](Pasted%20image%2020250321144112.png)

but after clicking on the `March 2, 2000` snapshot we are redirected to our [page](https://web.archive.org/web/20000302221305/http://x.com:80/about_management.htm)  as shown below.

![](Pasted%20image%2020250321144248.png)

With the above findings, we can confidently say we have finished solving the challenges.

