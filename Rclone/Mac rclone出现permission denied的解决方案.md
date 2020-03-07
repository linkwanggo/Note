# Mac rclone出现permission denied的解决方案

## 1. 问题：rclone前必须加sudo

```
Failed to load config file "/Users/linkwanggo/.config/rclone/rclone.conf": open /Users/linkwanggo/.config/rclone/rclone.conf: permission denied
```

目的：不使用sudo执行rclone

## 2. 解决方案

```
sudo chown -R linkwanggo /Users/linkwanggo/.config/rclone
```

## ---END---