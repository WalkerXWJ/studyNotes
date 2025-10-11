# burp suite professional

[参考文章](https://blog.csdn.net/tl4832194/article/details/140872075)

# 安装

mac安装burp专业版

```bash
brew install --cask burp-suite-professional 
```

下载启动程序 [burploader.jar](./../tools_file/burploader.jar) tools_file目录中有文件

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

## 官网的练习网站

网站1:
网站2:

## 评估输入

### 手动评估单个输入

1. 发送请求到Repeater

2. 切换到Repeater tab中，一次修改每个参数的value，如：
   
   1. 删除参数的value
   2. 输入一个空值
   3. 选中高亮输入，右键选择 Insert Collaborator payload

3. 点Send发送请求，并检查响应的差异，比如输入反馈和响应时间差异。

4. 比较时可以依次将响应发送到 Comparer 比较器中
   
   1. 选择要比较的两个响应
   
   2. 可以选择 Words 或 Bytes  进行比较，将在新窗口显示高亮标记的结果。
      
      ### 扫描单个输入

5. 可以在任何报文编辑器中，选中高亮value，如 Proxy > HTTP history

6. 右键，选择 Scan selected  insertion point ，scan launcher 窗口将打开。

7. 点击 Scan 将使用默认配置进行扫描。扫描记录可以在 Dashboard 窗口的 Tasks 列表中看到，扫描发现的问题的在 Issues tab 中。
   
   ### 扫描多个输入

8. 将请求发送到 Intruder 

9. 在 Intruder 中为参数添加payload的位置
   
   1. 选中高亮参数
   2. 点击 Add §，可以添加多个参数

10. 右键选择 Scan defined insertion point，scan launcher 窗口将打开。

11. 点击 Scan 将使用默认配置进行扫描。扫描记录可以在 Dashboard 窗口的 Tasks 列表中看到，扫描发现的问题的在 Issues tab 中。 
    
    ### 模糊测试输入

12. 将请求发送到 Intruder

13. 在 Intruder 中为参数添加 payload 的位置
    
    1. 选中高亮参数
    2. 点击 Add §，可以添加多个参数

14. 在 Payload Configuration 中添加模糊测试字符串列表，点击 add form list ，选择 Fuzzing - full 完整列表或添加其他列表。

15. 点击 start attack ，攻击测试将在新窗口开始，burp 将发送模糊测试的 payload 。

16. 攻击测试结束后通过通过查看响应发现漏洞问题。
    
    ### 分析难以理解的数据

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
   
   ## CloudX
   
   简介：
   cloudx，一个基于规则的加解密破签工具。
   项目地址：https://github.com/cloud-jie/CloudX
   使用：

7. github release中下载最新jar文件

8. Extensions > add > select file(.jar) > next

9. 