---
title: '配置自己的 Linux 云服务器'
date: 2019-06-21T15:30:40+08:00
lastmod: 2019-06-21T15:30:40+08:00
tags: ['提效工具', '服务配置']
categories: ['Notes']
authors:
  - '潘峰'
---

> 近期趁着 6.18 促销新采购了一台阿里云的 Linux 服务器，把配置过程记录如下

## 采购云服务器

采购平台：阿里云 腾讯云 百度云  
Linux 系统：Ubuntu

## 配置云服务器

### 登录 Linux 远程服务器

在本地终端使用 `ssh` 命令连接服务器并输入密码登录（以下使用 <hostname> 代指服务器 ip）

```bash
$ ssh root@<hostname>
```

### 配置个性化警告语

修改 Linux 系统远程登录欢迎语（`/etc/motd` 文件记录了用户登录后警告信息，并且不区分本地和远程）

```bash
# vim /etc/motd
```

复制以下内容，或使用其它更加[有趣的代码注释](https://blog.csdn.net/ydk888888/article/details/81563608)

```text



                  ___====-_  _-====___
            _--^^^#####//      \\#####^^^--_
        _-^##########// (    ) \\##########^-_
        -############//  |\^^/|  \\############-
      _/############//   (@::@)   \\############\_
    /#############((     \\//     ))#############\
    -###############\\    (oo)    //###############-
  -#################\\  / VV \  //#################-
  -###################\\/      \//###################-
_#/|##########/\######(   /\   )######/\##########|\#_
|/ |#/\#/\#/\/  \#/\##\  |  |  /##/\#/  \/\#/\#/\#| \|
`  |/  V  V  `   V  \#\| |  | |/#/  V   '  V  V  \|  '
    `   `  `      `   / | |  | | \   '      '  '   '
                    (  | |  | |  )
                    __\ | |  | | /__
                  (vvv(VVV)(VVV)vvv)
                      如未被授权
                      请您即刻退出！



```

修改 Ubuntu 系统的默认欢迎语
注释掉 `10-help-text` 文件中的相关信息（如果有）

```shell
# vim /etc/update-motd.d/10-help-text
```

```bash
#printf "\n"
#printf " * Documentation:  https://help.ubuntu.com\n"
#printf " * Management:     https://landscape.canonical.com\n"
#printf " * Support:        https://ubuntu.com/advantage\n"
```

注释掉 `80-livepatch` 文件中的相关信息（如果有）

```shell
# vim /etc/update-motd.d/80-livepatch
```

```bash
#case "$livepatch_status" in
#    "disabled (not available)")
#        # do nothing
#        ;;
#    "enabled")
#        echo
#        echo " * Canonical Livepatch is enabled."
#        print_status "${check_state}" "${patch_state}"
#        ;;
#    "disabled")
#        echo
#        echo " * Canonical Livepatch is available for installation."
#        echo "   - Reduce system reboots and improve kernel security. Activate at:"
#        echo "     https://ubuntu.com/livepatch"
#        ;;
#    *)
#        echo
#        echo " * Canonical Livepatch is in an unknown state."
#        echo "   - Please see /var/log/syslog for more information."
#        echo "     Status: \"$livepatch_status\""
#        ;;
#esac
```

### 配置用户

新建用户（以下使用 <username> 代指实际用户名）

```shell
# useradd -r -m -s /bin/bash <username>
```

设置用户密码

```shell
# passwd <username>
```

检查用户是否创建成功

```shell
# cat /etc/passwd
```

给用户配置免密码 `sudo` 权限

```shell
# vim /etc/sudoers
```

将文件中下面的这一段

```text
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

修改为：

```text
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL) NOPASSWD:ALL
```

然后将新建的 <username> 用户加入 sudo 用户组

```shell
# usermod -a -G sudo <username>
```

配置命令提示符 hostname 的显示

```shell
# vim /etc/hostname
```

将内容改成自己想要的如 ali-ecs
重启服务器

```shell
# reboot
```

### 配置 SSH Key 方式登录 Linux 远程服务器

登录新建的 <username> 用户

```shell
# ssh <username>@<hostname>
```

输入之前设置的密码进行登录
创建 `.ssh` 文件夹和 `authorized_keys` 并授予权限

```shell
$ mkdir ~/.ssh && touch ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

然后将本地机器 `id_rsa.pub` 中的文本复制添加到服务器的 `authorized_keys` 文件里
下次登录则无需输入密码
另外还可通过在本地设置 .ssh 目录下配置 config 文件，通过以下命令直接登录

```shell
$ ssh <username>
```
