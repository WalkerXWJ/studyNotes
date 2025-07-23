# msf-metasploit-framework
[MetaSploit官方文档](https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html)
## 安装
```bash
# 适用Linux和mac安装msf
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
```
## 使用
```bash
# 更新msf
msfupdate
# 第一次启动msf 后续启动直接msfcontrol
msfdb init
msfcontrol 
```
### 搜素模块
msf基于模块的概念，最常用的模块如下：
- auxiliary - 辅助模块不利用目标，但可以执行数据收集或管理任务
- exploit - 漏洞利用模块以允许框架在目标主机上执行任意代码的方式利用漏洞
- payloads - 可以在远程目标上执行以执行任务的任意代码，例如创建用户、打开 shell 等
- post - 在计算机遭到入侵后使用Post模块。它们执行有用的任务，例如从会话中收集、收集或枚举数据。

使用`search`搜索模块：
```bash
msf > search type:auxiliary http html title tag

Matching Modules
================

   #  Name                          Disclosure Date  Rank    Check  Description
   -  ----                          ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/title  .                normal  No     HTTP HTML Title Tag Content Grabber


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/http/title
```
