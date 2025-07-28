# burp suite professional

[参考文章](https://blog.csdn.net/tl4832194/article/details/140872075)

mac安装burp专业版

```bash
brew install --cask burp-suite-professional 
```

下载破解程序 [burploader.jar](./../tools_file/burploader.jar) tools_file目录中有文件

```bash
# 百度网盘链接 https://pan.baidu.com/s/10hY7f0RGkupblCvBDlopVQ?pwd=2024
```

1. 访达打开`/Applications/`
2. 右键选中`brup suite professional.app`
3. 选择显示包内容
4. 进入`'/Applications/Burp Suite Professional.app/Contents/Resources/app'`
5. 将 burploader.jar 复制进去

打开mac自动化操作

1. 选择应用程序
2. 添加 运行shell程序
3. 添加如下脚本内容

```bash
cd '/Applications/Burp Suite Professional.app/Contents/Resources/app'
java -jar burploader.jar
```

4. 保存自动化程序至`/Applications/`下，命名为burp-pro-loader.app
5. 启动台打开burp-pro-loader.app，窗口中点击`run`，burp pro就启动了。
