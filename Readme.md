[![Docker Pulls](https://img.shields.io/docker/pulls/immauss/openvas.svg)](https://hub.docker.com/r/immauss/openvas/)
[![Docker Stars](https://img.shields.io/docker/stars/immauss/openvas.svg?maxAge=2592000)](https://hub.docker.com/r/immauss/openvas/)
[![Docker Stars](https://img.shields.io/docker/image-size/immauss/openvas.svg?maxAge=2592000)](https://hub.docker.com/r/immauss/openvas/)
[![Docker Stars](https://img.shields.io/docker/cloud/build/immauss/openvas.svg?maxAge=2592000)](https://hub.docker.com/r/immauss/openvas/)
[![Docker Stars](https://img.shields.io/docker/cloud/automated/immauss/openvas.svg?maxAge=2592000)](https://hub.docker.com/r/immauss/openvas/)
[![GitHub Issues](https://img.shields.io/github/issues-raw/immauss/openvas.svg)](https://github.com/immauss/openvas/issues)
[![Discord](https://img.shields.io/discord/809911669634498596?label=Discord&logo=discord)](https://discord.gg/DtGpGFf7zV)
[![Twitter Badge](https://badgen.net/badge/icon/twitter?icon=twitter&label)](https://twitter.com/immauss)
![GitHub Repo stars](https://img.shields.io/github/stars/immauss/openvas?style=social)

# A Greenbone Vulnerability Management docker image
### Brought to you by ###
[![Immauss Cybersecurity](https://github.com/immauss/openvas/raw/master/images/ics-hz.png)](https://immauss.com "Immauss Cybersecurity")


# Attention !!! #
# Dec 1, 2021: 21.4.4-06 is now live on hub.docker.com and 'should' resolve all the issues. See below for more detail. #

Thanks,
Scott


# Tags  #
tag              | Description
----------------|-------------------------------------------------------------------
mc | This is the first BETA of the multi container build.
21.4.4-06 | This is the most recent and latest.  
armv7     | The latest build for ArmV7 with Postgresql11 based on Debian buster
20.08.04.6 | The last 20.08 image
beta            | from the latest master source from greenbone. This may or may not work.
pre-20.08   | This is the last image from before the 20.08 update. 
v1.0             | old out of date image for posterity. (Dont` use this one. . . . ever)


- - - -
## Documentation ##
The current docs are maintained on github [here](https://immauss.github.io/openvas/)
- - - - 

### 13 December 2021 ###

- OK ... it's working. Everything in its own container. Well ... almost. Postfix is running with gvmd. I haven't found away around that yet. If you want to try this out, just pull the git repo, or make a copy of the docker-compose.yml that is in the 'mulit-container' folder. From the same directory as the docker-compose.yml, run: ``` docker-compose up -d ``` 
- If you try this out, PLEASE let me know. 
- The same image will also run as the original always has. 
- Make sure you take a look at the docker-compose.yml first. The current default uses SKIPSYNC=true. (Because I was impatient during testing.) 

-Scott

- - - - 

### 3 December 2021 ###
- After some testing with arm64 image on RPI, I'm making 21.4.4-06 latest. Please let me know if you have any issues. 

-Scott

- - - -
### 1 December 2021 ###
# Finally !! #
- 21.4.4-06 is live!! This resolves a ton of problems and was a total PITA. Greenbone changed the default location of the openvas logging config to /etc/openvas. But the install.md did not get updated. So I was still dropping a config in /usr/local/etc/openvas. This shouldn't have been a big deal, but there was another bug in openvas/gvm-libs that caused openvas to segmentation fault if there is no /etc/openvas/openvas_log.conf.  UGH! Anyway, you should no longer get the warnings about having an old scanner with this one, postfix should work again, and a fully up to date GVM suite. 
- Next on the list is to finish the multi-container setup and more automation for testing. 

-Scott

- - - -
### 28 October 2021 ###
- It has been a while. Greenbone dropped a new a release a week or so ago, and it has given me quite the hard time. There was a change ... somehwere ... that causes the openvas scanner to segmentation fault, but only when run in a container. The stable branch does not exhibit this behavior, but the current release does. So .... there is good chance it will get fixd in the next release. In the mean time, I've upgrade everything except openvas scanner to the latest release. In the course of trying to resolve the segfault, I got the notion that there might be a directory missing or directory persmission problem. So I finally took the time to move ALL of the directory structure setup and linking for the volumes into the build process and took it out of the start.sh. (Take a look at the stage2-setup.sh in the scripts folder.) This greatly simplified start.sh and will come in handy as I move to the multiple containers setup. You know, how containers are suppose to be run, one process per container. 
- So after all that ... new release. For the future, I'm going to fix my versions on the version of gvmd in the build. For this one, that is 21.4.4. And I'll add a "-xx" for my versioning. So today's release will be 21.4.4-01
- Sorry ... no update to Arm v7 yet.  If someone really needs it, please reach out. 

-Scott

- - - -

### 26 August 2021 ###
- Well that was fun. Thanks to [@bricks](https://community.greenbone.net/u/bricks/summary), [@Lukas](https://community.greenbone.net/u/Lukas/summary), [@cybermcm](https://github.com/cybermcm), [@dkade](https://github.com/dkade), [@tedher](https://github.com/tedher), [@grisno](https://github.com/grisno), & [@orhpeus](https://github.com/orpheus) for the discussion and help resolving the socket issues. (Details are [here](https://community.greenbone.net/t/sockets-sockets-sockets/10035) ) In the end, it turns out I missed a permission change on a directory in the move to buster. 
- So now .... the latest, 21.4.3, and armv7 images are all up-to-date and working properly.  (Don't look at start.sh with too critical an eye right now though. It needs some serious clean up. )
- Now that the newbuild is actually working, its also time to merge the 'newbuild' branch into master and make buster the default. I've been back and forth on them for a while, so I expect this to be a moderately painful expereince ... 
- After that wil be getting the automation back for the weekly DB builds and code housekeeping. 
- - - - 
### 19 August 2021 ###
- Wow. Vacation can take a lot out of you. :) It took a while to catch up, but things are back on track. In addition to moving everything to buster, reworking the Dockerfile for more efficent build/rebuilds and creating a new tag with armv7/armh  support, Greenbone released new versions of everything last week. So here we are. **21.4.3** This is now the latest tag and is the gvmd version running in the latest and armv7 tags.  
- The weekly rebuilds are running a little behind. I still need to workout the automation for the weekly rebuilds, but now with two images since the DB is not compatabile between the latest (with arm64 & amd64) and the armv7 tags. 
- - - -
### 19 July 2021 ###
- Buster. When i started this journey, I only wanted to add a few features I thought were missing to an already existing container. I guess I let it get a little out of hand. One of things that bothered me about the orignal, that I didn't have the time deal with, was that it was running on a base container image using Ubuntu. Nothing against Ubuntu, but I know the Greenbone Devs build on Debian Buster. With all the dependancy issues I was having trying to get the ArmV7 image to build, I decided it needed to be done. So I finally took the time to rebuild the whole thing using Debian Buster. The idea is that this should reduce the number of problems I have with dependencies and be more in line with Greenbone.
Just one glitch:

   Postgresql 12

   When I moved to 20.08, I went to the current Postgressql with Ubuntu, which was 12.  But Greenbone is still standardizing on 11. Now I could build the image on 11, but then anyone with an existing DB (myself included would have to go throught the pain of downgrading the DB. This doesn't work well with the idea of having an easy to use slim container. So, I now have an image, based on Buster, and using Postgresql 12. 
   
   The Bad news. Postgresql12 is not available on ArmV7. But since I've never had a working ArmV7 image, there shoudln't be anyone with a Postgresql12 DB. So ... there will be an ArmV7 image, but it will be seperate and live with it's own tag in GitHub and Docker Hub, while the AMD64 & Arm64 images will be a multi-arch image as the latest. I know I can build ArmV7 on Buster, but only with Postgresql11. 
   
   ## Status: ## 
   - The multi-arch image is almost ready. (Expected by 22 July)
   - The ArmV7 will need a re-write of the start scripts to support Postgresql11. (Expected 29 July)
           
     
- - - -
### 12 July 2021 ###
- armv7 ..... is failing to build gvmLibs because of dependancies. At the moment, the latest tag only has amd64 & arm64

- Scott

- - - -
### 28 June 2021 ###
- armv7 is now live with the latest tag. 
- I've also just noticed that Greenbone has release 21.4.1 !! I'm building now and will try to test and have it up in the next day or so. 


-Scott

- - - - 
### 21 June 2021 ###
- Finally!!! The latest image on Docker Hub is now a multi-arch image. amd64 & arm64. These should run on any 64 bit x86 kernel and on the arm 64 bit kernels. (aarc64 & arm64). I also resolved a bug in weekly rebuild script that was having the images built with an old DB. If you expereinced a rather slow to ready container recently, that should be resolved. I've also moved the rebuilds off to my own hardware vs using the Docker Hub build system. This allows me to do the multi-arch builds with buildx.  There was also a minor update from Greenbone. 

-Scott

- - - -
### 14 June 2021 ###
- A new approach. Now that Docker is going to be charging for using thier build environment .... I decided to go a step further. I'm going to be doing the builds on my own infrastructure, so why not go for a multi-arch build. Something I don't think docker hub offers anyway. So the first (of hopefully many) multi-arch images is now on docker hub. The tag is simple "multi". I can't stress enough that this is very BETA at the moment. I've not done any serious testing with it. Please let me know if you find any isues on any platform. And maybe drop me a note if you have succes too. ( here, a twitter @immauss ... ) 

-Scott

- - - - 
### 12 May 2021 ###
- Looking for feedback on the ARM images. I have not yet attempted the 21.04 on ARM. If you are interested, please open an issue and let me know. 
- - - -
### 10 May 2021 ###
- 21.04 is now the latest! The new image will upgrade your DB, so no worries on using an older DB. Bonus, 21.04 is WAY faster with postgres 12. I`ve also added all the environement options to the docker_composer.yaml in the git repo, so if composer is your thing, start there. No other new functionality added to the image, but lots of changes from Greenbone.  If you have any problems, please open an issue. 
- - - -
### 4 May 2021 ###
***May the 4th be with you***
- So the 21.04 is taking a little longer than anticipated. What would be great, would be a few more testers. If you have the cycles, I would really appreciate any testing. Just make sure you are not working on your production data yet!

Thanks,
-Scott

- - - -
### 27 April 2021 ###
***New GVM version !!***
- OK ... I's here!! 21.04. It has some tweaks and fixes. But the biggest change that I see is that it seems to be performing much better with postgresql 12. This is good news. I have it working but I have NOT made it the latest yet. The current tag is 21.04-beta. It is currently not tested with upgrading a current DB. I've only tested with a fresh start. If you choose to test, please drop me not and PLEASE open an issue if you find a problem with it. 

-Scott

- - - -
### 28 Mar 2021 ###
***gmp option added***
- At the request of [hoboristi](https://github.com/hoboristi) , I`ve added an option to enable the gmp service. It will require two options though.
  - First, enable the service in the container with ``` -e GMP=<service port number> ```
  - Then publish the port from the container ``` -p <external port number>:<service port number> ``` 
  - Example:
```
docker run -d -p 9392:9392 -p 9390:9390 -e GMP=9390 --name openvas -v openvas:/data immauss/openvas:latest 
```

-Scott 

### 27 Mar 2021 ###
***Doh!***
- Appologies to anyone who tried to pull the latest image in the last 24 hours. It looks like I accidently pusshed my dev branch to master and Docker Hub diligently built a new image.  ....... This didn't work. I've reveresed the changes and the "latest" tag is now good. Thanks to [cybermcm](https://github.com/cybermcm) for catching the problem and opening an issue. 
- For the curious, the new work is on finding a clean way to downgrade the DB from postgresql 12 to 11. Apprently there are some performance issues with gmvd and postgresql 12. 

-Scott

- - - -

### 16 Mar 2021 ###
**Tag 20.08.04.6**
- Added the HTTPS environment variable. Setting this to true will cause gsa to start with https enabled. I`m working on a better implementation of this, but as this was requested, I went ahead and added it. The `better` implementation will be able to use letsencrypt certificates! 

-Scott

### 23 Feb 2021 ###
**Tag 20.08.04.5**
- Fixed the PASSWORD and USERNAME env vars. Make sure you check checks the docs for the caveats.
- Wrote a new script to make sure I`m getting the latest releaes from all of the Greenbone github repos.

-Scott

- - - -
### 18 Feb 2021 ###
## A few minor updates and one **BIG** change ##
- The restore logs now go to /usr/local/var/log/db-restore.log instead of the terminal
- The start.sh is modifying the feed sync scripts (greenbone-nvt-sync and greenbone-feed-sync) to make the normal output a little quieter.
- And the **BIG** announcement:
# Automated weekly builds with content updates!! #
Early every Monday morning, the image will be rebuilt with the latest feed updates. This means that the latest image will always have  feed data less than 1 week old!!!

-Scott
- - - -
### 16 Feb 2021 ###
I have added some additional functionality to the image:
- Container now does a proper shutdown of postgresql on container stop. (I believe not having this has been the cause of some DB corruption seen in the past.)
- New "RESTORE" option added to restore from a DB backup. [See the Docs](https://github.com/immauss/openvas/tree/master/docs#database-backup)
- Updated  [documentation](https://github.com/immauss/openvas/tree/master/docs)
- New latest tag is now 20.08.04.4.
- There is also a 'beta' tag now. Use this at your own peril as it may or may not work. (probably it will not.)

-Scott

- - - - 



### 14 Feb 2021 ###
# Short update
After pushing 20.08.04.1, I realized I had not merged the base db changes. So 20.08.04.2 includes the changes to support the base DB and contains a DB from today. 

-Scott

- - - - 



### 13 Feb 2021 ###
# Greenbone release 20.08.1 about a week ago. 
### I found out yesterday, and today ... I'm releasing tag: 20.08.04.1. 
- This has the fix in gvmd to make the processing of the feeds more resilient. If you are getting a ton of errors for an NVT that is not in the family, this image will fix it. The problem is actually in the feed,  but the latest gvmd does not get stuck on feed issues. 

- There is also a new beta tag, but as you might expect, this is not really working as it is pulling from the master branch of all the tools. The backend seems to work, but the gsa is just not getting it. This is mainly to help me be ready for the next version by keeping me alert on any new dependencies that may come with the next version. Use it at your own peril. 

-Scott

- - - - 


### 3 Feb 2021 ###
# Big News !

The latest image, tag 20.08.04 includes a baseline database and feed sync. No more waiting for the feeds to sync and then waiting for gvmd to build the database. This means you can login and start running scans about a minute after running the container!

The downside is the USER and PASSWORD environment variables no longer work as they default (admin:admin) is part of the baseline database. I think I can work around this, but that will have to wait for 20.08.05.

There is also a new environment variable: SKIPSYNC . This does exactly what it says, it bypasses the feed sync on container start to speed you along. 

-Scott

- - - - 

### Jan 2021 ###
# 20.08 
# NOTE: DO NOT USE THIS WITH YOUR OLD PRE-20.08 database. 

## New with the 20.08 images. ##
- Added an environment var for quietening the feed syncs.
  - QUIET="true" will send the output of the sync scripts on startup to /dev/null
- Added an environment var for increasing redis DBs. 
  - REDISDBS="<number of DBs>"   default is 512.
- This is only what I've added. There are tons of other changes with 20.08 itself.
- New multistage build makes for a MUCH smaller image. Down by more than 1 Gig. Same functionality! 
- - - - 





For License info, see the [GNU Affero](https://github.com/immauss/openvas/blob/master/LICENSE) license.

Thu Feb 25 23:59:13 UTC 2021

