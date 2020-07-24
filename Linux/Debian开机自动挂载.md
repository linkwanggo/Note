# Debian开机自动挂载

1. 打开文件

```bash
vim /etc/fstab
```

2. 修改文件

   在文件最后一行添加

   ```
   /dev/sda        /mnt/linkwanggo      ext4            defaults                                1 1
   ```

   

