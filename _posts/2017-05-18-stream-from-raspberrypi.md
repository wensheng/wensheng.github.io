---
layout: post
title: How to stream from RaspberryPi PiCamera
ghissueid: 5
---

## Pre-requisites

OS: Raspbian >= 8

Do `sudo raspi-config` and enable PiCamera, in 'Memory Split', give GPU more than 128M of memory.  Connect PiCamera, reboot.

Test PiCamera works by `raspistill -o photo.jpg`.

Load v4l2 module:

    sudo modprobe bcm2835-v412

Optionally add `bcm2835-v412` to `/etc/modules` file.

v4l2 should already present in the OS, if not: 

    sudo apt-get install v4l-utils

There're multiple ways to stream PiCamera to remote clients. Here I present 4.

## Method 1: vlc

Make sure vlc is installed: 

    sudo apt-get install vlc

shell script:

```bash
raspivid -o - -n -fps 15 -w 1280 -h 720 -t 0 \
|cvlc -vvv stream:///dev/stdin \
--sout '#rtp{sdp=rtsp://:8554/}' \
:demux=h264 :h264-fps=15

```

When you run the script, it will stream, the stream can be viewed from `rtsp://raspberrypi_ip:8554/` from VLC on a remote computer.

## Method 2: v4l2rtspserver

Details are in [v4l2rtspserver github](https://github.com/mpromonet/v4l2rtspserver).  But basically:

    sudo apt-get install cmake liblog4cpp5-dev libv4l-dev
    wget www.live555.com/liveMedia/public/live555-latest.tar.gz
    tar xfz live555-latest.tar.gz
    cd live/
    ./genMakefiles linux
    make CPPFLAGS=-DALLOW_RTSP_SERVER_PORT_REUSE=1
    sudo make install
    cd
    git clone https://github.com/mpromonet/v4l2rtspserver.git
    cd v4l2rtspserver/
    cmake .
    make
    sudo make install

Then you can run:

    v4l2rtspserver -F15 -H 720 -W1280 -P 8555 /dev/video0

The stream can now be viewed from `rtsp://raspberrypi_ip:8555/unicast` from VLC on another computer.

## Method 3: ffmpeg/avconv

Install avconv by `sudo apt-get install libav-tools`.

Don't bother trying to get ffmpeg from third-part apt source, it mostly likely would not work.  Also don't bother compile (it takes a long time) or cross-compile (hassle) ffmpeg.  avconv works for our purpose.

shell script:

```bash
avconv -f video4linux2 -framerate 15 -video_size 960x540 \
 -i /dev/video0 -an -f flv -y \
 rtmp://yourstreamserver/live/pi
```

Run it and view the stream on another computer with vlc: `rtmp://yourstreamserver/live/pi`.

`yourstreamserver` is the name or IP of your RTMP stream server, it can be set up with Nginx-rtmp, Wowza, or srs etc.

Using this method, the stream has longest delay, cpu load is pretty high.  I will try to reduce it when I get time.

## Method 4: gstreamer

Install gstreamer 1.0:

    sudo apt-get install gstreamer1.0-tools

Test we can stream with a MP4 video file:

```bash
gst-launch-1.0 -v filesrc location="video.mp4" \
 ! qtdemux ! video/x-h264 ! h264parse \
 ! flvmux ! rtmpsink \
 location="rtmp://yourstreamserver/live/pi"
```
After this works, we try to make it work with camera.  Unfortunately I couldn't get v4l2src to work with gstreamer to stream RTMP.

I turned to [gst-rpicamsrc](https://github.com/thaytan/gst-rpicamsrc).

Install dependencies:

    sudo apt-get install autoconf automake libtool 
    sudo apt-get install libgstreamer1.0-dev
    sudo apt-get install libgstreamer-plugins-base1.0-dev 
    sudo apt-get install libraspberrypi-dev

Build gst-rpicamsrc:

    git clone https://github.com/thaytan/gst-rpicamsrc.git
    cd gst-rpicamsrc/
    ./autogen.sh --prefix=/usr --libdir=/usr/lib/arm-linux-gnueabihf/
    make
    sudo make install
    sudo reboot

After reboot, create a script:

```bash
gst-launch-1.0 -v rpicamsrc \
 !'video/x-h264,width=1280,height=720,framerate=15/1' \
 ! h264parse ! flvmux \
 ! rtmpsink location="rtmp://yourstreamserver/live/pi"
```

The stream from this method has much lower delay compared with method 3.  It's still much slower than method 1 and 2.  This is because it send stream to a RTMP server, you request stream from the server, In method 1/2, the stream is sent directly to you from RPi.  The streams from method 3/4 can be viewed in a browser (using for example JWPlayer), while the streams from method 1/2 can't.
