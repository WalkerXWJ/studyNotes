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
指定当前活动的模块`use`,指定从服务器获取HTTP标题（网页的title）的模块
```bash
msf > use auxiliary/scanner/http/title 
msf auxiliary(scanner/http/title) > 
```
###运行服务模块
每个模块都提供可配置的选项，使用`show options`或`options`查看选项：
```bash
msf auxiliary(scanner/http/title) > show options

Module options (auxiliary/scanner/http/title):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   Proxies                       no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxies: socks5, socks5h, sapni, http, socks4
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT        80               yes       The target port (TCP)
   SHOW_TITLES  true             yes       Show the titles on the console as they are grabbed
   SSL          false            no        Negotiate SSL/TLS for outgoing connections
   STORE_NOTES  true             yes       Store the captured information in notes. Use "notes -t http.title" to view
   TARGETURI    /                yes       The base path
   THREADS      1                yes       The number of concurrent threads (max one per host)
   VHOST                         no        HTTP server virtual host


View the full module info with the info, or info -d command.

msf auxiliary(scanner/http/title) > 
```
设置模块选项，使用`set`，eg：`set command rhost`
```bash
msf auxiliary(scanner/http/title) > set rhosts www.jd.com
rhosts => www.jd.com
```
`run`命令，运营模块，显示目标的title。
```bash
msf auxiliary(scanner/http/title) > run
[+] [116.78.120.73:80] [C:302] [R:https://www.jd.com/] [S:nginx] 302 Found
[*] Scanned 1 of 2 hosts (50% complete)
[*] Scanned 2 of 2 hosts (100% complete)
[*] Auxiliary module execution completed
```
metasploit6新增功能，增加了对运行模块的支持，并将选项设置为`run`命令的一部分：
```bash
msf auxiliary(scanner/http/title) > run rhosts=www.jd.com httptrace=true
####################
# Request:
####################
GET / HTTP/1.1
Host: www.jd.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 14_7_2) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.4.1 Safari/605.1.15


####################
# Response:
####################
HTTP/1.1 302 Moved Temporarily
Server: nginx
Date: Wed, 23 Jul 2025 07:00:43 GMT
Content-Type: text/html
Content-Length: 138
Connection: keep-alive
Location: https://www.jd.com/
Timing-Allow-Origin: *
X-Trace: 302-1753254043649-0-0-0-0-0
Strict-Transport-Security: max-age=3600

<html>
<head><title>302 Found</title></head>
<body>
<center><h1>302 Found</h1></center>
<hr><center>nginx</center>
</body>
</html>

[+] [116.78.120.73:80] [C:302] [R:https://www.jd.com/] [S:nginx] 302 Found
[*] Scanned 1 of 2 hosts (50% complete)
####################
# Request:
####################
GET / HTTP/1.1
Host: www.jd.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 14_7_2) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.4.1 Safari/605.1.15


[*] Scanned 2 of 2 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(scanner/http/title) > 
```
