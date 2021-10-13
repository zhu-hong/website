---
title: "ffmpeg常用命令"
date: 2021-04-09T09:25:13+08:00
draft: true
tags: [ffmpeg]
---

## 转码

```
ffmpeg -i input.flv output.mp4
ffmpeg -i input.flv -qscale 0 output.mp4 #保持原品质
```

## 提取音频

```
ffmpeg -i input.mp4 output.mp3
ffmpeg -i input.mp4 -q:a 0 -map a outpot.mp3
ffmpeg -i input.mp4 -vn -c:a copy output.aac
# -vn 不要视频 # -c:a(acodec) 音频解码
```

## 移除音频

```
ffmpeg -i input.mp4 -c:v copy -an output.mp4
# -an 不要音频 # -c:v(vcodec) 视频解码
```

## 拼接视频

```
ffmpeg -f concat (-safe 0) -i filelist.txt -c copy output.mp4
ffmpeg -i "concat:input1.mp4|input2.mp4" -c copy outpoot.mp4
# -c(codec) 指定输入输出解码编码器
```

## 截取视频

```
ffmpeg -i input.mp4 -ss 00:00:50 -t 50 -c copy output.mp4
ffmpeg -i input.mp4 -ss 00:00:50 -to 00:00:50.30 -c copy output.mp4
# -ss 开始时间 -t 持续时间 -to 停止时间
```

## 截取GIF

```
ffmpeg -i input.mp4
	   -ss 00:00:20 
	   -to 10
       -r 10
       -vf scale=200:-1 output.gif
# -r 帧率 -vf 缩放
```

