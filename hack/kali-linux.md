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