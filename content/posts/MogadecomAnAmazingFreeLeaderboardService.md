---
title: "Mogade.com an amazing free Leaderboard Service
"
date: 2011-07-28T01:00:00+01:00
draft: false
aliases:
    - /2011/07/28/MogadecomAnAmazingFreeLeaderboardService.aspx
---
When making the WP7 game [Photo Challenge](http://windowsphone.com/s?appid=ab75a012-53a3-e011-986b-78e7d1fa76f8) we quickly agreed that we needed leaderboards. The problem is that is time consuming to build a scalable high quality leaderboard service tailor made for our game and secondly we would have to pay for server storage for a game that statistically won’t make a dime anyway. This meant that we searched for a ready to use leaderboard service. It wasn’t easy to find. But then a miracle happened. We found the amazing [Mogade.com](http://www.mogade.com/).

[Mogade.com](http://www.mogade.com/) is an incredible fit for our game.

Features:

* Most importantly there is great support from the developer Karl Seguin when you run into trouble (which we did – actually one of our problems led him to find and fix a bug in the service).
* It has an easy to use API. The basic API is Rest based with data transferred in JSon format. This means that is possible to access the leaderboards from any application. I am currently working on an ASP.NET MVC / JQuery based web page to access our leaderboards on the web.
* On top of the REST API there is a WP7 API which you can reference from your game and it looks like an Android API is currently under development.
* When you register an account you gain access to adding any number of games, to each of these games you can add a number of leaderboards.
* Furthermore mogade.com supports achievements and even has some Facebook integration which I haven’t looked into yet.
* There is also statistics. A cheap way to see the popularity of your game. On the AppHubs you can see number of downloads, but on Mogade.com you can actually see how many are playing your game.
* For accessing a leaderboard from your game you use a gamekey a  leaderboardkey and a secret. Each of these are Guid like Id’s. You can fetch pages of 50 scores at a time, and you can get the Rank of a specific user.
* The leaderboards can be defined as High-to-low or low-to-high. Meaning if it is preferable to have a low or a high score.
* Each score is associated with a username, a rank and the date&time at which the score was added.
* In the WP7 API there are a number of ways to define a user in mogade.com terms e.g. one uses your liveid and another the deviceid combined with a username. The last one is the one we use. This means that 2 users with the same username but different phones will be two distinct users on the leaderboard.
To get started you should download the code from Github https://github.com/mogade/mogade-windowsphone and look at the samples there. Furthermore I found this nice blogpost to help me get going http://briansolli.wordpress.com/2011/03/30/online-leaderboard-for-your-wp7-game

In [http://www.niedermann.dk/2011/07/26/PhotoChallengeReleased.aspx]({{<ref "/PhotoChallengeReleased.md">}}) you can see a screenshot of the graphical look of the leaderboard we came up with in Photo Challenge.