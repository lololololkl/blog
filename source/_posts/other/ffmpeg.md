---
tags: [other, ffmpeg]
categories: 
- other
- ffmpeg
title: ffmpeg
---

### 提取并转换音频格式
```bash
ffmpeg -i mavel4.mp4 -acodec copy -vn m.m4a
ffmpeg -i m.m4a m.mp3
```

