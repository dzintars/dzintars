---
title: "How to Integrate Google Analytics Into Hugo PaperMod Theme"
date: "2024-02-22T12:38:03+02:00"
draft: false
tags: ["Hugo"]
categories: ["How To's"]
---

{{< badge label="Hugo" icon="hugo" color="FF4088" alt="Hugo Badge" >}}

{{< bmc >}}

As I'm setting up my shiny new blog, I stuck at Google Analytics integration.
It looks like official PaperMod lacks some documentation on this topic.

So... there is what worked for me.

Don't use `analytics.google.SiteVerificationTag` directive mentioned somewhere in the PaperMod docs or whatever.

Instead, we will stick with more *idiomatic* [Hugo Docs](https://gohugo.io/templates/embedded/#google-analytics) approach and use this stanza:

```yaml hugo.yaml
services:
  googleAnalytics:
    id: G-XXXXXXX
```

Just stick in somewhere in `hugo.yaml` config file. And make sure, you have right spacing for every line. 2 and 4 spaces in this case for 2nd and 3rd line. Yaml format/language is **very** picky about the spaces.

Next, create a new file in `layouts/_internal/google_analytics.html` and paste the
entire code snippet you got from GA property setup page. And modify it to look like this:

```html google_analytics.html
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.Config.Services.GoogleAnalytics.ID }}"></script>
<script>
	window.dataLayer = window.dataLayer || [];
	function gtag() {
		dataLayer.push(arguments);
	}
	gtag("js", new Date());

	gtag("config", "{{ site.Config.Services.GoogleAnalytics.ID }}");
</script>
```

... or you can just copy/paste this snippet.

As you can see, we are "injecting" our `hugo.yaml#services.googleAnalytics.id` config value into this little template.
I am using `_internal` directory to "tell" that this is something which should be never used in the blog development. But things in the `partials` (as example) directory can be used everywere.

If you have no `layouts` directory, then just create it.

Basically that's it.

Because PaperMod have this line `{{- template "_internal/google_analytics.html" . }}` in the end of `themes/PaperMod/layouts/partials/head.html` file, our `google_analytics.html` partial will be automatically included in the every Hugo page.

And after your first visit to the page, you will see the first visitor stats on your GA
dashboard. If it doesn't work immediately, then something is wrong.

Overall, this is not just PaperMod specific approach. If you inject that GA template in your `<head>` tag, then this approach will work with any theme.

## GitHub

You can use GitHub search to find setup examples. For example use this search query
`path:_internal/google_analytics.html` to find other people GA scripts or... whatever. This is how you learn.

## AdBlockers

If you don't see any local (your own) activity on GA dashboard right away, make
sure you exclude your Hugo site from your adblocker. Or disable it for your site.

## Ethics

Thou... I am very triggered about everything anti-privacy... this is a weird
situation where I do encounter dilemma. Like... I have no intention to track
users, but in the same time I do want to see statistics about my blog. What
articles went good, which were bad. And so on.
IDK... I will think about this case.

## GDPR Compliance

I totally forgot about GDPR rules if GA is enabled.
I should implement that banner. Looking for the solution...

I think I will steal for now from
[LittleBigTech](https://littlebigtech.net/posts/hugo-gdpr-cookie-consent-banner)
with some styling modifications. Looks like I need some `z-index` to bring that
consent up.
By inspecting the F12 developer tools, I can see that it works. There is no
network requests to Google before accepting the consent.
Later I will look closer to this thing. For now it's good enough.

## Resources

Today I accidentally came up to this
[PaperMod discussion](https://github.com/adityatelange/hugo-PaperMod/discussions/248)
