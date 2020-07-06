---
date: 2020-07-08
title: Infinite pages and escaping crawl traps
tags:
- seo

---
![](/images/992382641_115bd44a2d_c.jpg)

_Note: this isn't an article about redirect loops, nor encompassing of all types of crawl traps. This is a niche case study of my own experiences._

For the inaugural post, I want to explore infinite pages and crawl traps. Not a new topic (see [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) for some handy in-depth guides), yet I've dealt with the same niche version of this issue on two occasions, which gives me a modicum of authority to shed some (hopefully) helpful advice for anyone else going through the same thing.

In short, this post is about infinite URL paths occurring on parameterised URLs... on pages that don't incorporate parameterisation. Confused? Good, then read on.

## Finding a needle in a haystack

The two times I ran into crawl traps were the result of code changes introduced during site re-designs. The first instance happened on a section of a site that migrated to a new CMS (the main focus for this post). The other occurred after an entire site was migrated to a custom Wordpress theme.

After going live with updated landing pages on a new headless CMS, changes to the relative linking logic presented unexpected bugs to how parameter URLs were handled within the navigation.

The twist here is that (as alluded to earlier) parameterisation wasn't an intended feature on these pages and therefore completely overlooked in the build/staging process. Neither I or the engineers anticipated the issue cropping up.

Attempting to access internal links from certain parameterised URL variants would append the new URL path to the existing one. To illustrate, rather than simply directing to a new page you'd end up with a seemingly infinite generation of URLs like this:

_Root parameter..._

> /category/?random_parameter=wtf

_navigate to internal link..._

> /category/?random_parameter=wtf/london/

_navigate to next link..._

> /category/?random_parameter=wtf/london/bristol/

_until you end up with..._

> /category/?random_parameter=wtf/london/bristol/birmingham/chelmsford/manchester/exeter/liverpool/newcastle/glasgow/edinburgh/london/bristol/birmingham/chelmsford/cardiff/exeter/liverpool/braintree/glasgow/norwich/

And so on.

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