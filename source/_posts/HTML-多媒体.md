---
title: HTML多媒体
date: 2017-02-04
tags: HTML
author: 映雪
---

多少红颜悴，多少相思碎，唯留血染墨香哭乱冢。

<!--more-->

### HTML 多媒体

- Web 上的多媒体指的是音效、音乐、视频和动画。
- 现代网络浏览器已支持很多多媒体格式。

### HTML 视频(Video)

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogg" type="video/ogg" />
</video>
```

### HTML 音频(Audio)

```html
<audio controls>
  <source src="horse.mp3" type="audio/mpeg" />
  <source src="horse.ogg" type="audio/ogg" />
</audio>
```

### HTML 音频/视频方法

|      方法      |                   描述                    |
| :------------: | :---------------------------------------: |
| addTextTrack() |       向音频/视频添加新的文本轨道。       |
| canPlayType()  | 检测浏览器是否能播放指定的音频/视频类型。 |
|     load()     |          重新加载音频/视频元素。          |
|     play()     |            开始播放音频/视频。            |
|    pause()     |         暂停当前播放的音频/视频。         |

### HTML 音频/视频属性

|        属性         |                             描述                             |
| :-----------------: | :----------------------------------------------------------: |
|     audioTracks     |         返回表示可用音频轨道的 AudioTrackList 对象。         |
|      autoplay       |        设置或返回是否在加载完成后随即播放音频/视频。         |
|      buffered       |       返回表示音频/视频已缓冲部分的 TimeRanges 对象。        |
|     controller      |   返回表示音频/视频当前媒体控制器的 MediaController 对象。   |
|      controls       |     设置或返回音频/视频是否显示控件（比如播放/暂停等）。     |
|     crossOrigin     |              设置或返回音频/视频的 CORS 设置。               |
|     currentSrc      |                  返回当前音频/视频的 URL。                   |
|     currentTime     |       设置或返回音频/视频中的当前播放位置（以秒计）。        |
|    defaultMuted     |              设置或返回音频/视频默认是否静音。               |
| defaultPlaybackRate |             设置或返回音频/视频的默认播放速度。              |
|      duration       |             返回当前音频/视频的长度（以秒计）。              |
|        ended        |               返回音频/视频的播放是否已结束。                |
|        error        |        返回表示音频/视频错误状态的 MediaError 对象。         |
|        loop         |         设置或返回音频/视频是否应在结束时重新播放。          |
|     mediaGroup      | 设置或返回音频/视频所属的组合（用于连接多个音频/视频元素）。 |
|        muted        |                设置或返回音频/视频是否静音。                 |
|    networkState     |                返回音频/视频的当前网络状态。                 |
|       paused        |                设置或返回音频/视频是否暂停。                 |
|    playbackRate     |               设置或返回音频/视频播放的速度。                |

### HTML 音频/视频事件

|      事件      |                        描述                        |
| :------------: | :------------------------------------------------: |
|     abort      |          当音频/视频的加载已放弃时触发。           |
|    canplay     |       当浏览器可以开始播放音频/视频时触发。        |
| canplaythrough | 当浏览器可在不因缓冲而停顿的情况下进行播放时触发。 |
| durationchange |          当音频/视频的时长已更改时触发。           |
|    emptied     |            当目前的播放列表为空时触发。            |
|     ended      |           当目前的播放列表已结束时触发。           |
|     error      |       当在音频/视频加载期间发生错误时触发。        |
|   loadeddata   |      当浏览器已加载音频/视频的当前帧时触发。       |
| loadedmetadata |      当浏览器已加载音频/视频的元数据时触发。       |
|   loadstart    |         当浏览器开始查找音频/视频时触发。          |
|     pause      |             当音频/视频已暂停时触发。              |
|      play      |        当音频/视频已开始或不再暂停时触发。         |
|    playing     |  当音频/视频在因缓冲而暂停或停止后已就绪时触发。   |
