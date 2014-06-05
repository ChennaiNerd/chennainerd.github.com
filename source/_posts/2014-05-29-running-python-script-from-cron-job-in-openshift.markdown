---
layout: post
title: "Running Python script from Cron job in Openshift"
date: 2014-05-29 11:47:37 +0530
comments: true
author: Fizer Khan
categories: ["python", "cron", "openshift"]
---

`Openshift` is one of the amazing PAAS service where you can deploy
your application in a very simple steps. It also provides Free gear for developer.
So you can deploy your hack without providing credit card information for zero dollar.

Our application stack is Python Flask, MongoDB, Angular.js and some CRON scripts.
We wanted to deploy our application in some PAAS which has CRON support.

First We choose `Google App Engine` since it supports CRON jobs.
Unfortunately Google App Engine does not allow us to connect external MongoDB
database like MongoHQ and MongoLab. Only solution is to create MongoDB instance
in `Google Compute Engine`. It is not feasible for us.

We also play with `Heroku` which does not work well for us.
Finally we move to `Openshift` to deploy our application. Following are the
steps to configure cron scripts in `Openshift`.

* Add CRON cartridge in your application

```
rhc cartridge add cron-1.4 -a application_name
```

`rhc` is a command line tool to control the `Openshift` application.

* Place your cron scripts to your application's `.openshift/cron/{minutely,hourly,daily,weekly,monthly}/` folder. Here is the sample python scripts.

```
#!/bin/bash

echo "************ Cronny Started ***************"
date >> ${OPENSHIFT_DATA_DIR}/ticktock-start.log

source ${OPENSHIFT_HOMEDIR}/python/virtenv/bin/activate
python ${OPENSHIFT_REPO_DIR}/wsgi/crawler.py

echo "************ Cronny Executed ***************"
date >> ${OPENSHIFT_DATA_DIR}/ticktock-end.log
```

And that's all there is to it! Have a nice day.


