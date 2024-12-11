# Red Team

基础环境信息

```bash
# system：Ubuntu 22.04 ARM64 parallels virtual machine
# uname:Linux ubuntu-linux-22-04-desktop 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 09:59:54 UTC 2024 aarch64 aarch64 aarch64 GNU/Linux
```

## 工具安装

安装手动安装powershell

```bash
apt install libunwind8
# 下载XX安装文件
# 使用dpkg命令执行安装
```

### dnscat2

该工具旨在通过DNS协议创建一个加密的命令和控制（C&C）通道，这是几乎每个网络的有效隧道。

#### dnscat2安装：

```bash
# 下载
git clone "https://github.com/iagox86/dnscat2" /opt/dnscat2
# 进入目录
cd /opt/dnscat2/client/
# 自动化编译
make
# 测试工具使用正常
./dnscat 
# 添加可全局执行的软连接
ln -s /opt/dnscat2/client/dnscat /usr/bin/dnscat
```

#### dnscat2使用：
