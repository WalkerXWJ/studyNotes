# python 官网文档学习
## [venv虚拟环境创建](https://docs.python.org/zh-cn/3.13/library/venv.html#module-venv)
venv是创建虚拟环境的标准工具，从python 3.3 开始成为python的组成部分，从python 3.4 开始venv会默认安装pip到所有创建的全部虚拟环境。

# book-python黑帽子
env：kali linux
## python环境
### 更新python
更新python到最新的python3
```bash
apt upgrade python3
```
### 创建虚拟环境

虚拟环境：
其实就是一个文件夹📁，里面存放了完整的python软件包和安装的第三方包。可以把不同需求的软件包隔离开来，每个环境都有自己的一套模块和依赖关系，不会干扰其他项目的依赖管理。

安装python3-venv
```bash
sudo apt install python-venv
```
创建虚拟环境
```bash
mkdir bhp
cd bhp
# 创建名称为venv3的python虚拟环境
# -m 参数选项来调用venv包
# venv3 要创建的环境名
python -m  venv venv3
# 启动名称为venv3的虚拟环境
source  venv3/bin/activate
```
### 激活环境后搜索和安装软件包
```bash
# 搜索包 search方法于2020年12月14日出现搜索滥用的过大流量，2022年1月3日因滥用情况未缓解XMLRPC搜索已被永久禁用
pip search hashcarck
# ⬆️ 不可用了
```
安装lxml库
```bash
pip install lxml
```
验证lxml库是否安装好了
```bash
# 进入python shell 导入包未报错 表示软件包已安装
python
>>> from lxml import etree
>>> exit()
```
## 安装ide工具
常见的Interrated Development Environment集成开发环境工具，一般包含代码编辑器（语法高亮、自动检查错误）和调试器。
IDE工具
- PyCharm
- Visual studio code
- WingIDE
文本编辑工具
- vim
- nano
- notepad
- emacs
```bash
# kali安装vscode
apt install code-oss
```
## 保持代码整洁
代码开发遵循python社区格式规范PEP 8.
文章链接：[https://peps.pythonlang.cn/pep-0008/](https://peps.pythonlang.cn/pep-0008/)
