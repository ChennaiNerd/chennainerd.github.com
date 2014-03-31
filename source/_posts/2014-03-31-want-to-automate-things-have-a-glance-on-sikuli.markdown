---
layout: post
title: "Want to automate things? Have a glance on Sikuli"
date: 2014-03-31 15:14:47 +0530
comments: true
categories:
---

You want to automate some repetitive tasks in daily usage of applications or
web pages, games or IT systems and networks etc., and you do not
have adequate tools in hand.

Then you are at the right place now. Just give a try for Sikuli, a simple tool for GUI automation.
Sikuli can automate any computer operations based on screen shots.

## Sikuli Installation steps

#### Step 1: Download the following from https://launchpad.net/sikuli/+download

    Sikuli-1.0.1-Supplemental-LinuxVisionProxy.zip (md5)
    sikuli-setup.jar (md5)

#### Step 2: Install dependencies

Ubuntu users: Install the below packages (Dependencies for Sikuli)

    sudo apt-get install openjdk-7-jdk
    sudo apt-get install libopencv-dev
    sudo apt-get install libtesseract-dev

Unzip the .zip file and follow the readme steps to build a new `libvisionproxy.so` file.

Windows users: Install java jre 7

#### Step 3: Install Sikuli using the jar file.

Run the `Sikuli-setup.jar` by setting the executable bit if not set
on it and use the command:

    java -jar sikuli-setup.jar

Ubuntu users should replace the newly generated `libvisionproxy.so` with the
existing one in `<Sikuli_installed_folder>/libs/` location.

#### Step 4: Launch the Sikuli IDE using the command.

    cd <Sikuli_installed_folder>
    runIDE.cmd (for windows users)
    ./runIDE (for Ubuntu users)

#### Step 5: Run the Sikuli tests from command line using the command:

    runIDE.cmd –r <sikuli_test_name.sikuli>    (for windows users)
    ./runIDE –r <sikuli_test_name.sikuli>      (for Ubuntu users)

### Sample Sikuli code snippet:

Say you want to automate the following: launch a notepad, type some text and save it.

Below is the sample code written in Sikuli IDE.

![Sikuli](http://i.imgur.com/qtvxGOA)

The same code snippet trying to open a diffent app say gedit, should work seamlessly in Ubuntu.
For web browser based automation, wait for my next blog on Selenium.

Happy automation folks !!!
