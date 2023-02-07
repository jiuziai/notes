# 关于Centos升级gcc版本

```bash
yum install centos-release-scl
yum install devtoolset-8-gcc*
scl enable devtoolset-8 bash
gcc -v
```
*此方法在/opt/rh中生成一个新版本的gcc库,且只在本次登陆生效,可将新版本的文件覆盖老版本文件使之永久生效*  
使用复制命令```cp```需要在```cp```之前加上反斜杠```\```取消覆盖提醒 
 - eg  
  ```bash
  \cp -rf /opt/rh/devtoolset-8/root/* /
  ```