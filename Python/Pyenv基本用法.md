# Pyenv基本用法
> ### pyenv 是一个 python 版本管理工具
> ### 可以方便的在工作环境中安装、管理和切换不同版本的 python
***
- 更新python可安装列表
```bash
pyenv update
```
- 查询windows支持的python版本列表
```bash
- pyenv install -l
```
- 安装指定python版本
```bash
pyenv install 3.11.1
```
- 安装多个python版本
```bash
pyenv install 3.7.0 3.9.0 3.11.0 
```
- 设置一个python版本为全局版本
```bash
pyenv global 3.11.1
```
- 设置一个python版本为本地版本
```bash
pyenv local 3.11.1
```
- 查看正在使用的python及其路径
```bash
pyenv version
```
- 查看此系统上安装的所有python版本
```bash
pyenv versions
```
- 卸载python版本
```bash
pyenv uninstall 3.11.1
```