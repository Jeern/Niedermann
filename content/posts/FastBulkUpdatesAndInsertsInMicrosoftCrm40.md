---
title: "Fast bulk updates and inserts in Microsoft CRM 4.0
"
date: 2010-03-07T01:00:00+01:00
draft: false
aliases:
    - /2010/03/07/FastBulkUpdatesAndInsertsInMicrosoftCRM40.aspx
---
In Microsoft CRM you have no choice when doing updates to the database. You have to use the CRM Web Service.

Microsoft does not support direct SQL updates of the underlying SQL Server tables, because of the behind the scenes CRM magic.

The Web Service is OK for small amounts of data. But recently I had a case where I had to code an import of a spreadsheet with up to 100.000 rows into CRM.

This would result in 600.000 web service calls, since there was 5 retrieves for each updates. The first tests showed that each web service call would be about 1,5 seconds even though they were done internally on the CRM server a fast quad-core 64 bit machine. This would be 900.000 seconds or 250 hours, more than 10 consecutive days.

Too slow for our particular business need.

Fortunately [UnsafeAuthenticatedConnectionSharing](http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.unsafeauthenticatedconnectionsharing(VS.71).aspx) came to the rescue.


```csharp
proxy.OnGetWebRequest += (sender, e) =>  
{  
    e.WebRequest.ConnectionGroupName = InitUserId.ToString();  
    e.WebRequest.UnsafeAuthenticatedConnectionSharing = true;  
};  
```

On the CrmService proxy you set the [UnsafeAuthenticatedConnectionSharing](http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.unsafeauthenticatedconnectionsharing(VS.71).aspx) to true. This just means that other calls to the web service will use the same connection to the database, and without reauthenticating. Which means that the next call with [UnsafeAuthenticatedConnectionSharing](http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.unsafeauthenticatedconnectionsharing(VS.71).aspx) set to true, will use the same connection. And will be assumed to be the same user. [ConnectionGroupName](http://msdn.microsoft.com/en-us/library/system.net.webrequest.connectiongroupname.aspx) is a convenient way to split up the connections so that we don’t accidentally reuse connections between different users. This can be done simply by using the guid of the current CRM User.

And now the important thing. This makes the web service call take 3/100 seconds. About 50 times faster ! The 600.000 calls can be done in 5 hours. Of course this speed was only attained because all calls where done internally on the CRM Server.

So why are these two settings not the default ? I don’t know. Perhaps because if you have a site with millions of users the amount of memory used up by each connection will be too big. But on a typical CRM site I would guess there would not be enough users to make this a problem.

But to be safe, if you just use the 2 properties in the areas that need performance with little concurrency between users you will be OK.

This and other performance best practices for CRM can be seen on [technet](http://technet.microsoft.com/en-us/library/bb955073.aspx). But I don’t think any will outperform this one.