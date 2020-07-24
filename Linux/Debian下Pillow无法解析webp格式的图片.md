# Debian下Pillow无法解析webp格式的图片

## 问题原因

debian下Pillow由于版本过高原因，导致Pillow无法正常解析webp

## 解决方案

安装`Pillow==2.9.0`版本

```
pip3 install `Pillow==2.9.0`
```

最好在安装上webp相关的库

```
apt install libwebp-dev/oldstable
apt install libwebp6/oldstable
apt install libwebpdemux2/oldstable
apt install libwebpmux2/oldstable
```

## ---END---

