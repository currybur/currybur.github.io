---
layout: post
title:  "Using jupyternotebook on Server"
description: 启用jupyter并且转发到本地
date: 2020-10-30 14:55:37 +023
---

## 配置jupyter
### 首先为jupyter创建目录
```
mkdir jupyter
cd jupyter
mkdir root
```
### 设置用密码进入jupyter
使用下面的命令，创建一个密文的密码：
```
python -c "import IPython;print(IPython.lib.passwd())"
```
执行后需要输入并确认密码，然后程序会返回一个 'sha1:...' 的密文，我们接下来将会用到它。
### 修改配置文件
我们使用 `--generate-config` 来参数生成默认配置文件：
```
jupyter notebook --generate-config --allow-root
```
生成的配置文件在 ~/.jupyter/ 目录下。
然后在配置文件最下方加入以下配置：
```
c.NotebookApp.ip = '*'
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.password = u'刚才生成的密文(shal:...)'
c.ContentsManager.root_dir = '~/jupyter/root'
```
其中：
`c.NotebookApp.password` 请将上一步中密文填入此项，包括 shal: 部分。
## 远程登录设置
### 在本地使用ssh隧道连接进行本地端口转发

```
ssh -N -f -L localhost:8888:localhost:8888 remote_user@remote_host -p ****
```
其中，`-N` 表示不需要执行任何命令，仅仅做端口转发。`-f` 表示后台运行。`-L` 表示本地映射转发，选项值代表ssh客户端和服务端转发的端口，这里选取8888为本地jupyter监听端口。