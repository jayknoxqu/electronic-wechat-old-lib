# 解决Liunx下微信electronic-wechat启动报错 Harfbuzz version too old (1.0.6)
## 问题描述
在`Fedora 30`下运行正常，但升级到`Fedora 32` 后微信启时报错，错误信息如下:
```
[root@localhost ~]# ./electronic-wechat

(electronic-wechat:64447): Pango-ERROR **: 13:20:08.369: Harfbuzz version too old (1.0.6)

[1]    64447 trace trap (core dumped)  ./electronic-wechat
```

## 问题根源
**`Harfbuzz version too old (1.0.6)`**
系统升级后，微信依赖的库，低于系统内置的库，导致微信启动失败。


## 解决问题

#### 下载
搜索`Fedora 30 for x86_64`内置的依赖版本

[http://rpmfind.net/linux/rpm2html/search.php?query=harfbuzz](http://rpmfind.net/linux/rpm2html/search.php?query=harfbuzz)

[http://rpmfind.net/linux/rpm2html/search.php?query=harfbuzz-icu](http://rpmfind.net/linux/rpm2html/search.php?query=harfbuzz-icu)

[http://rpmfind.net/linux/rpm2html/search.php?query=pango](http://rpmfind.net/linux/rpm2html/search.php?query=pango)

下载`Fedora 30 for x86_64`内置的依赖版本
```
-rw-r--r--. 1 jayknoxqu jayknoxqu 535K May 29 14:38 harfbuzz-2.3.1-1.fc30.x86_64.rpm
-rw-r--r--. 1 jayknoxqu jayknoxqu  16K May 29 14:48 harfbuzz-icu-2.3.1-1.fc30.x86_64.rpm
-rw-r--r--. 1 jayknoxqu jayknoxqu 261K May 29 14:51 pango-1.43.0-4.fc30.x86_64.rpm
```

命令解压或者右键提取
```bash
[root@localhost ~]# rpm2cpio harfbuzz-2.3.1-1.fc30.x86_64.rpm | cpio -div
[root@localhost ~]# rpm2cpio harfbuzz-icu-2.3.1-1.fc30.x86_64.rpm | cpio -div
[root@localhost ~]# rpm2cpio pango-1.43.0-4.fc30.x86_64.rpm | cpio -div
```

在微信的安装目录`/opt/wechat`下，新建一个`lib`文件夹，把解压出来的`usr`文件夹中`lib64`的所有文件，复制到刚才新建的`lib`文件夹中
```
[root@localhost ~]# tree /opt/wechat/lib

/opt/wechat/lib
├── girepository-1.0
│   ├── Pango-1.0.typelib
│   ├── PangoCairo-1.0.typelib
│   ├── PangoFT2-1.0.typelib
│   └── PangoXft-1.0.typelib
├── libharfbuzz-icu.so.0 -> libharfbuzz-icu.so.0.20301.0
├── libharfbuzz-icu.so.0.20301.0
├── libharfbuzz.so.0 -> libharfbuzz.so.0.20301.0
├── libharfbuzz.so.0.20301.0
├── libharfbuzz-subset.so.0 -> libharfbuzz-subset.so.0.20301.0
├── libharfbuzz-subset.so.0.20301.0
├── libpango-1.0.so.0 -> libpango-1.0.so.0.4300.0
├── libpango-1.0.so.0.4300.0
├── libpangocairo-1.0.so.0 -> libpangocairo-1.0.so.0.4300.0
├── libpangocairo-1.0.so.0.4300.0
├── libpangoft2-1.0.so.0 -> libpangoft2-1.0.so.0.4300.0
├── libpangoft2-1.0.so.0.4300.0
├── libpangoxft-1.0.so.0 -> libpangoxft-1.0.so.0.4300.0
└── libpangoxft-1.0.so.0.4300.0

1 directory, 18 files

```
