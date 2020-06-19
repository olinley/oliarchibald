---
date: 2020-06-23
title: Infinite pages and escaping crawl traps
tags:
- technical seo

---
![](/images/992382641_115bd44a2d_c.jpg)

_Note: this isn't an article about redirect loops, nor encompassing of all types of crawl traps. This is a niche case study, enjoy._

For the inaugural post, I want to explore the issue of infinite pages and crawl traps. A topic that's been covered numerous times before (see [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) for some particularly handy guides). However, the circumstances surrounding my own experiences were similar in nature on two different sites...

For the inaugural post, I want to explore an unexpected issue that's cropped up on a couple of sites I've worked on in recent times. Each of which were the result of code changes introduced during site re-designs. The first instance happened on a section of a site that migrated to a new CMS. The other occurred after a client updated to a custom Wordpress theme. Here I'll largely be concentrating on the former.

Changes to the internal linking logic presented unexpected bugs to how parameterisation was handled within the navigation. Attempting to access internal links from certain parameterised URL variants would append the new URL path to the relative link. Rather than simply directing to a new page, you end up with a seemingly infinite generation of URLs that looks something like:

Root parameter...

> /category/?random_parameter=wtf

navigates to internal link...

> /category/?random_parameter=wtf/london/

navigate to next link...

> /category/?random_parameter=wtf/london/bristol/

until you end up with...

> /category/?random_parameter=wtf/london/bristol/birmingham/chelmsford/manchester/exeter/liverpool/newcastle/glasgow/edinburgh/london/bristol/birmingham/chelmsford/cardiff/exeter/liverpool/braintree/glasgow/norwich/

And so on.

As you can expect, duplicate content hell.

## Shhh, I'm hunting spiders

The issue was initially discovered via the server logs where we saw a massive spike in Googlebot requests; 98% of which were concentrated on infinite, non-existent pages like the example above. 

This was simultaneously mirrored in Search Console's Coverage reports, initially spotting an increase in crawlable indexed URLs. Despite applying noindex,nofollow Google only paid attention to the first instruction yet, was continuing to 'discover' an increasing number of infinite pages.

Believe me when I say it got ridiculous...

![](/images/infinite-pages.png)

_And this almost certainly wasn't the full picture._

For sites with large taxonomies (as was the case with us), this can evidently be a huge problem (think locations, car models or clothing based categories for instance).

At this stage I should add that parameterisation wasn't an intended feature on these pages (same case with the other site), and was therefore completely overlooked in the build process. Neither I or the engineers anticipated the issue cropping up.

After some brief hypothesising that we were victims of sophisticated malicious activity, I eventually discovered the root cause via a handful of sites linking to us via (what looked like) custom tracking parameters.

If, on the other hand, parameterisation _is_ an intended feature on areas of your site (such as faceted navigation), you'll just as likely be able to spot similar patterns via a site crawl, or a good old Google _site:_ search for the offending parameters.

## Escaping crawl traps

In our case, Google respected noindex, but nofollow was ineffective in halting crawls. Once the infinite pages dropped from the index, disallowing the offending parameters via robots.txt proved the best solution for stopping spiders in their tracks (_"I've done my bit, goodbye"_).

Balance restored, but the root of the problem still isn't solved.

There's no single fix solution here. If parameterisation is an indended feature on your site then locating which relative links lead to infinite page generation should be relatively easy. 

On affected pages where parameterisation isn't utilised, the root cause won't be so obvious. However, once you've identified the problematic parameters, you should be able to work from the root URL (eg. [https://website.com/category/?random_parameter=wtf](https://website.com/category/?random_parameter=wtf "https://website.com/category/?random_parameter=wtf")) to understand which internal links are problematic.

Whether intended or otherwise, the source of the issue is likely a result of how relative links have been erroneously coded.

I'm certainly not the first to discuss this issue. There's some handy guides [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) worth reading, and Google has some typically [brief documentation](https://support.google.com/webmasters/answer/76401?hl=en) on the matter.

Regardless, the circumstances surrounding this particular case felt niche enough to shed light on. Plus I'm keen to know if anyone else out there has ever experienced crawl rates in the billions.