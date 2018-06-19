[TOC]



# 使用Docker Compose在线安装Jenkins



## 前言



一般地说，Jenkins 支持通过3种方式来安装：

* war包

  支持通过`java -jar jenkins.war` 的方式来运行，或者将jenkins.war放在Tomcat中来运行。

* rpm / deb包

  以CentOS为例，可以通过rpm包的方式来安装Jenkins

* docker镜像

  可以通过docker容器来运行Jenkins。



本文介绍了使用Docker Compose在线安装Jenkins，也介绍了通过Docker run方式在线安装Jenkins。



## 官方文档

- https://jenkins.io/download/
- https://jenkins.io/doc/book/installing/
- https://github.com/jenkinsci/docker
- https://hub.docker.com/r/jenkins/jenkins/



## 使用Docker-Compose安装Jenkins



### 前置条件



假设机器上已经安装了Docker和Docker-Compose。



如果还未安装，请参考Docker官方文档进行安装。



国内用户也可以参考以下参考文档：

* [CentOS7用阿里云Docker Yum源在线安装Docker 17.03.2](https://blog.csdn.net/nklinsirui/article/details/80610058)
* [Docker国内Yum源和国内镜像仓库](https://blog.csdn.net/nklinsirui/article/details/80490537)



### 安装步骤



克隆或复制[jenkins](https://github.com/cookcodeblog/docker-compose-examples/tree/master/jenkins)目录到机器上。



在jenkins目录运行以下命令：

```bash
# 给脚本授权
chmod u+x *.sh
# 创建Jenkins数据目录（用作持久卷）
./pre_install_jenkins.sh
# 启动Jenkins容器
docker-compose up -d
```



运行 `docker log -f jenkins`  查看Jenkins日志。



找到Jenkins Initial Password的日志，比如：



```txt
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

<Initial Admin Password>

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
```



访问Jenkins网址http://<server_ip>:8080，输入上面生成的初始密码，按照提示安装到成功完成。




## 使用Docker run来安装Jenkins



参考上面的步骤，只是使用下面的`docker run` 命令代替`docker-compose up -d`。



```bash
docker run --name jenkins \
           -d \
           -p 8080:8080 \
		  -p 50000:50000 \
		  -v /var/jenkins_home:/var/jenkins_home \
		  --restart always \
		  jenkins/jenkins:lts
```



上面的`docker run` 命令参见docker_run_install_jenkins.sh。









