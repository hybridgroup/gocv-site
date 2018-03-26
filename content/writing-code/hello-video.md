---
title: "Hello Video"
date: 2018-03-26T18:00:37+02:00
draft: false
weight: 1
---

This example opens a video capture device using device "0", reads frames, and shows the video in a GUI window:

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

