# 让Windows上Linux子系统更像Linux


## 前言

以前安装Linux子系统基本上是 `Debian`、`Ubuntu` 或者 `SUSE`，不过个人比较习惯 `RedHat` 系的Linux发行版，在 `WSL 1`
的时候从github找过 `CentOS` 安装过，感觉不怎么好用，不过工作中大多数时间都是用Mac，所以就体验了下就删了

现在升级 `WSL 2` 了，闲来无事折腾一下，虽然CentOS已经不再是当初的CentOS了，但是Linux的发行版又有了新的"CentOS"
，比如 `AlmaLinux`、`RockyLinux`

## 进入正题

**本次主角 `AlmaLinux`**

> 你问为什么选 `AlmaLinux` 不选 `RockyLinux` ？当然是因为 `AlmaLinux` 官方在Microsoft Store上架了App啦，一键安装不舒服嘛？

### 准备工作

1. 安装 `AlmaLinux 9`
2. 升级 `WSL 2`

### 开始折腾

为什么标题是"让Linux子系统更像Linux"？接下来一步一步的找出不像的点，再让它变得像Linux

#### 1. 启用systemd

wsl2本身是由Windows负责运行的，使用的是Windows提供的init启动系统，不是systemd

如果你使用systemctl命令，则会提示"系统不是以systemd启动，无法操作"
```shell
# jay @ Jay-PC in ~ [22:31:47]
$ systemctl
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```

执行pstree可以看到
```shell
# jay @ Jay-PC in ~ [22:24:18]
$ pstree
init(AlmaLinux9─┬─SessionLeader───Relay(9)───zsh───pstree
                ├─init───{init}
                └─{init(AlmaLinux9}
```

而现在大多数发行版都是使用的systemd，这就导致了很多服务都没法使用，接下来解决它，改成使用systemd

直接在子系统中执行以下命令
```shell
cat >> /etc/wsl.conf <<EOF
[boot]
systemd=true
EOF
```

然后切回Windows的Powershell执行
```shell
wsl --shutdown
```

然后在打开wsl，因为刚才shutdown了wsl，这会打开wsl会稍微等个2-3秒，再次执行pstree
```shell
# jay @ Jay-PC in ~ [22:43:37]
$ pstree
systemd─┬─dbus-broker-lau───dbus-broker
        ├─init-systemd(Al─┬─SessionLeader───Relay(50)───zsh───pstree
        │                 ├─init───{init}
        │                 ├─login───zsh
        │                 └─{init-systemd(Al}
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd
        ├─systemd───(sd-pam)
        ├─systemd-hostnam
        ├─systemd-journal
        └─systemd-logind
```

可以看到系统第一个进程显示为systemd了，再次执行systemctl可以看到正常显示所有服务了

#### 2. 禁止加载Windows环境变量

平常使用wsl敲命令的时候，想按Tab补全，总是出现一大堆的Windows命令，看了就头疼，解决他！

直接在子系统中执行以下命令
```shell
cat >> /etc/wsl.conf <<EOF
[interop]
appendWindowsPath=false
EOF
```

切回Windows的Powershell执行
```shell
wsl --shutdown
```

再次打开wsl，然后尝试使用Windows的命令，已经没有了！



---

> 作者: [Jay](https://github.com/Heelie)  
> URL: https://heelie.github.io/2023/230424_make_wsl_like_linux/  

