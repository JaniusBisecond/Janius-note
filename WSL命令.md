查看正在运行的wsl发行版

```shell
wsl --list --verbose
```

关闭指定wsl发行版

```shell
wsl -t name
```

关闭所有wsl发行版

```shell
wsl --shutdown
```

设置默认发行版

```shell
wsl -s name
```

# 错误

已退出进程，代码为 4294967295

1. 以管理员身份运行windows terminal
2. 输入netsh winsock reset
3. 重新打开windows terminal
