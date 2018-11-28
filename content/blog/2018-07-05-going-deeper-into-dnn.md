---
title: "Going Deeper Into DNN For Computer Vision"
draft: false
weight: 36
---

We have just released GoCV ([https://gocv.io](https://gocv.io)) version 0.14.0, our Go programming language package for computer vision using OpenCV. Thanks to the efforts of our contributors and team this is one of our most significant updates yet.

This new GoCV version has been timed to land in parallel with the just shipped OpenCV 3.4.2 release, which features some substantial new functionality for using Deep Neural Networks (DNN) for computer vision. The new GoCV also supports the [Intel OpenVINO](https://software.intel.com/en-us/openvino-toolkit) Inference Engine for DNN hardware acceleration on CPU's, GPU's, and VPU's too.

### Into The Nets

The new GoCV release offers some big enhancements that are part of the updated OpenCV DNN module. Most importantly, using the new `gocv.ReadNet()` function you can open models from either [Caffe](http://caffe.berkeleyvision.org), [Tensorflow](http://tensorflow.org), [Torch](http://torch.ch), or [DarkNet](https://pjreddie.com/darknet/yolo/), then use to process visual information.

This single line of code loads a Caffe SSD model and configuration file, and prepares it for processing:

```go
net := gocv.ReadNet("res10_300x300_ssd_iter_140000.caffemodel", "res10_300x300_ssd_iter_140000.prototxt")
```

Here is the same code, but loading a Tensorflow SSD model and configuration file instead:

```go
net := gocv.ReadNet("ssd_mobilenet_v1_coco.pb", "ssd_mobilenet_v1_coco.pbtxt")
```

If you are using Intel OpenVINO, which is a set of tools from Intel for DNN development that works with GoCV/OpenCV, just by adding 2 lines of code, you can also take advantage of hardware acceleration.

This code uses the OpenVINO backend with a connected GPU using 16-bit floating point values to process the Tensorflow model:

```go
net := gocv.ReadNet("ssd_mobilenet_v1_coco.pb", "ssd_mobilenet_v1_coco.pbtxt")

net.SetPreferableBackend(gocv.NetBackendOpenVINO)
net.SetPreferableTarget(gocv.NetTargetFP16)
```

And then this code uses the OpenVINO backend with a connected [Intel Movidius](https://www.movidius.com/) Video Processing Unit (VPU)]to process the same Tensorflow model:

```go
net := gocv.ReadNet("ssd_mobilenet_v1_coco.pb", "ssd_mobilenet_v1_coco.pbtxt")

net.SetPreferableBackend(gocv.NetBackendOpenVINO)
net.SetPreferableTarget(gocv.NetTargetVPU)
```

The OpenVINO Inference Engine backend compiles the model for processing on the target device, and then you can just use it with the same GoCV code as you would use with the CPU or GPU.

We have several new examples for object classification, object tracking, and other applications using DNNs that you can find in our Github repo.

### There Is More

This release also includes many more contributions from project collaborators both experienced and new. Bugfixes, new conversions, improved installation scripts, including for Windows and more. Thank you everyone, we appreciate your efforts and support. Check out the full changelog at [https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#0140](https://github.com/hybridgroup/gocv/blob/master/CHANGELOG.md#0140) for complete details.

### Stay In Touch

Stay informed of all our project activity by following us on Twitter at [@GoCVio](https://twitter.com/GoCVio) for the latest news and updates.
