# 基础使用
## 更新软件
### 更新所有软件包
更新kali所有软件包
```bash
$ sudo apt update     
```
更新包列表
```bash
$ sudo apt full-upgrade 
```
### 更新指定软件包
更新单个特定包
```bash
$ sudo apt install --only-upgrade package_name
```
### kali源配置文件
源配置文件：`/etc/apt/sources.list`
默认配置信息如下：
```bash
┌──(user㉿vbox-kali)-[/root]
└─$ cat /etc/apt/sources.list
# See https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

# Additional line for source packages
# deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

## 元包
元包`metapackages`：用于一次安装多个包，创建为对其他包的依赖项列表。
建议在安装`metapackages`之前更新系统，命令如下：
```bash
$ sudo apt update
$ sudo apt full-upgrade -y
```
安装`metapackages`: `kali-linux-default`
```bash
$ sudo apt install kali-linux-default
```
元包的安装，还支持通过菜单的形式进行安装，通过如下命令进入：
```bash
$ kali-tweaks
```
执行命令后，会显示metapackages的选项卡，可以通过⬆️⬇️⬅️➡️和回车键进行选择和确认。
metapackages介绍：
```markdwon
# System
kali-linux-core: Kali Linux 基础系统 - 包含始终需要的基础组件  
kali-linux-headless: 无图形界面的默认安装版本  
kali-linux-default: "默认"桌面镜像包含的这些工具  
kali-linux-arm: 适用于ARM设备的所有工具  
kali-linux-nethunter: 作为Kali NetHunter组成部分的工具

# Desktop environments/Window managers
kali-desktop-core: GUI镜像所需的任何关键工具  
kali-desktop-e17: Enlightenment (窗口管理器)  
kali-desktop-gnome: GNOME (桌面环境)  
kali-desktop-i3: i3 (窗口管理器)  
kali-desktop-kde: KDE (桌面环境)  
kali-desktop-lxde: LXDE (窗口管理器)  
kali-desktop-mate: MATE (桌面环境)  
kali-desktop-xfce: Xfce (窗口管理器)

# Tools
kali-tools-gpu: 能利用GPU硬件加速的工具  
kali-tools-hardware: 硬件黑客工具  
kali-tools-crypto-stego: 密码学和隐写术相关工具  
kali-tools-fuzzing: 用于协议模糊测试  
kali-tools-802-11: 802.11协议(通常称为"Wi-Fi")工具  
kali-tools-bluetooth: 针对蓝牙设备的工具  
kali-tools-rfid: 射频识别(RFID)工具  
kali-tools-sdr: 软件定义无线电工具  
kali-tools-voip: 网络语音(VoIP)工具  
kali-tools-windows-resources: 可在Windows主机上执行的资源  
kali-linux-labs: 用于学习和实践的环境

# Menu
kali-tools-information-gathering: 开源情报(OSINT)和信息收集工具  
kali-tools-vulnerability: 漏洞评估工具  
kali-tools-web: 用于Web应用程序攻击的工具  
kali-tools-database: 数据库攻击相关工具  
kali-tools-passwords: 密码破解工具 - 在线和离线方式  
kali-tools-wireless: 无线协议工具集 - 802.11、蓝牙、RFID和SDR  
kali-tools-reverse-engineering: 二进制逆向工程工具  
kali-tools-exploitation: 常用漏洞利用工具  
kali-tools-social-engineering: 社会工程学技术工具  
kali-tools-sniffing-spoofing: 嗅探和欺骗工具  
kali-tools-post-exploitation: 后渗透阶段技术工具  
kali-tools-forensics: 取证工具 - 实时和离线分析  
kali-tools-reporting: 报告生成工具

# Others
kali-linux-large: 我们之前镜像的默认工具集  
kali-linux-everything: 包含此处列出的所有元包和工具  
kali-desktop-live: 从镜像启动时的实时会话使用
```
..................
..................
..................
# 常见问题
## 由kali linux签名密钥过期导致的`apt`错误
GPG 密钥用于对存储库进行签名，以确保更新包时的真实性、完整性和信任度。每隔 2-3 年，Kali 团队要么延长用于签署 APT 存储库的 GPG 密钥的生命周期，要么用新密钥替换它。这可能会导致长时间未更新其 kali-archive-keyring 包的用户出现错误。错误将如下所示：
```bash
┌──(root㉿vbox-kali)-[~]
└─# apt update              
Get:1 http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease [41.5 kB]
Err:1 http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease
  Sub-process /usr/bin/sqv returned an error code (1), error message is: Verifying signature:            Not live until 2025-07-28T06:24:11Z
Fetched 41.5 kB in 6s (6,393 B/s)
6 packages can be upgraded. Run 'apt list --upgradable' to see them.
Warning: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. OpenPGP signature verification failed: http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease: Sub-process /usr/bin/sqv returned an error code (1), error message is: Verifying signature:            Not live until 2025-07-28T06:24:11Z
Warning: Failed to fetch http://http.kali.org/kali/dists/kali-rolling/InRelease  Sub-process /usr/bin/sqv returned an error code (1), error message is: Verifying signature:            Not live until 2025-07-28T06:24:11Z
Warning: Some index files failed to download. They have been ignored, or old ones used instead.
```
预防问题：
为避免将来出现此问题，请执行以下作：
定期更新您的系统，尤其是` kali-archive-keyring` 包。
如果您的 Kali 安装已超过 2 年，则可能不再受支持。请考虑更新到最新版本以继续接收更新。
解决此问题的另一种方法是检索最新的密钥并将其存储在 apt 可以找到它的地方。
```bash
┌──(root㉿vbox-kali)-[~]
└─# sudo wget https://archive.kali.org/archive-keyring.gpg -O /usr/share/keyrings/kali-archive-keyring.gpg
```
****************