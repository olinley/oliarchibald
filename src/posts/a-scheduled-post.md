---
title: The curse of infinite parameters
date: 2022-06-18
tags:
- technical seo
- log file analysis

---
![](/images/992382641_115bd44a2d_c.jpg)

For my inaugural post, I want to explore an unexpected issue that's cropped up on a couple of sites I've worked on. Each of which happened after site re-designs, although the issue is more specific to subsequent code changes. The first instance happened on a section of a site that migrated to a new CMS. The other occurred after a client updated to a custom Wordpress theme. For this post, I'll largely concentrate on the former.

Changes to the internal linking logic introduced bugs to how parameterisation was handled within the navigation. Went something like this...

Attempting to access internal links from certain parameterised URL variants would append the new URL path to the existing one. Rather than simply directing to a new page, you end up with an infinite array of URLs like the one below.

> [https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/](https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/ "https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/")

_As you can expect, duplicate content hell._

The issue was initially discovered via server logs. We saw a massive spike in Googlebot requests, with 98% of those concentrated on these iffy parameter URLs. Hints to this abnormal crawler activity were also mirrored in the Excluded section of Search Console's Coverage reports. Thoughts and prayers to anyone reading that's discovered the issue anywhere other than Excluded.

Depending on the number of sub-category variations that are possible on any given site, this can become a huge problem.

Here's how bad it got for us...

![](/images/billionurls.jpeg)  

_And this probably isn't even the full picture._

At this stage I should add that parameterisation isn't utilised on these pages (same case with the other site), and was therefore completely overlooked in the build process. Neither I or the engineers anticipated the issue cropping up.

After some initial hypothesis that we were victims of sophisticated malicious activity, I eventually discovered the root cause via a handful of sites linking to us via (what looked like) custom tracking parameters.

The lazy solution was a few disallow rules in robots.txt. Balance was restored and actual pages being crawled again. 

I've done my part, goodbye.

REVIEW THE BELOW...

The real effort was working with the development team to understand behaviours leading to infinite URL generation. 

In the case of both sites, certain internal links were causing this to happen and identifying those helped us to fix the bug.

To conclude, be sure to regularly check the server logs and Search Console's coverage reports for unusual crawl spikes. Check on testing to see if parameterisation leads to strange internal linking behaviour, and keep a record of where backlinks are pointing to.

I'm not the first to discuss this issue...

Hope this has proven useful. Really interested to know if anyone else has experienced similar issues for assurance I'm not shouting into the void. Let me know in the comments.