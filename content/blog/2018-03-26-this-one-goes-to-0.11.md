---
title: "This One Goes to 0.11"
draft: false
weight: 1
---

It is with great pleasure that we release to you our latest GoCV ([https://gocv.io](https://gocv.io)) version 0.11.0 This is an important update, because it slightly changes the API from previous versions. By this we mean breaking changes, which we will explain below.

We of course have lots of new functionality and features, as always largely due to our amazing contributors. Thank you to everyone who has added code, documentation, or feedback to the project in preparation for this release.

You can take a look at the full changelog at [https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#0110](https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#0110) for complete details, or read on for the human generated summary of our collective work.

### Clearing Up The Mat

After a [conversation on a Github issue](https://github.com/hybridgroup/gocv/issues/137), one fact became clear: the way we have been using `Mat` with GoCV has been a little "non-standard" relative to typical idiomatic Go code. The problem is well stated in the Github issue. In short, some GoCV functions changed the underlying `Mat` even though Go passes all parameters by value. How is this possible? It worked due to GoCV's `Mat` actually being a wrapper using CGo around the `cv::Mat` type in OpenCV.

But to your typical Go programmer, it looks extremely weird. So after a lot of discussion, we are making the following change across all of GoCV: whenever a `Mat` will be changed by OpenCV, due to being an output or destination parameter, GoCV will expect that parameter be a pointer to a `Mat` aka `Mat*` to indicate clearly as part of the GoCV API that the data in that `Mat` will be modified.

Take a look at the "Hello, Video" code using the new pointer-based parameter passing for output params:

```go
package main

import (
	"gocv.io/x/gocv"
)

func main() {
	webcam, _ := gocv.VideoCaptureDevice(0)
	window := gocv.NewWindow("Hello")
	img := gocv.NewMat()

	for {
		webcam.Read(&img)
		window.IMShow(img)
		window.WaitKey(1)
	}
}
```

The important difference is in this line:

```go
		webcam.Read(&img)
```

We are now passing a pointer to `img` by using the `&` operator e.g. `&img`. 

This is now the case for any GoCV functions that modify the destination `Mat`. For example:

```go
gocv.Threshold(imgDelta, &imgThresh, 25, 255, gocv.ThresholdBinary)
```

Most Go programmers will recognize passing a pointer to a struct as the idiomatic way to pass a struct "by reference", and to expect that the data can and will be changed within the function being called.

So why did we not just create a new `Mat` to return to the caller similar to how the Python API for OpenCV operates?

This results in less efficient code execution because of needing to perform additional CGo heap allocations of the additional C++ `cv::Mat` structures and data using OpenCV vs. being able to reuse the same memory due to the C++ `cv::Mat*` that the GoCV `Mat` wraps around. 

We want GoCV code to be as fast as executing the equivalent C++ but also able to use the concurrency of Go. More about this in a upcoming blog post...

### A Whole Lot More In This Release

Every release we are fortunate to have contributions from both project newcomers and existing project collaborators. This release includes many improvements to GoCV core, yet even more imgproc filters, a whole set of new Trackers for object tracking, many bugfixes, and the ongoing important work of documentation and samples. Thank you to everyone!

### Keep Track of What We're Up To

Want to keep up with all our project activity? Follow us on Twitter at [@GoCVio](https://twitter.com/GoCVio) for the latest updates about the project.
