# gstreamer-command


### send mp4 video through rtp 

```
gst-launch-1.0 filesrc location=/Users/xiang/Movies/test2.mp4 ! qtdemux !  video/x-h264 ! h264parse ! rtph264pay config-interval=-1 ! udpsink  host=127.0.0.1 port=5000
```

### play rtp video 

```
gst-launch-1.0 -v udpsrc port=5000 address=127.0.0.1 caps="application/x-rtp,media=(string)video,clock-rate=(int)90000,encoding-name=(string)H264,payload=(int)96" ! rtph264depay ! decodebin ! videoconvert ! autovideosink
```
