# Docker学习之Namespace机制

Namespace 技术实际上修改了应用进程看待整个计算机“视图”，即它的“视线”被操作系统做了限制，只能“看到”某些指定的内容。但对于宿主机来说，这些被“隔离”了的进程跟其他进程并没有太大区别。

Docker 就会在这个第 100 号员工入职时给他施一个“障眼法”，让他永远看不到前面的其他 99 个员工，更看不到比尔 · 盖茨。这样，他就会错误地以为自己就是公司里的第 1 号员工。

这种机制，其实就是对被隔离应用的进程空间做了手脚，使得这些进程只能看到重新计算过的进程编号，比如 PID=1。可实际上，他们在宿主机的操作系统里，还是原来的第 100 号进程。

> 不能Namespace化的：时间，如果你的容器中的程序使用 settimeofday(2) 系统调用修改了时间，整个宿主机的时间都会被随之修改，这显然不符合用户的预期。相比于在虚拟机里面可以随便折腾的自由度，在容器里部署应用的时候，“什么能做，什么不能做”，就是用户必须考虑的一个问题。

Namespace并不能像虚拟机一样很好的隔离，首先，虽然可以使用Mount Namesapce在容器内运行其他不同版本的操作系统文件，但是无法改变容器是运行在宿主机操作系统上的，并且共享宿主机内核，不像虚拟机一样可以在其之上任意的创建其他不共享宿主机操作系统内核的虚拟机。最后，并非所有资源都能被Namesapce化。