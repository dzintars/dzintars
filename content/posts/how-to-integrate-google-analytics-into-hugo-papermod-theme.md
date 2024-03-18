---
title: "How to Integrate Google Analytics Into Hugo PaperMod Theme"
date: "2024-02-22T12:38:03+02:00"
draft: false
tags: ["Hugo"]
categories: ["How To's"]
---

As I'm setting up my shiny new blog, I stuck at Google Analytics integration.
It looks like official PaperMod lacks some documentation on this topic.

So... there is what worked for me.

Don't use `analytics.google.SiteVerificationTag` directive.

Use only `googleAnalytics: G-xxxxxxx` directive.

Create new file in `layouts/_internal/google_analytics.html` and paste the
entire code snippet you got from GA property setup page.

Basically that's it.

This partial will be automatically included in every Hugo page. And after your
first visit to the page, you will see the firs visitor stats on your GA
dashboard. If it doesn't work immediately, then something is wrong.

Use GitHub search to find setup examples. For example use this search query
`path:_internal/google_analytics.html`.

## Ethics

Thou... I am very triggered about everything anti-privacy... this is a weird
situation where I do encounter dilemma. Like... I have no intention to track
users, but in the same time I do want to see statistics about my blog. What
articles went good, which were bad. And so on.
IDK... I will think about this case.

## Resources

Today I accidentally came up to this
[PaperMod discussion](https://github.com/adityatelange/hugo-PaperMod/discussions/248)
