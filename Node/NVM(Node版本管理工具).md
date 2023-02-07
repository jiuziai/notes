# NVM -- Node版本管理工具
>### [项目地址 https://github.com/nvm-sh/nvm/](https://github.com/nvm-sh/nvm/)
***
- ## Git安装nvm(git>v1.7.10)
```bash
cd ~
git clone https://github.com/nvm-sh/nvm.git .nvm
./.nvm/nvm.sh
```
***
- ## 配置环境变量
添加用户环境变量
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```
使变量立即生效
```bash
# bash:
source ~/.bashrc
# zsh:
source ~/.zshrc
# ksh:
source ~/.profile
```
***
- ## nvm用法
查看帮助
```bash
nvm --version
```
安装最新版node
```bash
nvm install node
```
安装指定版本node
```bash
nvm install [vertion]
```
卸载指定版本node
```bash
nvm uninstall [vertion]
```
列出可用版本
```bash
nvm ls-remote
```
设定默认版本
```bash
nvm alias default [vertion]
```
使用指定版本
```bash
nvm use [vertion]
```
获取可执行文件的安装路径
```bash
nvm which [vertion]
```
设置node镜像
```bash
# 清华: https://mirrors.tuna.tsinghua.edu.cn/
# 华为: https://repo.huaweicloud.com/nodejs/
# 阿里: https://npmmirror.com/mirrors/node/
# 腾讯: https://mirrors.cloud.tencent.com/nodejs-release/
nvm node_mirror [url]
```
设置npm 镜像
```bash
# 华为: https://repo.huaweicloud.com/npm-software/
# 阿里: https://npmmirror.com/mirrors/npm/
# 腾讯: https://mirrors.cloud.tencent.com/npm/
nvm npm_mirror [url]
```
*其他源:*  
*清华:https://mirrors.tuna.tsinghua.edu.cn/*  
