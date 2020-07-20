---
title: "QuickVault released"
date: 2020-07-18T01:00:00+01:00
draft: false
---

I just released 3 small libraries for configuring secrets in a .NET Core or Full framework app.

Its a simple key/value store with encrypted values. And it supports .NET Configuration (both Core and Full Framework)

Its called [QuickVault](https://github.com/Jeern/QuickVault) and consists of a .NET standard base library installed with:

```
Install-Package QuickVault
```

A library for .NET core configuration:

```
Install-Package QuickVault.Configuration.Core
```

And a library for .NET full framework configuration:

```
Install-Package QuickVault.Configuration
```

# What does it solve

You know the problem. You use a cheap web hosting company where you only have limited access to the server, and you do not want your precious secrets, password, api keys etc. to be openly accesible in the web.config or appsettings.json You could use something like Azure Key vault but if you had access to that you would probably use Azure for the website as well.

QuickVault solves the problem by giving you a key/value store in a file: QuickVault.bin. All keys are readable, but all values are encrypted with the RSACryptoServiceProvider.

The values are encrypted with the public key (stored in QuickVault.pub) and decrypted with the private key (stored in QuickVault.priv)

This means you need to protect your private key, for example by not pushing it to git. Make sure to add QuickVault.priv to your .gitignore file.

Microsoft recommends you never store your private key in a file, which makes sense but the purpose of QuickVault is not to provide military grade security but rather to make cheap good enough security for most cases. So its definitely not meant for big enterprise systems.

If you ever suspect your private key file has been lost, its easy to generate new keys and reencrypt your secrets (of course you should also change all the secrets in that case) !