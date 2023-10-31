# WSL命令

查看正在运行的wsl发行版

```shell
wsl --list
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

# WSL导出和导入

`--help` 命令

> ```
> --export <Distro> <FileName>
>  将分发导出为 tar 文件。
>  对于标准输出，文件名可以是 -。
> 
> --import <Distro> <InstallLocation> <FileName> [Options]
>  将指定的 tar 文件作为新分发导入。
>  文件名可以是 - 用于标准输入。
> 
>  选项:
>      --version <Version>
>          指定用于新分发的版本。
> ```

WSL导出

```shell
wsl --shutdown
wsl --export Ubuntu D:\WSL\package\ubuntu_export.tar	#将Ubuntu(子系统名称)导出为ubuntu_export.tar	 可以通过 wsl --list 命令查看子系统名称
wsl --unregister Ubuntu 				   					   
```

WSL导入

```shell
wsl --import Ubuntu D:\WSL D:\WSL\package\ubuntu_export.tar --version 2   #导入ubuntu_export.tar到D:\WSL作为Ubuntu
Ubuntu config --default-user janius	#设置默认用户(原系统里存在用户)
```

# WSL中命令的特殊处理
`tail -f` 无法实时更新，需要加上参数`—disable-inotify`
```shell
tail -f —disable-inotify xxxx.log
```


# 错误

已退出进程，代码为 4294967295

1. 以管理员身份运行windows terminal
2. 输入netsh winsock reset
3. 重新打开windows terminal
