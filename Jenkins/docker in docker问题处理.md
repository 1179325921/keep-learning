![image](./images/docker-in-docker.png)

问题：pipeline中使用docker进行docker build,push命令，会报没有docker命令。

原因： 应为jenkins job调用k8s创建新的pod，来跑这个job流水线任务，这个新pod,没有docker命令。

解决方案：把宿主机的docker和docker.sock映射到新的pod内，通过挂载卷的方式把/usr/bin/docker,/var/run/docker.sock挂载出来。

问题：容器内使用docker命令，报错：
![image](./images/docker-in-docker-problems.png)

原因：容器内 /usr/lib缺少这个libltdl.so.7

解决方案：重新构建jenkins-slave镜像，加入/usr/lib/libltdl.so.7

Dockerfile:

![image](./images/jenkins-dockerfile.png)