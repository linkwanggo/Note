# ffmpeg拼接视频文件的方法

## 1. 推荐：文件读取法

使用方法：将所有视频分片的绝对路径都写入特定的文件（filelist.txt）

格式：

```
file 'input1.mkv'
file 'input2.mkv'
file 'input3.mkv'
```

然后调用ffmpeg合并

简单合并命令

```
ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mkv
```

我正在用的一个命令

```
ffmpeg -f concat -safe 0 -i filelist.txt -acodec copy -vcodec copy -absf aac_adtstoasc output.mkv
```

## ---END---