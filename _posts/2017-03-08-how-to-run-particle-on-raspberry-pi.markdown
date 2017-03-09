---
layout: post
title: "How to run Particle on Raspberry Pi"
date: "2017-03-08 14:40:05 -0500"
---




>**Use the most popular single-board computer with the most popular IoT tools.**


<center>
<img src="/images/Particle-Pi.png">
</center>

## Introduction

In October of 2016 Particle announced that they would add support for Raspberry Pi on their IoT cloud platform.  I was in the first community focus group and was one their alpha testers.  Now that Particle has publicly announced their support for Raspberry Pi, anyone can sign up for the [Open Beta](https://www.particle.io/products/development-tools/raspberry-pi-on-particle).

Particle on Raspberry Pi allows you to bring the features of the Particle Cloud to the full features of Raspberry Pi and create amazing projects that could not have been easily created previously.

With Particle on Raspberry Pi, you can use the same C++ code for Particle's other devices to create powerful solutions.

Particle on Raspberry Pi provides complete access to the forty IO pins on Raspberry Pi, allowing you to do digital and analog interactions with your favorite electronics.  Additionally, Particle on Raspberry Pi allows you to create functions for calling virtually any Bash command or script present on Raspberry Pi, facilitating dynamic, flexible interaction from within firmware.

Currently, Particle on Raspberry Pi is optimized for the [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/), the [Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/), the [Raspberry Pi Zero](https://www.raspberrypi.org/products/pi-zero/), and the [Raspberry Pi B+](https://www.raspberrypi.org/products/model-b-plus/).  Particle is currently working to add support for the newly released [Raspberry Pi Zero Wireless](https://www.raspberrypi.org/products/pi-zero-wireless/).

<hr>

## Setup

The first step is to make sure your Raspberry Pi is updated to the latest version of Raspbian Jessie and it is connected to your network.  Update your Pi's software using the following command:

{% highlight shell %}
$ sudo apt update && sudo apt upgrade
{% endhighlight %}

Next, once you have been accepted into the [Raspberry Pi on Particle Beta](https://www.particle.io/products/development-tools/raspberry-pi-on-particle), run the following command on your Raspberry Pi to install [`particle-agent`](https://github.com/spark/particle-agent):

{% highlight shell %}
$ bash <( curl -sL https://particle.io/install-pi )
{% endhighlight %}

During the installation of [`particle-agent`](https://github.com/spark/particle-agent), you will be asked to log in using your Particle credentials in order to claim your Raspberry Pi to your account.

Once the installation has completed, you can use your Raspberry Pi with Particle's tools.  You can build and flash firmware using the [Web IDE](https://build.particle.io), [Particle Dev](https://www.particle.io/products/development-tools/particle-desktop-ide), [particle-cli](https://www.particle.io/products/development-tools/particle-command-line-interface), or [po-util](https://nrobinson2000.github.io/po-util/) to build locally.

<hr>

## Running bash commands with Particle

Particle on Raspberry Pi supports running bash commands and scripts as Linux processes from within the firmware.  Input can be supplied with arguments and stdin, and output can can be captured for use in your firmware.

Here is an example for retrieving the internal CPU temperature of the Raspberry Pi:

{% highlight cpp %}
#include "Particle.h"

void setup()
{
  Process proc = Process::run("vcgencmd measure_temp");
  proc.wait();
  // The output is temp=43.5'C, so read past the = and parse the number
  proc.out().find("=");
  float cpuTemp = proc.out().parseFloat();
  Particle.publish("cpu-temp", String(cpuTemp));
}

void loop()
{
 // Nothing in the loop
}
{% endhighlight %}

<hr>

## More information

Particle is thoroughly documenting Particle on Raspberry Pi.  [Their official documentation](https://docs.particle.io/guide/getting-started/intro/raspberry-pi/) provides all provides a multitude of information, reference material, and tutorials.  [Their setup guide for Raspberry Pi is comprehensive.](https://docs.particle.io/guide/getting-started/start/raspberry-pi/)

For me the most useful section of the documentation is the [Firmware Reference](https://docs.particle.io/reference/firmware/raspberry-pi/).

Additionally, the [Particle Community](https://community.particle.io) is always available to help with any issues or questions.
