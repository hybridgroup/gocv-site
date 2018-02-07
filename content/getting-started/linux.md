---
title: "Linux"
date: 2017-10-09T11:41:45+02:00
draft: false
weight: 1
---

This page has information on how to install and use GoCV on Ubuntu, as well as other Linux distros.

You can use `make` to install OpenCV 3.4 with the handy `Makefile` included with this repo. If you already have installed OpenCV, you do not need to do so again. The installation performed by the `Makefile` is minimal, so it may remove OpenCV options such as Python or Java wrappers if you have already installed OpenCV some other way.

### Installing

Install the GoCV package:

        go get -u -d gocv.io/x/gocv

Change directories into the newly installed package directory:

        cd $GOPATH/src/gocv.io/x/gocv

Now you can run the needed installation steps listed below.

#### Install required packages

First, you need to update the system, and install any required packages:

		make deps

#### Download source

Next, download the OpenCV 3.4 and OpenCV Contrib source code:

		make download

#### Build

Build and install everything. This will take quite a while:

		make build

#### Cleanup extra files

After the installation is complete, you can remove the extra files and folders:

		make cleanup

### How to build/run code

In order to build/run Go code that uses this package, you will need to specify the location for the include and lib files in your GoCV installation.

One time per session, you must run the script:

		source ./env.sh

Now you should be able to build or run any of the examples:

		go run ./cmd/version/main.go

The version program should output the following:

		gocv version: 0.8.0
		opencv lib version: 3.4.0

You might want to copy the `env.sh` script into your own projects, to make it easier to setup these vars when building your own code.

If you are not modifying gocv source, compile gocv to a static library, to significantly decrease your build times (`env.sh` must have been executed as described above):

        go install gocv.io/x/gocv
