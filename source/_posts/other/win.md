---
tags: [other, win]
categories: 
- other
- Windows
title: win
---
### 找到占用某个端口的进程
```
netstat -ano # 列出所有端口

netstat -aon|findstr "443" # 查看对应端口对应的PID

tasklist|findstr "2720" # 根据PID找到对应的进程

taskkill /f /t /im Tencentdl.exe # 结束该进程
```