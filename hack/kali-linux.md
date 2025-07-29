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
## 安装 Flatpak
```bash
# Flatpak 是一个用于 Linux 系统的通用软件打包和分发技术
# 搜索应用：flatpak search 应用名称
# 安装应用：flatpak install flathub 应用ID
# 已安装应用：flatpak list
# 运行应用：flatpak run 应用ID
# 更新flatpak应用：flatpak update
# 卸载flatpak应用：flatpak uninstall 应用ID
apt update
apt install -y flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
# 为 GNOME 软件安装 Flatpak 插件,之后就可以在软件中心 安装flatpak软件了 ,kali 应用中：software
apt install gnome-software-plugin-flatpak

```
主题支持：
```bash
# 如果你想让 flatpak 应用程序看起来与系统更一致，你可以强制它们使用你的本地主题：
mkdir -p ~/.themes
cp -a /usr/share/themes/* ~/.themes/
flatpak override --filesystem=~/.themes/
```
## 安装 snap
```bash
# Snap 是 Canonical 开发的另一种通用 Linux 软件打包格式，类似于 Flatpak
# 搜索应用：snap find 关键词
# 安装应用：snap install 应用名称
# 列出安装应用：snap list
# 运行应用：snap run 应用名称
# 更新所有应用：snap refresh
# 更新特定应用：snap refresh 应用名称
# 卸载应用：snap remove 应用名称
apt install -y snapd
# 允许开机启动服务：snapd snapd.apparmor
systemctl enable --now snapd apparmor
# 重启系统
```
## 安装tor浏览器
<span style="color:green">官方命令，提示找不到torbrowser-launcher</span>
```bash
sudo apt install -y tor torbrowser-launcher
# 第一次它将下载并安装 Tor 浏览器，包括签名验证
# 下次它将用于更新和启动 Tor 浏览器。
torbrowser-launcher
```
## 工具信息查询
### 本地工具信息查询
```bash
man 工具名称
工具名称 --help
```
### 在线信息查询
https://www.kali.org/tools/
## MetaSploit框架
根据 [Kali Linux 网络服务策略](https://www.kali.org/docs/policy/kali-linux-network-service-policy/)，默认情况下，没有网络服务（包括数据库服务_）_在启动时运行，因此需要采取几个步骤才能启动并运行 [Metasploit](https://www.metasploit.com/) 并支持数据库。
快速启动并运行所有内容：
```bash
sudo msfdb init
```
1. 查看msfdb的命令交互有哪些
```bash
msfdb       

Manage the metasploit framework database

You can use an specific port number for the
PostgreSQL connection setting the PGPORT variable
in the current shell.

Example: PGPORT=5433 msfdb init

  msfdb init     # start and initialize the database
  msfdb reinit   # delete and reinitialize the database
  msfdb delete   # delete database and stop using it
  msfdb start    # start the database
  msfdb stop     # stop the database
  msfdb status   # check service status
  msfdb run      # start the database and run msfconsole
```
2. 启动metasploit的postgresql
```bash
msfdb start
```
3. 检查是否正在监听 5432 端口，验证postgresql是否正在运行
```bash
┌──(root㉿vbox-kali)-[~]
└─# ss -ant 
┌──(root㉿vbox-kali)-[~]
└─# msfdb status
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; preset: disabled)
     Active: active (exited) since Tue 2025-07-29 02:33:10 CDT; 5min ago
 Invocation: 466ebb98d90d485cab4be32e0f0b9976
    Process: 23081 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 23081 (code=exited, status=0/SUCCESS)
   Mem peak: 1.7M
        CPU: 2ms

Jul 29 02:33:10 vbox-kali systemd[1]: Starting postgresql.service - PostgreSQL RDBMS...
Jul 29 02:33:10 vbox-kali systemd[1]: Finished postgresql.service - PostgreSQL RDBMS.

COMMAND    PID     USER FD   TYPE DEVICE SIZE/OFF NODE NAME
postgres 23047 postgres 6u  IPv6  56883      0t0  TCP localhost:5432 (LISTEN)
postgres 23047 postgres 7u  IPv4  56884      0t0  TCP localhost:5432 (LISTEN)

UID          PID    PPID  C STIME TTY      STAT   TIME CMD
postgres   23047       1  0 02:33 ?        Ss     0:00 /usr/lib/postgresql/17/bin/postgres -D /var/lib/postgresql/17/main -c config_file=/etc/postgresql/17/main/postgresql.conf

[+] Detected configuration file (/usr/share/metasploit-framework/config/database.yml)
```
4. 初始化 msf 的 postgresql数据库
```bash
sudo msfdb init

```
5. 启动msfconsole
```bash
msfconsole -q
```

# kali tools
## THC Hydra
Hydra 是一个并行登录破解程序，支持多种协议 攻击。它非常快速和灵活，并且很容易添加新模块。
hydra支持的协议：
```text
Cisco AAA, Cisco auth, Cisco enable, CVS, FTP, HTTP(S)-FORM-GET, HTTP(S)-FORM-POST, HTTP(S)-GET, HTTP(S)-HEAD, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MySQL, NNTP, Oracle Listener, Oracle SID, PC-Anywhere, PC-NFS, POP3, PostgreSQL, RDP, Rexec, Rlogin, Rsh, SIP, SMB(NT), SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP
```
安装hydra
```bash
apt install hydra
```
dpl4hydra 生成一个默认的密码列表文件
```
dpl4hydra -h
dpl4hydra refresh
```
.................

# 常见问题
## 由kali linux签名密钥过期导致的`apt`错误
[[GPG]] 密钥用于对存储库进行签名，以确保更新包时的真实性、完整性和信任度。每隔 2-3 年，Kali 团队要么延长用于签署 APT 存储库的 [[GPG]] 密钥的生命周期，要么用新密钥替换它。这可能会导致长时间未更新其 kali-archive-keyring 包的用户出现错误。错误将如下所示：
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