title: Android 多媒体开发总结
date: 2016-04-20 11:27:52
tags:
---
### 1. [Android MediaCodec stuff](http://bigflake.com/mediacodec/#overview)
* create and configure the MediaCodec object
* loop until done:
if an input buffer is ready:
read a chunk of input, copy it into the buffer
if an output buffer is ready:
copy the output from the buffer
* release MediaCodec object

### 2. [Supported Media Formats](http://developer.android.com/intl/zh-cn/guide/appendix/media-formats.html)
parameters 	        | SD (Low quality)	  | SD (High quality)	     | HD 720p (N/A on all 
--------------------|---------------------|--------------------------|------------------------------
Video resolution    | 176 x 144 px	      | 480 x 360 px	         | 1280 x 720 px
Video frame rate	| 12 fps	          | 30 fps	                 | 30 fps
Video bitrate	    | 56 Kbps	          | 500 Kbps	             | 2 Mbps
Audio codec	        | AAC-LC	          | AAC-LC	                 | AAC-LC
Audio channels	    | 1 (mono)	          | 2 (stereo)	             | 2 (stereo)
Audio bitrate	    | 24 Kbps	          | 128 Kbps	             | 192 Kbps