---
title: The curse of infinite parameters
date: 2022-06-18
tags:
- blog

---
![](/images/992382641_115bd44a2d_c.jpg)

Today I want to talk about a bizarre issue I've experienced on two sites; each of which occurred after site re-designs. One happened as a result of migrating to a new CMS, while the other occurred after updating to a custom built Wordpress theme. I'm going to be talking more about the former case.

Essentially code bugs in the front end meant that whenever a parameterised URL variant was accessed, accessing other links on the page would instead append the new path to the existing URL, rather than simply directing to the new page.

Here's an example to help unpack that lofty explanation...

> https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/

Endless variants of duplicate pages are accessible to crawlers, which get trapped in an endless cycle and sap crawl budget. 

But how bad can this get?

![](/images/billionurls.jpeg)

Looking through Search Console's coverage issues can usually provide hints to whether Google is crawling, or indexing, infinite pages like the example above. SC doesn't have an exact name for this and it could fall under any multitude of their listed issues. Here it's best to look out for an abnormally high number (as per above).

Analysing server log requests will provide the most telling sign that crawl budget has been affected. At its peak, Googlebot requests to infinite page parameters made up around 98% of daily crawls.

## How does this happen?

Bugs like this are more likely to occur on pagetypes that don't typically utilise any form of parameterisation. Therefore the possibility of this bug occurring isn't considered in the build process.

In our case a blanket noindex rule on parameter URLs saved us from the hell of Google indexing these duplicate pages.

Google was discovering some old backlinks to (presumably) affiliate links in the farthest reaches of the netherweb...

Things to cover:

What was the cause

Worst case scenario - no block on parameters, indexable URLs

What did/didn't work - noindex,nofollow robots.txt

SEO handling - robots.txt