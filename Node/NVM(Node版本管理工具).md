# NVM -- Node版本管理工具
> *一个随时任意切换管理多版本node的工具*  
> [*项目地址(linux版)*](https://github.com/nvm-sh/nvm)  
> [*项目地址(windows版)*](https://github.com/coreybutler/nvm-windows) 

## Linux
### 安装
```bash
cd ~
git clone https://github.com/nvm-sh/nvm.git .nvm
./.nvm/nvm.sh
```

### 配置
*在用户配置文件中添加环境变量*
- *bash*
```bash
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.bashrc
source ~/.bashrc
```
- *zsh*
```bash
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.zshrc
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.zshrc
source ~/.zshrc
```

- *ksh*
```bash
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.profile
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.profile
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.profile
source ~/.profile
```


## Windows
Windows版有安装版和不安装版本，不安装板本包含一键设置脚本，无需特别配置。
## 基本用法
*查看帮助*
```bash
nvm --version
```
*安装最新版node*
```bash
nvm install node
```
*安装指定版本node*
```bash
nvm install [vertion]
```
*卸载指定版本node*
```bash
nvm uninstall [vertion]
```
*列出可用版本*
```bash
nvm ls-remote
```
*设定默认版本*
```bash
nvm alias default [vertion]
```
*使用指定版本*
```bash
nvm use [vertion]
```
*获取可执行文件的安装路径*
```bash
nvm which [vertion]
```

## 国内镜像源
*设置node镜像*
```bash
# 清华: https://mirrors.tuna.tsinghua.edu.cn/
# 华为: https://repo.huaweicloud.com/nodejs/
# 阿里: https://npmmirror.com/mirrors/node/
# 腾讯: https://mirrors.cloud.tencent.com/nodejs-release/
nvm node_mirror [url]
```
*设置npm 镜像*
```bash
# 华为: https://repo.huaweicloud.com/npm-software/
# 阿里: https://npmmirror.com/mirrors/npm/
# 腾讯: https://mirrors.cloud.tencent.com/npm/
nvm npm_mirror [url]
```