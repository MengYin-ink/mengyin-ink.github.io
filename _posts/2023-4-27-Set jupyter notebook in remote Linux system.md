---
title:  "用Jupyter notebook连接远程服务器"
layout: post
---

## 1. 登陆远程服务器，创建conda环境
* 利用ssh登陆远程服务器
* 创建一个新的conda环境
```
conda create -n [name] python=[verson x.x]
```
* 激活这个conda环境
```
conda activate [name]
```
* 查看conda的环境
```
conda env list
```

## 2. 远程Linux服务器上：
* 利用Anaconda安装
```
conda install jupyter notebook
```
* 生成Jupyter notebook配置文件
```
jupyter notebook --generate-config
```
* 利用ipython配置Jupyter notebook密码
```
ipython
In [1]: from notebook.auth import password

In [2]: passwd()
Enter password:  ****(自定义)
Verify password: ****
Out[2]: 'argon2:xxxxx' #保存这个生成的密钥，后面配置文件的时候需要用到

In [3]: exit()
```
* 配置jupyter_notebook_config.py文件
```
vim ~/.jupyter/jupyter_notebook_config.py
```
  * 在最后一行加入下面信息：
  ```
  c.NotebookApp.ip = '*' # 允许访问此服务器的 IP，星号表示任意 IP
  c.NotebookApp.password = u'argon2:xxxxx # 之前生成的密码 hash 字串, 粘贴进去
  c.NotebookApp.open_browser = False # 运行时不打开本机浏览器
  c.NotebookApp.port = 8890 # 使用的端口，随意设置，不建议使用默认的8888，感觉经常会被占用
  c.NotebookApp.enable_mathjax = True # 启用 MathJax
  c.NotebookApp.allow_remote_access = True # 允许远程访问
  c.NotebookApp.notebook_dir = '/work/meng/DC1/sisl' # 设置默认目录
  ```
## 3. 在远程服务器启动Jupyter notebook
* 在远程服务器输入：
```
jupyter notebook
```

* 出现下面信息则表示在远程服务器上启动成功：
```
[I 15:46:50.372 NotebookApp] Serving notebooks from local directory: /work/meng/DC1/siesta
[I 15:46:50.372 NotebookApp] Jupyter Notebook 6.4.8 is running at:
[I 15:46:50.372 NotebookApp] http://super2:8890/
[I 15:46:50.373 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
```
## 4. 本地连接远程服务器
```
ssh -N -f -L localhost:8890:localhost:8890 [用户名]@[服务器地址]
```
* 检查本地端口是否被占用以及彻底kill的办法：
  * 在terminal中输入：
  ```
  lsof -i tcp:端口
  ```
  * 如果此端口被占用，则会输出相应的PID号，用一下指令kill：
  ```
  kill -9 PID号
  ```

## Reference
https://www.coonote.com/jupyter-note/jupyter-passwd.html
