---
tags: [other, ffmpeg]
categories: 
- other
- ffmpeg
title: ffmpeg
date: 2021-09-15 20:03:02
---

### 提取并转换音频格式
```bash
ffmpeg -i mavel4.mp4 -acodec copy -vn m.m4a
ffmpeg -i m.m4a m.mp3
```

