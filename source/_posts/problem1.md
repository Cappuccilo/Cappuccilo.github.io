---
abbrlink: ''
categories:
- [前端, html]
cover: https://s2.loli.net/2024/03/05/MR5Qvkoiu2dODwW.jpg
date: '2024-01-29T09:36:55.800230+08:00'
excerpt: 这篇文章介绍了在开发前端html页面时，使用video标签引入本地MP4文件后，在chrome浏览器下可以正常播放，但在safari下无法播放的问题。作者经过查询资料后得知，safari浏览器只支持H.264编码的MP4格式，所以猜测可能是编码格式的原因。作者使用ffmpeg工具进行转码，将视频从hevc编码格式转换为h264编码格式，然后再进行部署和测试，在safari浏览器下视频可以正常播放了。文章还提到了可能的其他问题，如Safari对于文件流的请求需要包含请求头和响应头等。
tags:
- ffmpeg
- safari
- html
title: 记录一次safari浏览器无法正常播放html的video标签的问题
updated: '2024-03-05T11:47:54.647+08:00'
---
## 问题描述

开发一个前端html页面时，使用了video标签，引入了一个本地MP4文件。在chrome浏览器下可以正常展示，但在safari下无法播放。

## 解决方案

查询资料后得知，safari浏览器只支持H.264编码的MP4格式，猜测可能是这个原因。

用ffmpeg查看视频属性：

```shell
ffprobe .\video.mp4 -show_streams -select_streams v -print_format json
```

发现我的视频是hevc编码格式：

![https://s2.loli.net/2024/01/29/LNTDbwMazvn8Y9x.png](https://s2.loli.net/2024/01/29/LNTDbwMazvn8Y9x.png)

转码该视频：

```shell
ffmpeg -i ./input.mp4 -vcodec h264 output.mp4
```

转码完成后再进行部署，用safari打开网页，发现视频可以正常播放了。

## 其他可能的问题

Safari对于文件流的请求需要包含一个请求头Range，和一个响应头Content-Range，如果项目是通过Nginx代理，添加以下内容来支持Range返回：

```nginx
server {
    listen 80;
    location ~xxx{
        add_header Accept-Ranges bytes;
    }
}
```
