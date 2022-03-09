---
title: Creating Vultr Instance
date: 2022-03-08T14:59:36.000Z
draft: false
description: Creating_Instance
---

Matching blog post to [Build Opekkt Tech](/docs/vps_migration/creating_vultr_instance) in [Docs](/docs/)

### Creating instance at Vultr & setting up DNS

I initially created a "High Frequency" instance running FreeBSD 13 with 32GB NVMe, 1 vCores, 1G ram and 1GB bandwidth For testing. But my testing quickly became live and so I upgraded to 2 vCores, 80GB NVMe, 2GB RAM and 3 GB bandwidth.

![](https://titania.rkey.tech/img/titania.webp)

Before the upgrade, I migrated my test only domain bootstraps.tech over from Digital Ocean to Vultr. My domains are over at HostGator, so moving the domains simply meant that I created DNS entries for boostraps.tech, rkey.tech and r0bwk3y.com at Vultr and then point HostGator from Digital Ocean to Vultr. I use ProtonMail for my mail servers so I have to plug that into those DNS entries as well. One thing I see missing on many web sites is CAA entries. I always add CAA entries to my DNS configuration.

#### I use a Caddy server that auto-magically takes care of my SSL certs for me. I like lazy solutions that work.

When Vultr sets up initial DNS entry you select the instance to point it to and then Vultr creates a wild card '*' entry. I initially did not like this solution because,,,,,,,,Well I don't know why, but I'm finding I like the solutions as it only requires I create a Caddy entry in my web server and then I'm done. Again the lazy solution thing.

#### Before changing the DNS pointers I need to migrate my Caddy server at Digital Ocean (DO) over to Vultr.

At DO I had a mixed setup I had a Caddy server that ran on the host, not in a jail breaking all my own rules. Then I had another server running only jails, which consisted of a WIKI and a couple of Ghost blogs. Everything else was just files in a directory on the host running the Caddy server. So my initial migration was just from an instance on Digital Ocean to Vultr running at the host level. Since I deploy static web sites via rsync from my **_Archlabs_** workstation at home I simply added the new host to my script and deployed to both instances for rkey.tech and r0bwk3y.com

Of course prior to rsyncing things over and with any new VPS I generally do two things first.

1. I turn off password login in ssh. Since I keep my public ID at DO and Vultr and Linode for that matter. when I create a VPS the first thing I do is add a new user, then I rsync my keys over from root that the VPS provider added when I created the instance to my user account.

  **_Note_** in the document section there is no slash on `.ssh` but there is at the end of user/ The is not necessary but a habit of mine. When syncing directories if you leave off the trailing `/` then the whole directory gets synced. Since my user does not have an `.ssh` directory yet I leave off the slash. The `/` at the end of `user/` is more about habits in this particular case. It does not need to be there. But when you have backed up terabytes of data to a server and forgot the slash on either end, you tend to be fussy about such things.

2. I make sure the new user can su to root. Linux is a mess sometimes adding to `wheel` works and sometimes there not even a `wheel` group so you have to add to `sudoers` and then modify the sudoers file via `visudo` generally Linux has some lame editor like Nano and I screw everything up using `vi` keys. So if I'm only doing things once I usually just `vi` the file. On BSD I just make sure the user belongs to wheel when creating the account and then I usually install `doas`, and configure `/usr/local/etc/doas.conf` honestly just for muscle memory I usually install `opendoas` on my Linux boxes as well. My `doas.conf` file is super freaking simple.

3. After verifying that my user can ssh in. I turn off password authentication and root user login

  Most VPS providers already have `PasswordAuthentication no` unless you do not have your keys on site or do not select to add during creation. Of course until you create a user and verify you can login you will most certainly find that `PermitRootLogin` is `yes` since the default is now no. You could just comment it out but I like the feel of an explicit statement in a config that affects security. It's a mental thing for me.

My Caddy configuration on the host is just running a file server for now so the entries are very basic. Because I can not for the life of me figure out why the "Congo" theme does not follow basic Hugo rules for inserting images into markdown documents. I have a catch all entry for two things. One is a fake 404, which is just a redirect for funsies and gamesies and the other is a place to put my images in documents, which is actually a good thing now that I think of it because it saves drive space when I re-use images. Funny how problems become solutions.

For images there is a img directory under wtf that I just point my pages to. So instead of `![](titania.webp)` which should work but does not; I have `![](https://titania.rkey.tech/img/titania.webp)` If anyone knows why having the image in a Hugo bundle block does not work on Congo, please feel free to let me know. I'm sure it's something stupid simple.

My other web sites are just directories named after the web site so `rkey.tech` for this site and `r0bwk3y.com` for my personal site. So far it's all just child's play and pretty boring stuff even for the process of migrating VPS providers and sites over. I even made sure my deploy scripts deployed to DO and Vultr so when the DNS hit with the new site everything was in sync and up to date. For this migration process. I did not even have to shut down my monitors.

Later on when I migrated NextCloud from a home server to the cloud and even migrated my Caddy to a jail, things did not feel as much like child's play. Nor did I do so without taking hits on my up-time monitor.

#### Yes, Yes, I know the status page says 100% as of today 8-March-22. But it was a migration and I should have paused the monitors anyway. So I feel justified in resetting the counters :)
