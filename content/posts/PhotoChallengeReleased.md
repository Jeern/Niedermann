---
title: "Photo Challenge Released"
date: 2011-07-26T02:00:00+02:00
draft: false
aliases:
    - /2011/07/26/PhotoChallengeReleased.aspx
---
I and a team of 1 graphic artist and 1 sound engineer just finished our first game for Windows Phone 7 called “[Photo Challenge](http://windowsphone.com/s?appid=ab75a012-53a3-e011-986b-78e7d1fa76f8)” a few weeks ago. We planned this pretty simple game to get experience before going for the big hit. The concept has been seen before. A Puzzle Slider where you have to solve a square puzzle with 2x2, 3x3, 4x4, 5x5 or 6x6 squares with one square missing. The Puzzle can be made from you own photos or from some build in ones. Here is a few screenshots:

|||||
|:---:|:---:|:---:|:---:|
|![Screenshot number 1 of Photo Challenge](/images/PhotoChallengeReleased/screenshot_1_thumb.png)|![Screenshot number 2 of Photo Challenge](/images/PhotoChallengeReleased/screenshot_2_thumb.png)|![Screenshot number 3 of Photo Challenge](/images/PhotoChallengeReleased/screenshot_4_thumb.png)|![Screenshot number 4 of Photo Challenge](/images/PhotoChallengeReleased/screenshot_7_thumb.png)| 

The point of the game is to solve the puzzle as fast as possible to make it to the leaderboards. We used the great service Mogade.com for the leaderboards. I plan to cover that aspect in another post.

We are very satisfied with the style and feel of the final game. Even for such a simple game details are important. Similar competing games seems to have been made in Silverlight and thus are more limited with regards to graphics and sound than our XNA game.

Some of the learnings from making the game are:

XNA is a fantastic framework and my own framework for XNA developed over time for PC and XBox could be used directly with very few changes.
WP7 has a fantastic developer platform. IMO better than the iPhones.
The WP7 API’s still lacks a lot of functionality that is present in the iPhone API’s but slowly catching up.
If you use some of the build in “Tasks” like the PhotoChooserTask or the MarketplaceReviewTask you cannot play sounds on the device if it is connected via USB to the computer. Sounds like a small thing but it means you cannot debug on the device ! Really horrible. My solution was to not play music and sound effects in the DEBUG edition.
On the other hand you can debug on the emulator. On the iPhone it is extremly important to debug directly on the iPhone because the iPhone simulator cannot be trusted. I have often experienced that code working in the iPhone simulator was not working on the device. This never happened once for the WP7 emulator and my Samsung Omnia 7. Still it is important to be able to debug on the device since there are things not available in the emulator.
As mentioned the API is somewhat lacking. E.g it is impossible to send images in emails via the EmailComposeTask. It is not possible to integrate facebook in an XNA app and so forth. The last one is supposed to be fixed in the Mango update since it has opened up for mixing Silverlight and XNA. I have not tried it yet. But hope it will be possible. Facebook integration in games is almost a must these days.
Despite being extremely happy with the developing experience for WP7 the iPhone is still an overall better smartphone experience in my opinion (seen from a user perspective). I hope and think WP7 will catch up in time. Competition is good.