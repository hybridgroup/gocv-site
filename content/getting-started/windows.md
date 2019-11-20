---
title: "Windows"
date: 2017-10-09T11:41:54+02:00
draft: false
weight: 3
---

This page has information on how to install and use GoCV on Microsoft Windows 10, 64-bit.

### Installing

Install the GoCV package:

    go get -u -d gocv.io/x/gocv

In order to use GoCV on Windows you must build and install OpenCV 4.1.2. First download and install MinGW-W64 and CMake, as follows.

#### MinGW-W64

Download and run the MinGW-W64 compiler installer from [https://sourceforge.net/projects/mingw-w64/?source=typ_redirect](https://sourceforge.net/projects/mingw-w64/?source=typ_redirect).

The latest version of the MinGW-W64 toolchain is `7.3.0`, but any version from `7.X` on should work.

Choose the options for "posix" threads, and for "seh" exceptions handling, then install to the default location `c:\Program Files\mingw-w64\x86_64-7.3.0-posix-seh-rt_v5-rev2`.

Add the `C:\Program Files\mingw-w64\x86_64-7.3.0-posix-seh-rt_v5-rev2\mingw64\bin` path to your System Path.

#### CMake

Download and install CMake [https://cmake.org/download/](https://cmake.org/download/) to the default location. CMake installer will add CMake to your system path.

#### OpenCV 4.1.2 and OpenCV Contrib Modules

The following commands should do everything to download and install OpenCV 4.1.2 on Windows:

	chdir %GOPATH%\src\gocv.io\x\gocv
	win_build_opencv.cmd

It will probably take at least 1 hour to download and build.

Last, add `C:\opencv\build\install\x64\mingw\bin` to your System Path.

### Verifying the installation

Change the current directory to the location of the GoCV repo:

	chdir %GOPATH%\src\gocv.io\x\gocv

Now you should be able to build or run any of the command examples:

	go run cmd\version\main.go

The version program should output the following:

	gocv version: 0.21.0
	opencv lib version: 4.1.2

That's it, now you are ready to use GoCV.

#### Cache builds

If you are running a version of Go older than v1.10 and not modifying GoCV source, precompile the GoCV package to significantly decrease your build times:

	go install gocv.io/x/gocv

### Custom Environment

By default, OpenCV is expected to be in `C:\opencv\build\install\include`. This behavior can be disabled by supplying `-tags customenv` when building/running your application. When building with this tag you will need to supply the CGO environment variables yourself.

Due to the way OpenCV produces DLLs, including the version in the name, using this method is required if you're using a different version of OpenCV.

For example:

	set CGO_CXXFLAGS=--std=c++11
	set CGO_CPPFLAGS=-IC:\opencv\build\install\include
	set CGO_LDFLAGS=-LC:\opencv\build\install\x64\mingw\lib -lopencv_core412 -lopencv_face412 -lopencv_videoio412 -lopencv_imgproc412 -lopencv_highgui412 -lopencv_imgcodecs412 -lopencv_objdetect412 -lopencv_features2d412 -lopencv_video412 -lopencv_dnn412 -lopencv_xfeatures2d412 -lopencv_plot412 -lopencv_tracking412 -lopencv_img_hash412 -lopencv_calib3d412

Please note that you will need to run these 3 lines of code one time in your current session in order to build or run the code, in order to setup the needed ENV variables. Once you have done so, you can execute code that uses GoCV with your custom environment like this:

	go run -tags customenv cmd\version\main.go
