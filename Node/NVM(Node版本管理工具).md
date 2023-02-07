# NVM -- Node版本管理工具
>### [项目地址 https://github.com/nvm-sh/nvm/](https://github.com/nvm-sh/nvm/)
***
- ## Git安装nvm(git>v1.7.10)
```shell
cd ~
git clone https://github.com/nvm-sh/nvm.git .nvm
./.nvm/nvm.sh
```
***
- ## 配置环境变量
  *bash: ~/.bashrc*  
  *zsh: ~/.zshrc*  
  *ksh: ~/.profile*
```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```
***
- ## nvm用法
查看帮助
```shell
nvm --version
```
安装最新版node
```shell
nvm install node
```
安装指定版本node
```shell
nvm install [vertion]
```
卸载指定版本node
```shell
nvm uninstall [vertion]
```
列出可用版本
```shell
nvm ls-remote
```
设定默认版本
```shell
nvm alias default [vertion]
```
使用指定版本
```shell
nvm use [vertion]
```
获取可执行文件的安装路径
```shell
nvm which [vertion]
```