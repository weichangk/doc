FFmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序。采用LGPL或[GPL](https://baike.baidu.com/item/GPL/2357903)许可证。它提供了录制、转换以及流化音视频的完整解决方案。



FFmpeg 官网  http://ffmpeg.org/ 

下载地址 https://github.com/BtbN/FFmpeg-Builds/releases

在 FFmpeg 官网可以下载对应平台的可执行程序包，bin 文件夹下能看到三个可执行程序：ffmpeg、ffplay、ffprobe，配置好环境变量后即可使用。 



ffmpeg：Hyper fast Audio and Video encoder 超快音视频编码器

ffplay：Simple media player 简单媒体播放器

ffprobe：Simple multimedia streams analyzer 简单多媒体流分析器



查询命令帮助文档

基本信息：ffmpeg -h

高级信息：ffmpeg -long

所有信息：ffmpeg -full

输出文件：ffmpeg -h > ffmpeg_h.log



FFMPEG 从功能上划分为几个模块，分别为核心工具（libutils）、媒体格式（libavformat）、编解码（libavcodec）、设备（libavdevice）和后处理（libavfilter,libswscale, libpostproc），分别负责提供公用的功能函数、实现多媒体文件的读包和写包、完成音视频的编解码、管理音视频设备的操作以及进行音视频后处理。













