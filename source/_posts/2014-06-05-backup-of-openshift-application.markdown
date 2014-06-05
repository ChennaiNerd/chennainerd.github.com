---
layout: post
title: "Backup of Openshift Application"
date: 2014-06-05 21:43:40 +0530
comments: true
author: Fizer Khan
categories: openshift
---

You may want to schedule backups of your `openshift` application daily, weekly, or monthly.
It can be done in two simple steps

1. Create backup application

First we need to spin up a backup application

    rhc app create osbs https://raw.githubusercontent.com/wshearn/openshift-cartridge-osbs/master/metadata/manifest.yml http://tinyurl.com/OpenShiftRedisCart cron --no-git

Once it is created, it will give your username and password. Please make a note of it.

2. Create a backup cartridge

Then you have to add backup cartridge to the application for which you want to take backup.

    rhc cartridge add -a <application to backup> -c https://raw.githubusercontent.com/wshearn/openshift-cartridge-osbs-client/master/metadata/manifest.yml

Once it is done, you can login with username and password(that you got in previous step) in the following URL and schedule the backups.

     http://osbs-<your_namespace>.rhcloud.com

Hope it helps. Have a nice day.