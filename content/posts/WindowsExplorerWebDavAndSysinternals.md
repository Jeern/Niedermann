---
title: "Windows Explorer, WebDav and Sysinternals"
date: 2009-03-13T01:00:00+01:00
draft: true
aliases:
    - /2009/03/13/WindowsExplorerWebDavAndSysinternals.aspx
---
Any windows user is familiar with Windows Explorer but were you aware that one can access an ftp site or WebDAV enabled site using the Windows Explorer ? I myself haven't really been looking in to this, since I have been using Filezilla to upload files to my website, but why not use Windows Explorer ? It is always at hand with the Windows+E keystroke.

As an example I will show you have to access the http://live.sysinternals.com site via the Windows Explorer since it is a WebDAV enabled site with anonymous access. The site contains all the famous tools from Sysinternals a company which was purchased by Microsoft.

In order to connect you just have to click on the good old "Map Network Drive ..."

![Map network drive](/images/WindowsExplorerWebDavAndSysinternals/Temp1.jpg)

Since I know the name of the subdirectory 'tools', the easiest way seems to be to map to a drive letter directly by typing the name \\\\live.sysinternals.com\\tools in the folder textbox:

![Map network drive](/images/WindowsExplorerWebDavAndSysinternals/Temp2.jpg)

If you are trying to connect to an ftp server the right way seems to be to click on "sign up for online storage..." After pressing "Next" twice you get this screen:

![Map network drive](/images/WindowsExplorerWebDavAndSysinternals/Temp3.jpg)

Here you can write the name of the ftp server or website if using WebDAV, so here I could have written 'http://live.sysinternals.com'. The next screen makes you enter username/password or Anonymous access. Pretty self explanatory.

The end result is that the site appears under 'My Network Places.'

After adding the sysinternals site to your explorer you get all the usual advantages, easy copying and pasting etc.

BTW there are still reasons for using a "proper" ftp client like Filezilla, e.g. you can specify an alternative port number, which doesn't seem to be an option in the Windows Explorer.
