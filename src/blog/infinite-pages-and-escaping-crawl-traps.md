---
date: 2020-07-04
title: Infinite pages and escaping crawl traps
tags:
- technical seo

---
![](/images/992382641_115bd44a2d_c.jpg)

_Note: this isn't an article about redirect loops, nor encompassing of all types of crawl traps. This is a niche case study of my own experiences, enjoy._

For the inaugural post, I want to explore infinite pages and crawl traps: a topic that's been covered numerous times before (see [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) for some handy in-depth guides). Yet, having dealt with this issue on multiple occasions, I perhaps have a modicum of authority on the matter and feel it's worth shedding further light on what worked/didn't work from a personal standpoint.

The two times I got caught in crawl traps were the result of code changes introduced during site re-designs. The first instance happened on a section of a site that migrated to a new CMS. The other occurred after an entire site was migrated to a custom Wordpress theme. Here I'll largely be concentrating on the former.

## Finding a needle in a haystack

After going live with new landing pages on a new headless CMS, changes to the relative linking logic presented unexpected bugs to how parameter URLs were handled within the navigation.

The twist here is that parameterisation wasn't an intended feature on these pages and was therefore completely overlooked in the build/staging process. Neither I or the engineers anticipated the issue cropping up.

Attempting to access internal links from certain parameterised URL variants would append the new URL path to the existing one. To illustrate, rather than simply directing to a new page you end up with a seemingly infinite generation of URLs like this:

Root parameter...

> /category/?random_parameter=wtf

navigate to internal link...

> /category/?random_parameter=wtf/london/

navigate to next link...

> /category/?random_parameter=wtf/london/bristol/

until you end up with...

> /category/?random_parameter=wtf/london/bristol/birmingham/chelmsford/manchester/exeter/liverpool/newcastle/glasgow/edinburgh/london/bristol/birmingham/chelmsford/cardiff/exeter/liverpool/braintree/glasgow/norwich/

And so on.

As you can expect, duplicate content hell.

The issue was initially discovered via the server logs where we saw a massive spike in Googlebot activity. 98% of requests were concentrated on infinite, non-existent pages (similar to the example above), which were rooted to a handful of unknown parameters.

This was simultaneously mirrored in Search Console's Coverage reports, initially spotting an increase in crawlable indexed URLs. Despite applying noindex,nofollow Google only paid attention to the first instruction yet, was continuing to 'discover' an increasing number of infinite pages.

Believe me when I say it got ridiculous...

![](/images/infinite-pages.png)

_And this almost certainly wasn't the full picture._

For sites with large taxonomies (as was the case with us), this can evidently be a huge problem (think locations, car models or clothing based categories for instance) where crawl budget can get utterly sapped.

As mentioned, parameterisation wasn't featured on the affected pages so it took a while to figure out where these specific ones came from. ~~After some brief hypothesising that we were somehow being spammed,~~ I eventually discovered via backlink data that a handful of sites were linking to a couple of these landing pages with (what appeared to be) custom tracking parameters.

If, on the other hand, parameterisation _is_ an intended feature on areas of your site (such as faceted navigation), you'll just as likely be able to spot similar patterns via a site crawl, or a good old Google _site:_ search for the offending parameters.

## Escaping crawl traps

In our case, Google respected noindex, but nofollow was ineffective in halting crawls. Once the infinite pages dropped from the index, disallowing the offending parameters via robots.txt proved the best solution for stopping spiders in their tracks (_"I've done my bit, goodbye"_).

Balance restored, but the root of the problem still isn't solved.

There's no single fix solution here. If parameterisation is an indended feature on your site then locating which relative links lead to infinite page generation should be relatively easy.

On affected pages where parameterisation isn't utilised, the root cause won't be so obvious. However, once you've identified the problematic parameters, you should be able to work from the root URL (eg. [https://website.com/category/?random_parameter=wtf](https://website.com/category/?random_parameter=wtf "https://website.com/category/?random_parameter=wtf")) to understand which internal links are problematic.

Whether intended or otherwise, the source of the issue is likely a result of how relative links have been erroneously coded.

I'm certainly not the first to discuss this issue. There's some handy guides [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) worth reading, and Google has some typically [brief documentation](https://support.google.com/webmasters/answer/76401?hl=en) on the matter.

Regardless, the circumstances surrounding this particular case felt niche enough to shed light on. Plus I'm keen to know if anyone else out there has ever experienced crawl rates in the billions.