---
layout: post
title: "Get some vim all up in your arduino development"
date: 2012-11-04 14:54
comments: true
categories: [arduino, vim, make]
---
I love me some physical computing and have done a fair bit of tinkering with [Arduinos](http://arduino.cc) over the years. Up until now, I've always done my tinkering using the Arduino IDE. The Arduino IDE makes compiling and uploading to my boards all pretty simple, and I've generally writing fairly simple sketches, so my needs haven't been great.

While it does compilation and uploading really well, it is much less useful as a text editor (I'd go so far as to say it kinda sucks compared to a powerhouse like vim). When I started my current project, I really wanted to find a better way (ie, do everything from vim).

<!-- more -->

After some searching, I found a few vim plugins, but all were locked to a particular version of the Arduino software. I did end up finding a decent [syntax file](https://github.com/vim-scripts/Arduino-syntax-file) which seems quite effective.

I eventually discovered a [makefile](http://mjo.tc/atelier/2009/02/arduino-cli.html) that can build and upload, and works with the latest Aduino software and boards. I ended up having to pull in a few commits from some forks of the project to get it working with my particular setup (most of the issues came from using hardcoded paths). My fork is available [here](https://github.com/akjones/Arduino-Makefile) in case it is of any use to you.

This makefile requires some additional libraries to be installed, on Ubuntu this'll look something like:

    sudo apt-get arduino-core avrdude libconfig-yaml-perl libftdi1 libyaml-libyaml-perl libyaml-perl libdevice-serialport-perl

Finally, you'll need to set some environment variables:

* `ARDUINO_DIR` points to where you've installed the Arduino software (I reccomend grabbing the latest from the [website](http://arduino.cc/en/Main/Software)), and
* `ARDMK_DIR` should point to where you put the master makefile.

The original author of the makefile also recommends setting `AVR_TOOLS_DIR` to point to `/usr` on linux, though my setup seems to work ok without it.

Once you've got all that set up, you can write a short makefile for each of your arduino projects. You'll only require a few lines:

    BOARD_TAG=proMicro16MHz
    ARDUINO_PORT=/dev/ttyACM1

    include /home/andrew/projects/arduino/Arduino-Makefile/arduino-mk/Arduino.mk

You can also override any of the environment variables in each makefile if you need to. In my current project I need to override `ARDUINO_DIR`, since I'm using a Sparkfun board, which requires a slightly customised Arduino hardware directory layout (I'll probably document this in a later post).

Hopefully now you'll be able to `make && make upload` your way to Arduino development without the Arduino IDE. For extra awesomeness, I've got some vim keybindings hooked up to `make clean`, `make` and `make upload`, which you'll be able to duplicate in any decent text editor or IDE.

_Happy coding!_
