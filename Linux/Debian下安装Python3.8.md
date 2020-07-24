# Debian下安装Python3.8

[python全版本下载地址](https://www.python.org/ftp/python/)

[python3.8.3下载地址](https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tar.xz)

## 前置

```bash
apt update
apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
```

## 安装

```bash
wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tar.xz
tar -xvf Python-3.8.3.tar.xz
cd Python-3.8.3
./configure --enable-optimizations # --enable-optimizations选项将通过运行多个测试来优化Python二进制文件，这将使构建过程变慢。如果继续报错可以尝试去掉--enable-optimizations
make -j 4  # -j代表使用的核心数 可以通过键入nproc来找到它
make altinstall # 不要使用标准的make install，因为它会覆盖默认的系统python3二进制文件。
```

## 查看Python3.8

此时，Python 3.8已安装在你的Debian系统上并可以使用。

```bash
python3.8 --version  # 查看版本信息
```

如果想改为python3默认使用python3.8

```
rm /usr/bin/python3
ln -s /usr/local/bin/python3.8 /usr/bin/python3
```

默认pip使用pip3.8

```bash
sudo ln -s /usr/local/bin/pip3.8 /usr/bin/pip3
```

## 问题总结

1. 如果编译出现问题 请重试3次

2. pip3如果出现`subprocess.CalledProcessError: Command ‘(‘lsb_release’, ‘-a’)’ returned non-zero exit status 1.` 

   ```bash
   find / -name lsb_release
   rm -rf /usr/bin/lsb_release
   ```

   