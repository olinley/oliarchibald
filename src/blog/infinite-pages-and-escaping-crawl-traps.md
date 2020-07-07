---
date: 2020-07-10
title: Infinite pages and escaping crawl traps
tags:
- seo
description: ''

---
![](/images/992382641_115bd44a2d_c.jpg)

_Note: this isn't an article about redirect loops, nor encompassing of all types of crawl traps. Rather a niche case study of my own experiences._

For the inaugural post, I want to explore infinite pages and crawl traps. Not a new topic (see [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) for some handy in-depth guides), yet I've dealt with the same specific version of this issue on two occasions, which I think gives me a modicum of authority to shed some helpful advice for anyone else going through the same thing.

In short, I'll be discussing infinite URL paths occurring on parameterised URLs... on pages that don't incorporate parameterisation. Confused? Exactly.

## Finding a needle in a haystack

The two times I ran into crawl traps were the result of code changes, which happened to be introduced during site re-designs. The first instance happened after migrating existing landing pages to a new CMS (Site A). The other occurred after an entire site was migrated to a custom Wordpress theme, although the issue only happened on category pages with pagination (Site B).

On each site, code changes to internal link handling caused query strings to effectively sit within the same folder as the main directory, instead of being treated as its own entity. This leads to the unexpected bug, whereby accessing (certain) internal links appends the new URL path to the existing one, rather than directing to the new page. Infinite URL variants are possible, while page content remains the same.

Enough waffle, let me illustrate the scenario.

_Root category with query string that shouldn't exist..._

> /category/?random_parameter=wtf

_navigate to internal link..._

> /category/?random_parameter=wtf%2Fpage%2F6%2Fpage%2F2/page/2

_navigate to next link..._

> /category/?random_parameter=wtf%2Fpage%2F6%2Fpage%2F2/page%2Fpage%2F6%2Fpage%2F2/page/2

_until you end up with..._

> /category/?random_parameter=wtf/london/bristol/birmingham/chelmsford/manchester/exeter/liverpool/newcastle/glasgow/edinburgh/london/bristol/birmingham/chelmsford/cardiff/exeter/liverpool/braintree/glasgow/norwich/

And so on.

Something as simple as a missing trailing slash in the relative linking logic is causing the bug. 

The issue was initially discovered via the server logs where we saw a massive spike in Googlebot activity. 98% of requests were concentrated on infinite, non-existent pages (similar to the example above), which were rooted to a handful of unknown parameters.

This was simultaneously mirrored in Search Console's Coverage reports, initially spotting an increase in crawlable indexed URLs. Despite applying noindex,nofollow Google only paid attention to the first instruction yet, was continuing to 'discover' an increasing number of infinite pages.

Believe me when I say it got ridiculous...

![](/images/infinite-pages.png)

_And this almost certainly wasn't the full picture._

For sites with large taxonomies (as was the case with us), this can evidently be a huge problem (think locations, car models or clothing based categories for instance). Duplicate content hell with a fine dollop of crawl budget wastage on top.

Given parameterisation wasn't featured on the affected pages, it took a while to figure out where these specific ones came from. ~~After briefly wondering if we'd been spammed,~~ I eventually discovered a handful of sites in the backlink data that were linking with (what appeared to be) custom tracking parameters, which matched those in the server logs and Search Console coverage reports.

## Escaping crawl traps

In our case, Google respected noindex, but nofollow was ineffective in halting crawls (which is what you can see in the Search Console screenshot above). Once the infinite pages dropped from the index, disallowing the offending parameters via robots.txt proved the best solution for stopping spiders in their tracks (_"I've done my bit, goodbye"_).

Balance restored, but the root of the problem still isn't solved.

There's no single fix solution here. If parameterisation _is_ an intended feature on your site then locating relative links leading to endless URL generation should be fairly easy. Site crawl data should also help you get an accurate read on the scale of the issue.

In cases where it _isn't_ utilised, the root cause of parameter related crawl traps won't be so obvious as parameters won't feature in your internal linking. If you've discovered increased crawls to unknown parameter URL variants, it's certainly worth checking your backlinks for sites that may have implemented their own custom tracking.

Whether intended or otherwise, it should be possible to work from the root URL of your problematic parameters (eg. [https://website.com/category/?random_parameter=wtf](https://website.com/category/?random_parameter=wtf "https://website.com/category/?random_parameter=wtf")) to understand where relative links have been erroneously coded in the navigation.

Hopefully this has proved interesting/useful for someone. Thanks for reading.