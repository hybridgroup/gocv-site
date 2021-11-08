---
title: "Linux"
date: 2020-04-10T11:41:45+02:00
draft: false
weight: 1
---

This page has information on how to install and use GoCV on Ubuntu, as well as other Linux distros.

You can use `make` to install OpenCV 4.5.4 with the handy `Makefile` included with this repo. If you already have installed OpenCV, you do not need to do so again. The installation performed by the `Makefile` is minimal, so it may remove OpenCV options such as Python or Java wrappers if you have already installed OpenCV some other way.

### Installing

Install the GoCV package:

    go get -u -d gocv.io/x/gocv

Change directories into the newly installed package directory:

    cd $GOPATH/src/gocv.io/x/gocv

Now you can run the needed installation steps listed below.

#### Quick Install

The following commands should do everything to download and install OpenCV 4.5.4 on Linux:

	make install

If it works correctly, at the end of the entire process, the following message should be displayed:

	gocv version: 0.29.0
	opencv lib version: 4.5.4

That's it, now you are ready to use GoCV.

#### Complete Install

Here are details for each of the steps that are performed during the "Quick Install". If you have already run `make install` as described above, you do not need to run any of these commands.

##### Install required packages

First, you need to update the system, and install any required packages:

	make deps

#### Download source

Now, download the OpenCV 4.5.4 and OpenCV Contrib source code:

	make download

#### Build

Build everything. This will take quite a while:

	make build

#### Install

Once the code is built, you are ready to install:

	make sudo_install

### Verifying the installation

To verify your installation you can run one of the included examples.

First, change the current directory to the location of the GoCV repo:

	cd $GOPATH/src/gocv.io/x/gocv

Now you should be able to build or run any of the examples:

	go run ./cmd/version/main.go

The version program should output the following:

	gocv version: 0.29.0
	opencv lib version: 4.5.4

#### Cleanup extra files

After the installation is complete, you can remove the extra files and folders:

	make clean

### Cache builds

If you are running a version of Go older than v1.10 and not modifying GoCV source, precompile the GoCV package to significantly decrease your build times:

	go install gocv.io/x/gocv

### Custom Environment

By default, pkg-config is used to determine the correct flags for compiling and linking OpenCV. This behavior can be disabled by supplying `-tags customenv` when building/running your application. When building with this tag you will need to supply the CGO environment variables yourself.

For example:

	export CGO_CPPFLAGS="-I/usr/local/include"
	export CGO_LDFLAGS="-L/usr/local/lib -lopencv_core -lopencv_face -lopencv_videoio -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_video -lopencv_dnn -lopencv_xfeatures2d"

Please note that you will need to run these 2 lines of code one time in your current session in order to build or run the code, in order to setup the needed ENV variables. Once you have done so, you can execute code that uses GoCV with your custom environment like this:

	go run -tags customenv ./cmd/version/main.go

### Alpine 3.7 Docker image

There is a Docker image with Alpine 3.7 that has been created by project contributor [@denismakogon](https://github.com/denismakogon). You can find it located at [https://github.com/denismakogon/gocv-alpine](https://github.com/denismakogon/gocv-alpine).
