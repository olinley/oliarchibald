---
date: 2020-07-10
title: Infinite pages and escaping crawl traps
tags:
- seo
description: 'When query strings cause infinite URL paths... on pages that don''t
  use parameterisation. '

---
![](/images/992382641_115bd44a2d_c.jpg)

_Note: this isn't an article about redirect loops, nor encompassing of all types of crawl traps. Rather a niche case study of my own experiences._

For the inaugural post, I want to explore infinite pages and crawl traps. Not a new topic (see [here](https://www.contentkingapp.com/academy/crawler-traps/) and [here](https://www.advancedwebranking.com/blog/avoid-the-seo-spider-trap-how-to-get-out-of-a-sticky-situation/) for some handy in-depth guides), yet I've dealt with the same specific version of this issue on two occasions, which I think gives me a modicum of authority to shed some helpful advice for anyone else going through the same thing.

In short, I'll be discussing infinite URL paths occurring on parameterised URLs... on pages that don't incorporate parameterisation. 

## Confused? Exactly.

The two times I ran into crawl traps were the result of code changes, which happened to be introduced during site re-designs. The first instance happened after migrating existing landing pages to a new CMS (Site A). The other occurred after an entire site was migrated to a custom Wordpress theme, although the issue only happened on category pages with pagination (Site B).

On each site, code changes to internal link handling caused query strings to effectively sit within the same folder as the main directory, instead of being treated as its own entity. This leads to the unexpected bug, whereby accessing (certain) internal links appends the new URL path to the existing one, rather than directing to the new page. Infinite URL variants are possible, while page content remains the same.

Enough waffle, let me illustrate the scenario.

_Root category with query string that shouldn't exist..._

> /category/?random_parameter=wtf

_navigate to internal link..._

> /category/?random_parameter=wtf%2Fpage%2

_onto the next link..._

> /category/?random_parameter=wtf%2Fpage%2F6%2Fpage%2F2

_until you end up with..._

> /category/?random_parameter=wtf%2Fpage%2F6%2Fpage%2F2%2Fpage%2F2page%2F3%2Fpage%2F2%2Fpage%2F297%2F%2Fpage%2F6%2Fpage%2F2%2Fpage%2F2page%2F3%2Fpage%2F2%2Fpage%2F297%2Fpage/297/

And so on.

In this example, something as simple as a missing trailing slash in the relative linking logic for paginated links is causing the bug. 

## Finding the trap

Focusing on Site A for the moment, the issue was initially discovered via the server logs where I noticed a massive spike in Googlebot activity. 98% of requests were concentrated on infinite, non-existent pages, all of which were specific landing page types and rooted to a handful of unknown parameters. 

This was simultaneously mirrored in Search Console's Coverage reports, which showed an increasing numbers of crawlable indexed URLs. Despite applying noindex,nofollow, Google only paid attention to the noindex bit. Access logs continued to show Googlebot getting lost down the infinite page rabbit hole.

Believe me when I say it got ridiculous...

![](/images/infinite-pages.png)

_And this almost certainly wasn't the full picture._

## Why is this a problem?

The amount of possible 'infinite URL' variations that Googlebot can discover are extrapolated based on the number of categories (other other page types) within your taxonomy (think locations, car models or clothing based categories for the most extreme examples). The bigger the site and number of pages, the bigger the trap for crawlers to get lost in and less budget being assigned to the meaningfuls stuff. 

Even on Site B (which consists of roughly 6k indexable pages), paginated links on categories led to +1million pages being crawled. 

Duplicate content hell with a fine dollop of crawl budget wastage on top.

## Escaping the trap

Turning attention to Site A again, it took a while to figure out the root cause of how these URLs came into existence in the first place. ~~After briefly wondering if we'd been spammed,~~ I eventually discovered a handful of sites in the backlink data that were linking with (what appeared to be) custom tracking parameters, which matched those in the server logs and Search Console coverage reports.

Meanwhile on Site B, all infinite URLs included the _?ak_action=reject_mobile_ query string, which belongs to the Jetpack plugin when Mobile Theme is enabled. Jetpack is no longer used by this site and, like Site A, the parameters are some weird legacy stuff that Googlebot has cached.

The exact query strings aren't wholly important, although it's handy to understand where they've come from and be sure you're dealing with parameters that aren't useful in the current version of your site (eg. faceted navigation or other means of changing content). Regardless, any query string will lead to the same outcome.

Solving the issue initially took some time. As mentioned before, Google respected noindex, but nofollow was ineffective. When pages eventually dropped from the index, blocking the offending parameters via robots.txt proved the best solution for stopping spiders in their tracks (_*walks off into the sunset*_).

Balance restored, but the crucial bit in all this is understanding where coding bugs exist within your site's architecture, and fixing it.

Having spent far too long writing this, an infographic would probably have been the best way to explain everything in hindsight. Hopefully this has proved interesting/useful for someone though. Otherwise, feel free to humblebrag if you've discovered infinite page crawls beyond the billions. 

Thanks for reading.