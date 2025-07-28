# burp suite professional

[参考文章](https://blog.csdn.net/tl4832194/article/details/140872075)

# 安装

mac安装burp专业版

```bash
brew install --cask burp-suite-professional 
```

下载破解程序 [burploader.jar](./../tools_file/burploader.jar)  tools_file目录中有文件

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

# 使用

# 插件

## Param Miner

**简介：**

Param Miner，直译：参数矿工，参数挖掘机--是一个检查隐藏输入的插件。

**使用：**

1. 安装插件
2. 打开  Target > Site map 选择要测试的链接
3. 右键 Extensions > Param Miner
4. 选择隐藏输入的猜测类型
   1. Guess GET parameters （猜测GET参数）
   2. Guess cookie parameters（猜测cookie参数）
   3. Guess headers （猜测请求头）
   4. Guess everything（猜测一切）
5. 在 Attack Config 对话框中点击 OK ，Param Miner 将向目标发送一系列请求。
6. 查看运行日志和已识别的隐藏输入：Extensions > Installed > Param Miner > Output （`Issues`中也会显示识别到的问题，相关问题描述以 Secret input开头）
