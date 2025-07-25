---
title: "Docker"
date: 2025-07-25T11:41:45+02:00
draft: false
weight: 4
---

This page has information on how to install and use GoCV on Docker.

## Pre-built Docker images

There are a number of pre-built Docker images that you can use, or use as a starting point for your own needs. Please see https://hub.docker.com/r/gocv/opencv

## Building from source

The project provides several Docker files which let you build [GoCV](https://gocv.io/) images which you can then use to run `GoCV` applications in Docker containers. The `Makefile` contains `docker` target which lets you build Docker image with a single command:

```
make docker
```

By default Docker image built by running the command above ships [Go](https://golang.org/) version `1.24.4`, but if you would like to build an image which uses different version of `Go` you can override the default value when running the target command:

```
make docker GOVERSION='1.24.5'
```

## Running GUI programs in Docker on macOS

Sometimes your `GoCV` programs create graphical interfaces like windows eg. when you use `gocv.Window` type when you display an image or video stream. Running the programs which create graphical interfaces in Docker container on macOS is unfortunately a bit elaborate, but not impossible. First you need to satisfy the following prerequisites:

- install [xquartz](https://www.xquartz.org/). You can also install xquartz using [homebrew](https://brew.sh/) by running `brew cask install xquartz`
- install [socat](https://linux.die.net/man/1/socat) `brew install socat`

Note, you will have to log out and log back in to your machine once you have installed `xquartz`. This is so the X window system is reloaded.

Once you have installed all the prerequisites you need to allow connections from network clients to `xquartz`. Here is how you do that. First run the following command to open `xquart` so you can configure it:

```shell
open -a xquartz
```

Click on _Security_ tab in preferences and check the "Allow connections" box:

![app image](./images/xquartz.png)

Next, you need to create a TCP proxy using `socat` which will stream [X Window](https://en.wikipedia.org/wiki/X_Window_System) data into `xquart`. Before you start the proxy you need to make sure that there is no process listening in port `6000`. The following command should **not** return any results:

```shell
lsof -i TCP:6000
```

Now you can start a local proxy which will proxy the X Window traffic into xquartz which acts a your local X server:

```shell
socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"
```

You are now finally ready to run your `GoCV` GUI programs in Docker containers. In order to make everything work you must set `DISPLAY` environment variables as shown in a sample command below:

```shell
docker run -it --rm -e DISPLAY=docker.for.mac.host.internal:0 your-gocv-app
```

**Note, since Docker for MacOS does not provide any video device support, you won't be able run GoCV apps which require camera.**
