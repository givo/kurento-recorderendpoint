# RtpEndpoint -> RecorderEndpoint Example

## Introduction

This is a `Node.js` example which creates a `RtpEndpoint` to `RecorderEndpoint` and to `WebRtcEndpoint` pipeline in `Kurento` media server. 

In this example a we used `Haivision Makito` to capture a monitor and watching the live stream on a simple web page.

We managed to successfuly connect `Kurento` to Maktio using `Direct RTP` and `QuickTime`.

`Kureto` is deployed on Ubuntu 14.04 virtual machine.

## Recording

In order to record, you have to:

* Have write permission to the directory you chose to write to:

```
sudo chmod 777 <directory>`
```

or

```
sudo chmod kurento:kurento <directory>` then `sudo chmod u+w,g+w <directory>
```

* Make sure to call `recorderEndpoint.record()` only when you are sure video has started. Use `MediaFlowInStateChange`.
 
* Provide a proper `recordingParams` which match the actual stream properties on `RecorderEndpoint` creation and a proper connection type when connecting the recorder endpoint, for example:
 
 * If the stream contains only video track - use `{ mediaProfile: 'MP4_ONLY_VIDEO' }` and `VIDEO` parameter when connecting the `RecorderEndpoint`

 * If the stream contains only audio track - use `{ mediaProfile: 'MP4_ONLY_AUDIO' }` and `AUDIO` parameter when connecting the `RecorderEndpoint`

* In order to record on a a shared directory (NAS), mount the share on the ubuntu machine using:

 * Install `cifs-utils`
 * `mount -t cifs -o username=<nas_user>,password=<nas_user_password> //<nas>/<share> /media/<share>`

## Improtant Notes
 
* In order for this example to work you need to configure, in runtime, (just after receiving the sdp answer) to use the udp port which `kurento` accepts in the sdp answer.

* Your `Ubuntu` machine which runs `Kurento` have to have `openh264` and package from Cisco installed.

## Codec Transcoding

By default `Kurento` use VP8 as the `WebRtc` codec, that's why Kurento will transcode any `h.264` stream before sinking to a WebRtcEndpoint.

* In order to disable transcoding to VP8 at Kurento, request a h264 rtp profile in client side by editing the local sdp.
* You can implement that by removing all `rtpmap` lines which are different then 96 and leave only `a=rtpmap:96 H264/90000` line.

### 'openh265' offline installation instructions:

 * Copy `libopenh264-1.4.0-linux64.so.bz2` (or newer) to the root `/` directory and add permissions (`chmod 777`)
 * Then install the following packages: (`openh264_1.4.0.trusty.20170725133438.e20ebe9_amd64-fixed.deb` is the openh264 with customization for offline installation)
 
```
sudo dpkg -i openh264_1.4.0.trusty.20170725133438.e20ebe9_amd64-fixed.deb

sudo dpkg -i openh264-gst-plugins-bad-1.5_1.8.1.1~20160909144557.99.gf836e53.trusty_amd64.deb
```

## Haivision Makito

![directRtp](/uploads/d5dca4a1d08e2fcc6ddac1150f18f34c/directRtp.png)

## Required Node.js Packages:

* async
* express
* express-session
* ws
* kurento-client
