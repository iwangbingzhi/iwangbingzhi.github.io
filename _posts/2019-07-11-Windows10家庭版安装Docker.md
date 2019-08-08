---
layout: post
title: Windows10家庭版及以下系统Docker环境的安装
feature-img: "/assets/article imgs/docker tutorial/pic1.jpg"
thumbnail: "/assets/article imgs/docker tutorial/pic1.jpg"
tags: [Windows,Docker]
author-id: wangbingzhi

---

# Windows10家庭版及以下版本系统Docker的安装

---



- ## 1.【下载Docker Toolbox】 
  - 1.1[点击下载最新版Docker Toolbox](<https://github.com/docker/toolbox/releases>)
- ## 2.【安装Docker】
  - 2.1双击下载下来的`docker toolbox.exe`文件，安装好之后会出现一个docker quickplay.exe的快捷方式
- ## 3.【Docker的启动】
  - 3.1如果是在安装Docker之前安装了Git，那么双击`docker quickstart.exe`会提示要找到`bash.exe`的快捷方式（此处的`bash.exe`需要找到Git安装目录的bin目录下的`bash.exe`)
- ## 4.【Docker的启动】
  - 4.1此时就等待命令行的开启，然后会配置一个VirtualBox中名字为default的Linux虚拟机
- ## 5.【查看Docker信息】
  - 5.1 看到Docker标志的鲸图标出现之后，我们就可以使用docker version查看Docker版本信息，使用`docker info`可以查看到当前的Docker仓库的镜像源
- ## 6.【Docker镜像源修改】
  - 6.1 在国内使用Docker需要修改下镜像源地址，否则下载速度特别慢，由于想要修改名字为default这个虚拟机的镜像源花费了很长时间无果，最后找到一个最为直接的方式，自己再创建一个虚拟机，并且在创建的时候为虚拟机赋予阿里云镜像源
- ## 7.【阿里云镜像源的使用】
  - 7.1 登陆注册阿里云，然后登陆后右上角有一个控制台三个字，点击进入，然后在产品部分看到弹性计算，然后选择 容器镜像服务，然后选择镜像加速器![]({{"/assets/article imgs/docker tutorial/aliyunmirror2.jpg" | relative_url}})
- ## 8.【阿里云镜像源的配置】
  - 8.1 按照下面红圈命令行创建一个带有阿里云镜像源地址的虚拟机，最后一个字段default是虚拟机名字，因为在启动Docker的时候会自动创建一个名为default的虚拟机，所以如果使用相同的名字创建会提示已经存在一个相同名字的虚拟机，要做的事就是把最后虚拟机名字改一下，然后使用ssh链接修改后的虚拟机就可以，Docker使用ssh命令如下，```docker-machine ssh 虚拟机名字```![]({{"/assets/article imgs/docker tutorial/aliyunmirror3.jpg" | relative_url}})
- ## 9.【Docker容器重新启动】
  - 9.1 在关闭计算机或是关闭了Docker服务后，如果是容器没有设置为自动启动，那么就需要自己使用命令手动启动。
  - 9.2 如启动MySql：首先输入命令 ```docker start mysql``` 启动Docker中的MySql  输入``` docker exec -it mysql /bin/bash```  ``` mysql -u root -p ```即可进入MySql
- 由于Docker Engine守护程序使用特定于Linux的内核功能，因此无法在Windows上本机运行Docker Engine。需要用到Docker Toolbox包含的Oracle VM VirtualBox创建的Linux VM。在Win10专业版中可开启Windows自带的Hyper-V系统管理程序虚拟化技术，但家庭版及Win10以下的版本不带有Hyper-V，即便是通过在家庭版中修改注册表的方式开启Hyper-V也无效，所以只能使用Docker Toolbox。