---
title: "Spam in 2018"
date: 2018-09-24T20:14:00+02:00
draft: false
---
When I published my blog with the shiny new format this morning, SPAM started flowing in IMMEDIATELY. Less than 10 minutes after I published, the first "Viagra" message was added as a comment.

And then it flowed in, in a steady pour, approximately one spam message every 45 minutes.

My old blog used Akismet to filter out the worst spam. Unfortunately Akismet does not work well with Staticman yet, instead I decided to try ReCaptcha. 

Pretty easy to implement, the hardest part was finding the documentation that tied Staticman and ReCaptcha together. 

This is where I found it in the end:
https://github.com/eduardoboucas/staticman-recaptcha

I knew spam would come, but not at that rate. I naively hoped that in 2018 the bad guys would have found more profitable areas of revenue than spamming blog comments. Turned out not to be the case.

I hope that ReCaptcha will effectively stop it.

*Update Nov 4, 2018: Recaptcha was only partially effective and stopped about two thirds of the spam. When I added a [honeypot](https://stackoverflow.com/questions/36227376/better-honeypot-implementation-form-anti-spam) to the mix spam was finally eliminated.*
