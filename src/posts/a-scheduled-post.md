---
title: The curse of infinite parameters
date: 2022-06-18
tags:
- blog

---
![](/images/992382641_115bd44a2d_c.jpg)

For my inaugural post, I want to explore an unexpected issue that's cropped up on a couple of sites I've worked on. Each of which happened after site re-designs, although the issue is more specific to subsequent code changes. The first instance happened on a section of a site that migrated to a new CMS. The other occurred after a client updated to a custom Wordpress theme. For this post, I'll largely concentrate on the former.

Server logs showed a massive spike in Googlebot requests to parameters

Mirrored in Search Console's Excluded section

Changes to the internal linking logic introduced bugs to how parameterisation was handled within the navigation. Went something like this...

Attempting to access internal links from certain parameterised URL variants would append the new URL path to the existing one. Rather than simply directing to a new page, you end up with an infinite array of URLs like the one below.

> [https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/](https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/ "https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/")

As you can expect, duplicate content hell.

A massive spike in Googlebot requests to these parameter variants further solidified the issue. 98% of requests were being made 

At this stage I should add that parameterisation isn't utilised on the affected page types of either site, so the issue was completely unforeseen. 

Each required code changes to the internal linking logic and didn't factor in how parameterised URLs should be handled. But they didn't necessarily need to, given that parameterisation 

I'm going to be talking more about the former case.

Essentially code bugs in the front end meant that whenever a parameterised URL variant was accessed, accessing other links on the page would instead append the new path to the existing URL, rather than simply directing to the new page.

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