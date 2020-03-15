# ffmpeg打开文件错误 Unsafe file name

加入 -safe 0

```
ffmpeg -f concat -safe 0 -i {0} -acodec copy -vcodec copy -absf aac_adtstoasc {1}
```

## ---END---

