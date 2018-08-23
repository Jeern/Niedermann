---
title: "How to repair GreatNews database"
date: 2009-05-06T01:00:00+01:00
draft: true
aliases:
    - /2009/05/06/HowToRepairGreatNewsDatabase.aspx
---
My favourite RSS reader is [GreatNews](http://www.curiostudio.com/). First of all it is a standalone application, and secondly it is portable, which means I can install it on a USB key or better yet in my [Dropbox](http://www.getdropbox.com/) which is what I do.

Well to be fair it is not portable in the purist way because it caches some stuff on the computers harddrive, but that is only for performance, so functionally [GreatNews](http://www.curiostudio.com/) IS portable.

And GreatNews is very fast, which is just one more reason to love it.

In fact it has only one drawback, the database tends to get corrupt every 2-3 months, and there is no obvious way to repair it. I guess it will be corrected at some point in time (?) but for now it nearly made me give up on this otherwise great product.

After some googling I found that [GreatNews](http://www.curiostudio.com/) uses an sqlite database called newsfeed.db. I couldn't really find a repair tool for it, even though I found a page with lots of [tools for sqlite](http://www.sqlite.org/cvstrac/wiki?p=ManagementTools).

So I had to repair it in a more indirect way.

I downloaded one called [sqliteman](http://sqliteman.com/) and with this I was able to make a new database called newsfeed.db and export all content from the old database and import it to the new one.

One nice sideeffect was that the database was only 1/10 in size after the import. Apparently either [GreatNews](http://www.curiostudio.com/) or Sqllite tends to bloat the database.

After the import I replaced the old database with the new one (you should make a backup of course !), and viola it worked perfectly. So once more I am a happy user of [GreatNews](http://www.curiostudio.com/) :O).

I cannot guarantee that this method will work for you too, perhaps I was just lucky. I advice you to keep your backup of the old newsfeed.db just in case.

P.S: To export the database you just choose "Dump database..." as shown below in a screenshot from [sqliteman](http://sqliteman.com/), which generates an SQL script that can be opened and run in the new database:

![Map network drive](/images/HowToRepairGreatNewsDatabase/newsfeed1.jpg)
