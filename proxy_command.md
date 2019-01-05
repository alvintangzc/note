# 命令行下使用socket代理
mac下shadowsocks默认http代理地址为 127.0.0.1 1807，根据实际情况修改。创建脚本
```
vim /usr/local/bin/proxy
```
输入内容：
```
#!/bin/bash
http_proxy=http://127.0.0.1:1807 https_proxy=http://127.0.0.1:1807 $*
```
修改权限：
```
chmod +x /usr/local/bin/proxy
```
实际需要使用的命令行程序，使用 **proxy** 执行即可：
```
proxy curl https://google.com
```

