---
title: "OS X"
date: 2017-10-09T11:41:50+02:00
draft: false
weight: 2
---

This page has information on how to install and use GoCV on OS X.

### Installing

Install the GoCV package:

        go get -u -d gocv.io/x/gocv

Now, install OpenCV 3.4 using Homebrew:

		brew install opencv

If you already have an earlier version of OpenCV installed, you should probably upgrade it to the latest version, instead of installing:

		brew upgrade opencv

### How to build/run code

In order to build/run Go code that uses this package, you will need to specify the location for the include and lib files in your GoCV installation. If you have used Homebrew to install OpenCV 3.3, the following instructions should work.

First, you need to change the current directory to the location of the GoCV repo:

		cd $GOROOT/src/gocv.io/x/gocv

One time per session, you must run the script:

		source ./env.sh

Now you should be able to build or run any of the command examples:

		go run ./cmd/version/main.go

The version program should output the following:

		gocv version: 0.8.0
		opencv lib version: 3.4.0

You might want to copy the `env.sh` script into your own projects, to make it easier to setup these vars when building your own code.

If you are not modifying gocv source, compile gocv to a static library, to significantly decrease your build times (`env.sh` must have been executed as described above):

        go install gocv.io/x/gocv
