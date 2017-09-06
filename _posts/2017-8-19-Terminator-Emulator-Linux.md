---
layout: post
title: Terminator Emulator for Ubuntu
categories: [Usability, Tools]
tags: [Ubuntu, Terminator]
comments: true
---

Alright! I have been working on linux machine from past 6 years. I earlier worked on `centos` and later on `ubuntu`. 
If you have a strong expertise on commandline, then you are for sure going to use mouse very less or not at all. 

This ensure the best and fast actions with least efforts and decision making when to pick mouse and when to switch back to keyboard.
Hence, your complete focus would be on work and not switching between keyboard and mouse. You might need to master a lot of things on linux like using terminal effectively for almost all operations.
Operations could vary from searching for a string in set of files to updating a file using vi, nano or vim editor.

Terminator is such a tool that makes it very easy to work on terminal. It's interface is very easy to use and understand. With using hotkeys it becomes super quick to switch between terminals and perform desired actions.

Let's understand a very good tool available for linux machines. You could use **yum** for `RPM based` OS like centos  and **apt-get** for Debian based OS.

For RPM based:-

> yum install -y terminator

For Debian Based:-

>sudo apt-get update
 sudo apt-get install terminator

Once installed you could open terminator and start working straight away. All your aliases and other settings will reflect as they do in default terminal.

To Open preference window use right click anywhere on terminator window. See screenshot below for reference:-

![_config.yml]({{ site.baseurl }}/images/2017-8-19-Terminator-Emulator-Linux/preferences.PNG)


To view & modify available hot keys, you could go to 

>Terminator Preferences (Right Click anywhere on terminator window body) > Key Bindings

Below is a screenshot of same.

![_config.yml]({{ site.baseurl }}/images/2017-8-19-Terminator-Emulator-Linux/see-hotkeys.PNG)

You could open multiple tabs by using following key strokes:-

	CTRL + E - Open Tab Horizontally
	CTRL + O - Open Tab Vertically
	CTRL + ALT + X - Maximize / Restore tab to previous size
	ALT + Arrow Key - Select another tab

![_config.yml]({{ site.baseurl }}/images/2017-8-19-Terminator-Emulator-Linux/opening-multiple-terminal-tabs.PNG)


If you want to change background, scrolling options or compatibility, please go to 

>Terminator Preferences  > Profiles

Below is a screenshot of same:-

![_config.yml]({{ site.baseurl }}/images/2017-8-19-Terminator-Emulator-Linux/changingColoorBGColor.PNG)

There could be times when you want to increase or decrease size of any of your terminal tab. You could use 

>CTRL + Arrow key

to increase decrease size.

For example if your tab is on left top then, To increase size horizontally use `CTRL + Right Arrow`. Similary `CTRL + Down Arrow` for increasing size vertically.


Rest should be intutive :)

![_config.yml]({{ site.baseurl }}/images/2017-8-19-Terminator-Emulator-Linux/useCTRL_ARROWkeyToIncreaseDecreaseSize.PNG)

Overall you shouldn't need mouse for these frequent actions especially opening the new tabs and resizing those as and when required.

Hope it helps! Please feel free to share your suggestions if any.

