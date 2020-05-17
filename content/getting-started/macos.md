---
title: "macOS"
date: 2020-04-10T11:41:50+02:00
draft: false
weight: 2
---

This page has information on how to install and use GoCV on macOS.

### Installing

Install the GoCV package:

    go get -u -d gocv.io/x/gocv

You can install OpenCV 4.3.0 using Homebrew.

If you already have an earlier version of OpenCV installed, you should update:

	brew upgrade opencv

If this is your first time install OpenCV 4.3.0:

	brew install opencv

### pkgconfig Installation

pkg-config is used to determine the correct flags for compiling and linking OpenCV.
You can install it by using Homebrew:
    
    brew install pkgconfig
	
### How to build/run code

Once you have installed OpenCV, you should be able to build or run any of the command examples:

	go run ./cmd/version/main.go

The version program should output the following:

	gocv version: 0.23.0
	opencv lib version: 4.3.0

### Cache builds

If you are running a version of Go older than v1.10 and not modifying GoCV source, precompile the GoCV package to significantly decrease your build times:

	go install gocv.io/x/gocv

### Custom Environment

By default, pkg-config is used to determine the correct flags for compiling and linking OpenCV. This behavior can be disabled by supplying `-tags customenv` when building/running your application. When building with this tag you will need to supply the CGO environment variables yourself.

For example:

	export CGO_CXXFLAGS="--std=c++11"
	export CGO_CPPFLAGS="-I/usr/local/Cellar/opencv/4.3.0/include"
	export CGO_LDFLAGS="-L/usr/local/Cellar/opencv/4.3.0/lib -lopencv_stitching -lopencv_superres -lopencv_videostab -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dpm -lopencv_face -lopencv_photo -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_line_descriptor -lopencv_optflow -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_dnn -lopencv_plot -lopencv_xfeatures2d -lopencv_shape -lopencv_video -lopencv_ml -lopencv_ximgproc -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_flann -lopencv_xobjdetect -lopencv_imgcodecs -lopencv_objdetect -lopencv_xphoto -lopencv_imgproc -lopencv_core"

Please note that you will need to run these 3 lines of code one time in your current session in order to build or run the code, in order to setup the needed ENV variables. Once you have done so, you can execute code that uses GoCV with your custom environment like this:

	go run -tags customenv ./cmd/version/main.go
