---
title: 'Python 处理系统文件'
date: 2019-01-04T11:30:02+08:00
lastmod: 2020-10-26T11:30:02+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 处理系统文件

## 内置方法

### open()

python open() 函数用于打开一个文件，创建一个 file 对象<br/>
语法：`open(name[, mode[, buffering]])`<br/>
name : 一个包含了你要访问的文件名称的字符串值<br/>
mode : mode 决定了打开文件的模式，这个参数是非强制的，默认文件访问模式为只读(r)<br/>
不同模式打开文件的完全列表：http://www.runoob.com/python/python-func-open.html

---

## os 库 --- 操作系统接口模块

### os.name

输出字符串指示正在使用的平台。如果是 Window 则用'nt'表示，对于 Linux/Unix 用户，它是 'posix'

### os.getcwd()

得到当前工作目录，即当前 Python 脚本工作的目录路径

### os.listdir()

返回指定目录下的所有文件和目录名

### os.mkdir(name)

创建指定目录

### os.remove(name)

删除指定文件

### os.rmdir(name)

删除指定空目录

### os.removedirs(name)

递归删除空目录

### os.system(command)

运行 shell 命令，并返回脚本的退出状态码（会等待命令执行完成）
执行成功返回 0

### os.popen(command)

运行 shell 命令，并返回脚本执行过程中的输出内容（不等待命令执行完成）

### os.rename("oldname", "newname")

重命名文件/目录（文件或目录都是使用这条命令）

### os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])

1. top 表示需要遍历的目录树的路径<br/>
2. topdown 的默认值是 "True",表示首先返回目录树下的文件，然后在遍历目录树的子目录 .Topdown 的值为 "False" 时，
   则表示先遍历目录树的子目录，返回子目录下的文件，最后返回根目录下的文件<br/>
3. onerror 的默认值是 "None"，表示忽略文件遍历时产生的错误.如果不为空，则提供一个自定义函数提示错误信息后继续遍历或抛出异常中止遍历<br/>
   该函数返回一个元组，该元组有 3 个元素，这 3 个元素分别表示当前遍历的目录，当前遍历的目录列表，当前遍历的目录的文件列表

## os.path --- 常见路径操作

### os.path.basename(path)

返回文件名

### os.path.dirname(**file**)

返回当前脚本所在路径

### os.path.abspath(path)

返回绝对路径

### os.path.exists(name)

判断文件/文件夹是否存在

### os.path.join()

智能连接一个或多个路径 q

---

## shutil 库 --- 高级文件操作

### shutil.copyfile("oldfile","newfile")

复制文件（oldfile 和 newfile 都只能是文件）

### shutil.copy("oldfile","newfile")

复制文件（oldfile 只能是文件夹，newfile 可以是文件，也可以是目标目录）

### shutil.copytree("olddir","newdir")

复制文件夹（olddir 和 newdir 都只能是目录，且 newdir 必须不存在）

### shutil.move("oldpos","newpos")

移动文件/目录

### shutil.rmtree(path)

递归删除文件夹

---

## pathlib 库 --- 面向对象的文件系统路径

---

## platform 库 --- 访问底层平台的识别数据

### platform.system()

返回操作系统名称，如：'Windows' 'Linux' 'Darwin'
