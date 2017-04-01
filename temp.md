> **The Particle Web IDE, Particle Dev, and particle-cli are not the only ways to program Particle devices.**

<p align="center">
<img src="/images/po-util-updated.svg">
</p>

## What is po-util?

[Po-util](https://github.com/nrobinson2000/po-util) is a tool I maintain for facilitating the ultimate local Particle development experience. In non-technical terms, po-util takes the hassle out of developing Particle projects locally without the need for the Particle cloud to do the firmware compiling for you.

Po-util uses the GCC for ARM Cortex processors tool-chain to build your projects using the [Particle Firmware Source](https://github.com/spark/firmware) directly on your computer.

Po-util is a bash shell script that it is available on GitHub for [Linux](https://github.com/nrobinson2000/po-util) and [macOS](https://github.com/nrobinson2000/homebrew-po).

## Features of po-util

There are are many more features to po-util than just aliasing a few `make` commands.  Po-util is unique work-flow I meticulously created so that others can experience local Particle development the way it was meant to be.

#### Po-util offers many features, including:

* Full installation of required dependencies.

* Support for building firmware for the Particle Photon, Electron, P1, Core, Raspberry Pi, and Redbear Duo.

* "Instant" firmware uploading over USB using DFU Utilities.

* Sequential Over The Air firmware uploading to multiple devices in a "product".

* A unified project structure that is cross-compatible between the Linux and macOS editions of po-util.

* An efficient library manager that works with libraries from Particle Libraries 2.0 or GitHub.

## Getting started with po-util

Installing po-util and using po-util is a pleasurable experience.  (You must be an administrator on your computer however.)

For Linux Distributions based on Debian, Red Hat, or Arch, you can run the following in your Terminal to install po-util:

```bash
$ bash <( curl -sL https://git.io/vX4cl ) install
```

On macOS, you can install po-util using [Homebrew](https://brew.sh):

```
$ brew tap nrobinson2000/po
$ brew install po
$ po install
```

## Creating a project with po-util

To use po-util, you must work in an initialized project directory (more on project structure below).

To create a project simply run the following in your Terminal:

```bash
$ po init photon newProject
```

This command creates the `newProject` folder and creates the appropriate folders and files inside.  The [Atom](https://atom.io) shortcuts file is set to build firmware for the Particle Photon.

To make use of the shortcuts you will need the [Atom Build package](https://atom.io/packages/build).  You can easily the package and a couple other recommended ones with:

```bash
$ po setup-atom
```

## Po-util project structure

Po-util operates by keeping your code separated in projects that follow an organized structure:

```
firmware/
└ main.cpp

bin/
└ firmware.bin

devices.txt
libs.txt
.atom-build.yml
README.md
```

All source code goes in `firmware/main.cpp`, and the compiled binary is saved as `bin/firmware.bin`. You can specify devices to flash sequentially to in `devices.txt`, and `libs.txt` is how po-util keeps track of what libraries to use in the project.  Po-util supports build shortcuts for [Atom](https://atom.io), and these are set in `.atom-build.yml` should you need to modify it on a per-project basis.

## Using po-util within Atom

Now that you have created a project and installed the recommended packages, open the project folder in Atom.

On the left, you should find your `firmware/main.cpp` file.  This is where you should keep most of your code.  (You can use separate files and libraries however, but the majority of your code should be kept here.)

When your project is initialized, you will find the following dummy code in your `firmware/main.cpp`:

```cpp
#include "Particle.h"

void setup() // Put setup code here to run once
{

}

void loop() // Put code here to loop forever
{

}
```

Here is where you can observe one of the most useful features of po-util, the keyboard shortcuts for Atom.  If you press `Ctrl + Alt + 1`, Atom will build the project by running:

```bash
$ po photon build
```

Pressing `Ctrl + Alt + 2` builds the firmware and flashes it to your device over USB using DFU Utilities by running the following, which can also be done from your Terminal:

```bash
$ po photon flash
```

Pressing `Ctrl + Alt + 3` cleans the firmware and removes the `bin/` directory by running:

```bash
$ po photon clean
```

Pressing `Ctrl + Alt + 4` uploads firmware to your device without rebuilding first by running:

```bash
$ po photon dfu
```

Pressing `Ctrl + Alt + 5` uploads your firmware Over The Air to any devices listed in your `devices.txt` by running;

```bash
$ po photon ota --multi
```

## Configuring your devices to work with po-util

All devices that po-util will use DFU Utilities to upload firmware to must have their firmware patched/upgraded in order to allow po-util to automatically put them into DFU mode.  To do so, manually put your device into DFU mode and then run the following to update the system firmware on your device:

```bash
$ po photon update
```


>By default, all Particle devices will go into DFU mode when the computer they are connected to sets the serial baud rate to `14400`.  However, on Linux, baud rates can only be set to specific values and not an arbitrary value like `14400`.  Po-util solves this caveat by using `19200` as the baud rate to put the device into DFU mode.  You can use `14400` on macOS by running `$ po config` and choosing `default` when prompted for a baud rate.

## Using libraries with po-util

One of the greatest features of po-util is its flexible library manager.  The library manager not only allows using libraries from Particle's list of libraries, but it also supports using any library that is available from GitHub.

A library can be downloaded and added to a project by using the `po lib get` and `po lib add` commands.

For example, you could add the `neopixel` library to a project with the following:

```bash
$ po lib get neopixel
$ po lib add neopixel
```

You could go even further and load an example from the `neopixel` library using the library example manager.  

First, check what examples are available with:

```bash
$ po lib ex ls neopixel
```

To load the `extra-examples` example into your project run:

```bash
$ po lib ex load neopixel extra-examples
```

## Sharing a po-util project

The most practical way to share a po-util project is to turn it into a git repository, but po-util provides another method.  Using `$ po lib pack`, you can create a copy of your `firmware/` directory with all symbolic links and libraries packaged inside.  A tar.gz archives is created as well.

When using publicly accessible libraries is advised that you clean the binaries and libraries from your project.  This removes the symbolic links from your `firmware` directory, but leaves the libraries your project depends on in your `libs.txt`.

To clean your project run the following:

```bash
$ po DEVICE clean
$ po lib clean
```

At any time, you can re-download and add the libraries using:

```bash
$ po lib setup
```

## More po-util information

For more information about po-util, check the man page with `$ man po`, check out [the website](https://nrobinson2000.github.io/po-util/), or consult the [repository on GitHub](https://github.com/nrobinson2000/po-util).

Stars, Feedback, Contributions and Donations are greatly appreciated.  I hope you enjoy using po-util as much as I have enjoyed creating it.

[Linux Edition Repo](https://github.com/nrobinson2000/po-util)

[macOS Edition Repo](https://github.com/nrobinson2000/homebrew-po)

<p>
    <a href="https://www.paypal.me/nrobinson2000"><img src="https://img.shields.io/badge/paypal-donate-009cde.png"></a>
    <a href="http://nrobinson2000.github.io/donate-bitcoin/?currency=USD"><img src="https://img.shields.io/badge/bitcoin-donate-orange.png"></a>
</p>
