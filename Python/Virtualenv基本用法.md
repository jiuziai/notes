# Virtualenv基本用法
> virtualenv 是一个Python虚拟环境管理工具，各虚拟环彼此隔离，各自的操作不会相互影响，有效防止多 python 版本导致项目或者开发环境崩溃等问题的发生  

---  
- 创建虚拟环境
```bash
virtualenv 虚拟环境路径
```
- 创建指定Python版本的虚拟环境
```bash
virtualenv -p /usr/bin/python2.7 虚拟环境路径
```
- 激活虚拟环境
```bash
source 虚拟环境路径/bin/activate
```
- 退出当前虚拟环境
```bash
deactivate 
```
- 删除虚拟环境
```bash
rm 虚拟环境路径 -r
```
