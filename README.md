# gstreamer-command


### send mp4 video through rtp 

```
gst-launch-1.0 filesrc location=/Users/xiang/Movies/test2.mp4 ! qtdemux !  video/x-h264 ! h264parse ! rtph264pay config-interval=-1 ! udpsink  host=127.0.0.1 port=5000
```

### play rtp video 

```
gst-launch-1.0 -v udpsrc port=5000 address=127.0.0.1 caps="application/x-rtp,media=(string)video,clock-rate=(int)90000,encoding-name=(string)H264,payload=(int)96" ! rtph264depay ! decodebin ! videoconvert ! autovideosink
```

### video audio generate hls

```
gst-launch-1.0 videotestsrc ! x264enc ! muxer.  audiotestsrc ! avenc_ac3 ! muxer.  mpegtsmux name=muxer ! hlssink max-files=5
```


### audio video to flv file

```
gst-launch-1.0 -v flvmux name=mux ! filesink location=test.flv  audiotestsrc samplesperbuffer=44100 num-buffers=10 ! faac ! mux.  videotestsrc num-buffers=250 ! video/x-raw,framerate=25/1 ! x264enc ! mux.

```

### mux audio and videofile to mkv file 

```
gst-launch-1.0 -v filesrc location=/path/to/mp3 ! mpegaudioparse ! matroskamux name=mux ! filesink location=test.mkv  filesrc location=/path/to/theora.ogg ! oggdemux ! theoraparse ! mux.

```

### demux media file to audio and video 

```
gst-launch-1.0 filesrc location=test.mov ! qtdemux name=demux  demux.audio_0 ! queue ! decodebin ! audioconvert ! audioresample ! autoaudiosink   demux.video_0 ! queue ! decodebin ! videoconvert ! videoscale ! autovideosink
```


### demux rtmp stream to audio and video 

```
gst-launch-1.0 -v rtmpsrc location=rtmp://localhost/live/stream  ! flvdemux name=demux demux.audio ! queue ! decodebin ! audioconvert ! audioresample ! opusenc ! rtpopuspay timestamp-offset=0  ! udpsink  host=127.0.0.1 port=5000 demux.video! queue ! h264parse ! rtph264pay timestamp-offset=0 config-interval=-1  ! udpsink  host=127.0.0.1 port=5002
```
