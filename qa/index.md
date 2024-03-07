# QA


  <!--more-->

## 2024.3.7

### Mac系统安装PicGo时打开报错：文件已损坏，您应该将它移到废纸篓

**解决办法**

- 打开终端输入以下内容，“为安装路径，默认是以下路径”

```sh
sudo xattr -d com.apple.quarantine "/Applications/PicGo.app"
1
```

- 按照提示输入电脑锁屏密码回车即可

