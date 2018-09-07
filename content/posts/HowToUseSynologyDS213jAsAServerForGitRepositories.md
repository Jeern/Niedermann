---
title: "How to use Synology DS213j as a server for Git Repositories"
date: 2014-06-08T02:00:00+02:00
draft: true
aliases:
    - /2014/06/08/HowToUseSynologyDS213jAsAServerForGitRepositories.aspx
---
I must admit that I prefer SVN over GIT due to the (in 2014 only slightly inferior) tooling and due to the illogical naming and structuring of commands and parameters. 
But still GIT has a lot going for it, and I needed the GIT submodule command for a private project of mine  i.e. something that I want to sell 
and therefore not place on github.

So away with SVN.

On my trusted Synology I have the SVN server installed which was basicly just plug and play. So I figured that installing the GIT server package would be just as easy.

Sadly that was not case. Now 6 hours later I finally have it working. So this post is here, to hopefully save you and my future self some time.

This is a rundown of what I should have done.

1. Upgrade to at least DSM 4.3 ( I used 4.2 but the GIT Server package did not work with that. Basicly there was no git commandline interface).
1. Install the GIT Server package from Synology
1. Create a user on the server to access GIT
1. Create a Share for GIT (e.g. named “gitrepo” or similar)
1. If using a windows client. Install putty to issue GIT commands to the server via SSH (you have to enable SSH on the server)
1. Issue commands to make a bare server GIT Repo

	mkdir somerepo.git
	cd somerepo.git
	git init --bare –shared

1. Login to Putty as root (important). Change the owner to be your newly created git User

	chown -R gituser somerepo.git

1. You can now clone the repo from your prefered Git client – using ssh.
btw. When using ssh you should access it via full path. I.e. ssh://gituser@<servername or ip>/volume<x>/gitrepo/somerepo.git

Of these steps (not) upgrading to DSM 4.3 was most costly because nothing anywhere indicated that I could only use GIT server after upgrading.

I also had problems changing ownership of files. Turned out I just needed to login to putty as root. Probably just basic linux knowledge that I do not have.

Same goes for the ssh path. I naturally thought the path would be the same as for https i.e. without the /volume<x> part.

So basicly it works now, although I still have to decide on the security part.

Should it be with username/password or a putty key, should I open up my firewall to allow me access from the internet and so on.