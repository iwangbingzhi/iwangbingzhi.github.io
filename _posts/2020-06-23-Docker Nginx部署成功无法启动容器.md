# Docker Nginx部署成功无法启动容器

---

**前提**有使用-v 挂载docker容器的目录以及本地的目录，在docker logs nginx中提示该错误 ，

2019/08/21 17:14:03 [emerg] 1#1: open() "/etc/nginx/nginx.conf" failed (2: No such file or directory)

nginx: [emerg] open() "/etc/nginx/nginx.conf" failed (2: No such file or directory)

**提示找不到nginx.conf**



根据docker官方人员提示（https://github.com/nginxinc/docker-nginx/issues/360）：启动一个新的容器，将该容器中的conf文件夹下的内容复制到你的本地目录中：

当使用绑定挂载的文件夹(如${D_DRIVE}/nginx/conf/:/etc/nginx/)时，它不会被镜像中的任何内容所填充。如果在目标文件夹(/etc/nginx/)中有镜像文件，那么这些文件将被隐藏起来，基本上无法在该容器中访问。为了给nginx提供配置，你需要在你的主机上提供一个完整的配置文件（以及任何其他需要的文件，比如ssl证书），然后将包含的文件夹绑定到容器中正确的位置。你可以用下面的方法从容器中获取初始nginx配置的副本（但你很可能需要为你的设置定制它）。

$ docker container create --name tmp1 nginx $ # 假设D_DRIVE在shell中被适当地设置。 $ mkdir -p ${D_DRIVE}/nginx/conf/。 $ docker cp tmp1:/etc/nginx/conf.d/default.conf ${D_DRIVE}/nginx/conf/。 $ docker container rm tmp1

按照上述方法操作成功后，再次使用docker run就可以运行nginx镜像了