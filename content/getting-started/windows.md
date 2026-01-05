---
title: "Windows"
date: 2020-04-10T11:41:54+02:00
draft: false
weight: 3
---

This page has information on how to install and use GoCV on Microsoft Windows 10+, 64-bit.

## Installing

In order to use GoCV on Windows you must build and install OpenCV 4.13.0. First download and install MinGW-W64 and CMake, as follows.

### MinGW-W64

Download and run the MinGW-W64 compiler installer from [https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/]https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/).

The latest version of the MinGW-W64 toolchain is `13.0.0`, but any version from `8.X` on should work.

Choose the options for "posix" threads, and for "seh" exceptions handling, then install to the default location.

Add the install path to your system path.

### CMake

Download and install CMake [https://cmake.org/download/](https://cmake.org/download/) to the default location. CMake installer will add CMake to your system path.

### Clone the GoCV repo

	git clone https://github.com/hybridgroup/gocv.git

### OpenCV 4.13.0 and OpenCV Contrib Modules

The following commands should do everything to download and install OpenCV 4.13.0 on Windows:

	chdir gocv
	.\win_download_opencv.cmd
	.\win_build_opencv.cmd

It will probably take at least 1 hour to download and build.

Last, add `C:\opencv\build\install\x64\mingw\bin` to your System Path.

### Verifying the installation

Change the current directory to the location of the GoCV repo:

	chdir gocv

Now you should be able to build or run any of the command examples:

	go run cmd\version\main.go

The version program should output the following:

	gocv version: 0.43.0
	opencv lib version: 4.13.0

That's it, now you are ready to use GoCV.

### Custom Environment

By default, OpenCV is expected to be in `C:\opencv\build\install\include`. This behavior can be disabled by supplying `-tags customenv` when building/running your application. When building with this tag you will need to supply the CGO environment variables yourself.

Due to the way OpenCV produces DLLs, including the version in the name, using this method is required if you're using a different version of OpenCV.

For example:

	set CGO_CXXFLAGS=--std=c++11
	set CGO_CPPFLAGS=-IC:\opencv\build\install\include
	set CGO_LDFLAGS=-LC:\opencv\build\install\x64\mingw\lib -lopencv_core4130 -lopencv_face4130 -lopencv_videoio4130 -lopencv_imgproc4130 -lopencv_highgui4130 -lopencv_imgcodecs4130 -lopencv_objdetect4130 -lopencv_features2d4130 -lopencv_video4130 -lopencv_dnn4130 -lopencv_xfeatures2d4130 -lopencv_plot4130 -lopencv_tracking4130 -lopencv_img_hash4130 -lopencv_calib3d4130

Please note that you will need to run these 3 lines of code one time in your current session in order to build or run the code, in order to setup the needed ENV variables. Once you have done so, you can execute code that uses GoCV with your custom environment like this:

	go run -tags customenv cmd\version\main.go
