---
title: "Go, OpenCV, Caffe, and Tensorflow: Putting It All Together With GoCV"
draft: false
weight: 2
---

We are pleased to present to you the new GoCV ([https://gocv.io](https://gocv.io)) version 0.8.0, which is our first release of 2018. This is a big update and adds a lot of powerful new functionality, along with bug fixes and more documentation, thanks to our wonderful contributors.

The new version adds support for the [OpenCV Deep Neural Network (DNN) module](https://docs.opencv.org/3.4.0/d6/d0f/group__dnn.html), which means you can now use Caffe ([http://caffe.berkeleyvision.org/](http://caffe.berkeleyvision.org/)) and Tensorflow ([https://www.tensorflow.org/](https://www.tensorflow.org/)) models from your GoCV code. GoCV also now has complete support for the latest [Intel Computer Vision SDK with Photography Vision Library (PVL)](https://software.intel.com/en-us/cvsdk-devguide-advanced-face-capabilities-in-intels-opencv) including face recognition.

There is lots more, check out the full changelog at [https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#080](https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#080) for complete details, or read on for the highlights.

### We're Going Deep

The availability of useful trained deep neural networks for fast image classification based on Caffe and Tensorflow adds a new level of possibility to computer vision applications. GoCV can now load Caffe and Tensorflow models, and then use them as part of your Golang application. Check out this example code which in less than 40 lines of Go code uses the Tensorflow "Inception" model for image recognition on an incoming video camera feed:

```go
package main

import (
    "fmt"
    "image"

    "gocv.io/x/gocv"
)

func main() {
    webcam, _ := gocv.VideoCaptureDevice(0)
    img := gocv.NewMat()
    net := gocv.ReadNetFromTensorflow("/path/to/tensorflow_inception_graph.pb")

    for {
        // read image form camera
        webcam.Read(img)

        // convert to a 224x244 image blob that can be processed by Tensorflow
        blob := gocv.BlobFromImage(img, 1.0, image.Pt(224, 244), gocv.NewScalar(0, 0, 0, 0), true, false)
        defer blob.Close()

        // feed the blob into the classifier
        net.SetInput(blob, "input")

        // run a forward pass thru the network
        prob := net.Forward("softmax2")
        defer prob.Close()

        // reshape the results into a 1x1000 matrix
        probMat := prob.Reshape(1, 1)
        defer probMat.Close()

        // determine the most probable classification, and display it
        _, maxVal, _, maxLoc := gocv.MinMaxLoc(probMat)
        fmt.Printf("maxLoc: %v, maxVal: %v\n", maxLoc, maxVal)

        gocv.WaitKey(1)
    }
}
```

For a complete example using GoCV with Tensorflow take a look at [https://github.com/hybridgroup/gocv/blob/master/cmd/tf-classifier/main.go](https://github.com/hybridgroup/gocv/blob/master/cmd/tf-classifier/main.go). For an example using GoCV with Caffe take a look at [https://github.com/hybridgroup/gocv/blob/master/cmd/caffe-classifier/main.go](https://github.com/hybridgroup/gocv/blob/master/cmd/caffe-classifier/main.go).

### Recognizing Faces With The Intel CV SDK PVL

We now offer complete support for the Intel Computer Vision (CV) SDK, by adding the Photography Vision Library (PVL) FaceRecognizer. The FaceRecognizer is a fast and simple interface to this very common feature needed for many CV applications. You can just load in a PVL FaceDetector and a FaceRecognizer like this:

```go
fd := pvl.NewFaceDetector()
fr := pvl.LoadFaceRecognizer("/path/to/db.xml")
```

And then use them to detect and recognize faces from the video camera:

```go
for {
    webcam.Read(img)
    gocv.CvtColor(img, imgGray, gocv.ColorBGRToGray)

    // detect faces
    faces := fd.DetectFaceRect(imgGray)
    if len(faces) > 0 {
        // try to recognize the face
        personIDs, _ = fr.Recognize(imgGray, faces)

        // set the color of the box based on if the face is recognized
        color := blue
        msg := "Unknown"
        if len(personIDs) > 0 && personIDs[0] != pvl.UnknownFace {
            color = green
            msg = strconv.Itoa(personIDs[0])
        }

        // draw a rectangle the face on the original image,
        // along with text identifing if recognized
        rect := faces[0].Rectangle()
        gocv.Rectangle(img, rect, color, 3)

        size := gocv.GetTextSize(msg, gocv.FontHersheyPlain, 1.2, 2)
        pt := image.Pt(rect.Min.X+(rect.Min.X/2)-(size.X/2), rect.Min.Y-2)
        gocv.PutText(img, msg, pt, gocv.FontHersheyPlain, 1.2, color, 2)
    }

    // show the image in the window, and wait 1 millisecond
    window.IMShow(img)
    window.WaitKey(1)
}
```

### Keep Sight of What We're Doing

We're continuously making improvements to GoCV, with a very active community forming around Golang for computer vision applications. Please follow us on Twitter at [@GoCVio](https://twitter.com/GoCVio) for the latest and greatest news about the project.
