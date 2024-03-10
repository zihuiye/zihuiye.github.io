---
layout: post
title:  "play with opencv in python"
date:   2018-02-27 23:24:00 +0800
categories: 
---

# play with opencv in python

今天在高度摄像头的时候发现直接用cv2.VideoCapture()得出来的stream总是有100ms的delay，只有10fps左右。我记得在linux使用这个摄像头的时候是可以达到30帧的，所以一直想把这个问题搞定。可是百度google了很久也找不出个所以然，想用别的接口替代好像也没找到。最终只有求助OpenCV官方的文档。

在看这个类cv::VideoCapture Class Reference的时候，发现构造函数是有参数可以选的，这个参数是apiPreference=CAP_ANY，感觉就是这个了。点进去VideoCaptureAPIs发现有一大堆可以调用的驱动，用上了暴力解法，全部都试了一遍，发现只有两个是可以用的，分别是cv2.CAP_DSHOW和cv2.CAP_MSMF。cv2.CAP_MSMF这个就是默认调用的，因为它的delay和默认是一样的。用了cv2.CAP_MSMF则可以把FPS提高到15fps。这还是不够用，本来应该是30的。所以再找了一下，还有可以设置pfs的配置参数VideoCaptureProperties。直接用stream.set(cv2.CAP_PROP_FPS,30)就可以了。解决！
