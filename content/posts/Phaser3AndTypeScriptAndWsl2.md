---
title: "Using VS Code to develop Phaser 3 games in TypeScript on WSL2"
date: 2021-03-14T01:00:00+01:00
draft: false
---
 
Phaser 3 is a great javascript game engine for developing browser games. As far as I understand the new versions 
follow the general trend and is generally developed in typescript.

However it does not automatically follow that its easy for game developers to setup their own 
environments to develop their Phaser games in typescript. For historical reasons (I guess) there is not a lot of 
guidance to be found online.

Since I am a fluent C# "speaker" but VERY rusty in Javascript it seems natural for me to use typescript a language 
closer to C#.

In this post I look into how to get things started for developing a Phaser 3 game in Typescript.

The best help I found online is still this 
[phaser 3 typescript tutorial](https://spin.atomicobject.com/2019/07/13/phaser-3-typescript-tutorial/)

Very thourough and helpful guide with a Github repo. Awesome.

Unfortunately time goes by and things changes + the tutorial seems to be linux centric and I prefer to develop on Windows.

Certain yarn commands did not work as expected when running on windows so instead I tried to go for 
installing them in WSL2 (using Ubuntu on my machine)

That being said after a lot of pain getting everything to run on WSL2 I tried the tutorial again on windows and 
now it works there too. So perphaps it had to do with my specific setup and something unknown to me is shared 
between the two environments.

Still the yarn setup commands used in the original post are not verbatum applicable anymore because some of the packages 
do not work together the same way as they used to.

## Using VS code

But first things first. 

I set up VS Code (in Windows) to access project folders on my WSL2 Ubuntu instance. For this I installed the 
[Remove WSL extension](https://code.visualstudio.com/docs/remote/wsl-tutorial) and then I open the folder on the 
Ubuntu instance using `Command Palette ->  Remote-WSL: Open folder in WSL...`

This gives access to editing the files in the folder from VS Code running on Windows. 

The Ubuntu instance changes IP address everytime the computer is rebooted but it works seemlessly in VS Code.
BTW: When troubleshooting you can see the current IP address on the Ubuntu instance like this:

```shell
grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'
```

And confirm that you can ping it from windows.


## Installing TypeScript, Phaser etc.

Then I go to the folder in the Ubuntu instance (can also be accessed from the terminal in VS Code). 
And do the the setup as describer in the [phaser 3 typescript tutorial](https://spin.atomicobject.com/2019/07/13/phaser-3-typescript-tutorial/)


One of the things that do not work anymore is using the newest webpack packages in this setup

There were two problems with the new version

1. The Webpack dev server is no longer started with `webpack-dev-server` but `webpack serve`
2. Despite changing the setup to use `webpack serve` it was not enough. For some unknown reason
an error pops up:

```
Error: Conflict: Multiple chunks emit assets to the same filename app.bundle.js (chunks app and vendors).
```

Instead I go back to these specific versions

```shell
yarn add --dev copy-webpack-plugin@6.2.1  webpack@4.42.1 webpack-cli@3.3.11 webpack-dev-server@3.10.3
```

But for now still use the newest typescript and phaser

```shell
yarn add --dev ts-loader typescript
yarn add phaser
```

The rest of the guide had only very few problems like a missing Assets folder that I could just create.


## Accessing the dev web server on Ubuntu

It was problem free to start the development web server on ubuntu using `yarn dev` in the project folder.

And it even opened the browser in windows on `http://localhost:8080` all seems good. But unfortunately...
Connection Refused :sad:.

I spent several hours trying to fix this but finally it worked... I thought. The problem is that Windows need to 
know that Ubuntu is to be accessed on port 8080. Some kind of port forwarding is in order one should think.

But that must be set up already, because what finally seemed to work was this:

```powershell
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound  -InterfaceAlias "vEthernet (WSL)"  -Action Allow
```

A new firewall rule that allows all inbound connections on "vEthernet (WSL)". 

Unfortunately when I woke up the next day the problem had returned. Instead I have tried this:

```powershell
Set-NetFirewallProfile -DisabledInterfaceAliases "vEthernet (WSL)"
```

Disabling the network interface altogether. Now it seems to work (on my machine) even after 3 restarts. 
I feel somewhat confident.

The setup on your machine might be very different, I use Eset for firewall but has it set to respect the windows firewall rules.
You can experiment with temporarily disabling your firewall to get it working.

## How to continue

Now that I things work I will probably continue using WSL2 and VS Code to develop the next game. However there is also the option of
going all windows again or perhaps use a devcontainer in combination with VS Code. Will definitley try it out to see if it works for me.
I should think that would give me the linux capabilities in docker without the hassle of poking a hole in the firewall to WSL2. 
Port forwarding in docker is after all trivial.

One other thing I will probably do for development is to try to get the newest web pack version to work. 
Using a different development server than the one in webpack is an idea I could try. http-server comes to mind.
