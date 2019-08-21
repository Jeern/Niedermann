---
title: "Netlify Minify"
date: 2019-08-21T20:52:00+02:00
draft: false
---
if you use the service [Netlify](https://netlify.com) for a static site you get the option of doing automatic minification of your javascript when the site is deployed.

Its as easy as setting a checkbox.

![Netlify Asset Optimization](/images/NetlifyMinify/AssetOptimization.png)

I recently used on a site, and to my surprise it did not work (since [Netlify](https://netlify.com) usually works like a charm)

After doing some back and forth with Netlifys support and trying the advice in [this post](https://community.netlify.com/t/common-issue-does-netlify-optimize-every-single-asset-my-site-uses/152?utm_source=helpdesk&utm_medium=asset-optimization&utm_campaign=community_tracking) it still did not work  unfortunately.

In the site I was testing I had several scripts and none of them where minified.

Then I turned to good old binary search to find the cause of the problem, i.e. removing half of the code then testing again, etc.

It turns out that [Netlify](https://netlify.com) uses a very sensitive minification algorithm (which is custom made according to the supporter I talked to).

- If it fails for one script the minification does not run for any script.
- In my case I had two scripts that failed, 
	1. I had an object that had 2 consecutive kommas by mistake. Apparently that javascript was accepted by the browser but not by the minifier.
	1. I also had a script that uses the const keyword - apparently not accepted by the minifier.

[Netlify](https://netlify.com) is all in all an impressive service, but it is helpful for you to know quirks as the above.

And since their minifier is custom made it is not possible to test it on your own machine.