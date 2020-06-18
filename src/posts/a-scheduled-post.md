---
title: Infinite pages and crawl traps
date: 2022-06-18
tags:
- technical seo
- log file analysis

---
![](/images/992382641_115bd44a2d_c.jpg)

For the inaugural post, I want to explore an unexpected issue that's cropped up on a couple of sites I've worked on in recent times. Each of which were the result of code changes introduced during site re-designs. The first instance happened on a section of a site that migrated to a new CMS. The other occurred after a client updated to a custom Wordpress theme. Here I'll largely be concentrating on the former.

Changes to the internal linking logic presented unexpected bugs to how parameterisation was handled within the navigation. Attempting to access internal links from certain parameterised URL variants would append the new URL path to the relative link. Rather than simply directing to a new page, you end up with an infinite array of URLs like the one below.

> [https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/](https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/ "https://website.com/category/?random_parameter=wtf/page/6/page/6/page/2/page/6/page/6/page/6/page/6/page/2/page/2/page/3/page/6/page/2/page/6/page/2/page/6/page/6/page/2/page/6/page/3/page/6/page/3/page/2/page/3/page/3/page/3/page/2/page/3/page/3/page/2/page/2/page/6/page/2/page/3/page/3/page/2/page/2/page/2/page/3/page/2/")

_As you can expect, duplicate content hell._

The issue was initially discovered via server logs. We saw a massive spike in Googlebot requests, with 98% of those concentrated on 'infinite pages' like the example above. Hints to this abnormal crawler activity were also mirrored in the Excluded section of Search Console's Coverage reports. Thoughts and prayers if anyone reading has discovered a similar issue in a different tab.

Each of these data sources made it clear that G-bot was continually attempting to crawl A LOT of these pages as a result of getting caught in this near-infinite loop (not to be confused with redirect loops).

For sites with large taxonomies (as was the case with us), this can be a huge problem (think locations, car models or clothing category structures for instance).

How much is 'A LOT'?

![](/images/billionurls.jpeg)

_And this probably isn't even the full picture._

At this stage I should add that parameterisation isn't utilised on these pages (same case with the other site), and was therefore completely overlooked in the build process. Neither I or the engineers anticipated the issue cropping up.

After some initial hypothesis that we were victims of sophisticated malicious activity, I eventually discovered the root cause via a handful of sites linking to us via (what looked like) custom tracking parameters.

The lazy solution was a few robots.txt disallow rules against the offending parameters (_"I've done my bit, goodbye"_). Although balance was restored in the crawl logs, the root of the problem still wasn't solved. 

There's single fix solution here. Any site experiencing similar bugs will be completely unique to that site. The best approach is working from the root parameter (eg. [https://website.com/category/?random_parameter=wtf/](https://website.com/category/?random_parameter=wtf/ "https://website.com/category/?random_parameter=wtf/")) to understand which relative links are causing infinite page duplicates.

REVIEW THE BELOW...

The real effort, however, was locating where

working with the development team to understand behaviours leading to infinite URL generation.

In the case of both sites, certain internal links were causing this to happen and identifying those helped us to fix the bug.

To conclude, be sure to regularly check the server logs and Search Console's coverage reports for unusual crawl spikes. Check on testing to see if parameterisation leads to strange internal linking behaviour, and keep a record of where backlinks are pointing to.

I'm certainly not the first to discuss this issue. Google has some typically [brief documentation](https://support.google.com/webmasters/answer/76401?hl=en) on the matter and there's a great guide [here](https://www.contentkingapp.com/academy/crawler-traps/) worth reading.

However, these particular case studies felt unique enough to shed light on, plus I'm keen to know if anyone else has ever experienced +billion URL requests.