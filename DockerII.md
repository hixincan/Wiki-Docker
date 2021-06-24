*黑发不知勤学早，白首方悔读书迟*





# 容器数据卷

## 什么是容器数据卷

> Docker 理念回顾

将应用和环境打包成一个镜像！运行镜像会生成容器。



**卷技术的背景**

* 防止容器删除后，容器中的数据也丢失
* 解决容器之间的数据共享



> 什么是卷技术

将容器中的数据同步到本地，让容器之间可以有一个数据共享的技术。



**实现原理**

目录挂载：==将容器内的目录，挂载到Linux上面==！

类似于双向绑定，且与容器是否启动无关



**为什么要用卷技术**

* 容器的持久化和同步操作
    * 将数据同步到本地，保证数据安全
    * 将配置目录同步到本地，方便修改
* 容器间数据共享





## 使用数据卷

例子：在 centos 镜像中同步目录 /hone/test 到本地

```shell
docker run -it -v /home/test:/home/test   centos:centos7 /bin/bash
```





## 实战：安装MySQL

**思考：MySQL的数据持久化的问题**

官方文档详尽：https://hub.docker.com/_/mysql

```
docker pull mysql:5.7.34

--name 别名，下次通过 docker start name 直接启动
-v 卷映射  主机/home/mysql/conf —— 映射配置目录和数据目录
-p 端口映射 主机3310
-e 环境配置，设置密码，root默认密码 my-secret-pw（注意，这是初始密码！！！）
-d 后台运行
docker run --name mysql02 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -p 3310:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.7.34
```

启动后，用客户端来测试连接

![image-20210624175501184](DockerII.assets/image-20210624175501184.png)	





## 从本地数据恢复容器

已知数据持久化在本地，若容器被删除了，如何用数据恢复新容器？





## 具名和匿名挂载

-v 参数

| 挂载方式 | -v 形式              | 示例                                  | 卷工作地址                      | inspect ls |
| -------- | -------------------- | ------------------------------------- | ------------------------------- | ---------- |
| 指定挂载 | -v 宿主路径:容器路径 | -v /home/mysql/conf:/etc/mysql/conf.d | /home/mysql/conf                | 不可见     |
| 匿名挂载 | -v 容器路径          | -v /etc/mysql/conf.d                  | /var/lib/docker/volumes/xxxxxxx | 可见       |
| 具名挂载 | -v 卷名称:容器路径   | -v mysql:/etc/mysql/conf.d            | /var/lib/docker/volumes/mysql   | 可见       |

```shell
# 匿名挂载
-v 容器内路径!
$ docker run -d -P --name nginx01 -v /etc/nginx nginx

docker volumn inspect 卷名称
[root@centos7 ~] docker run -d -P --name nginx02 -v nginxconf:/etc/nginx nginx
6b1ce7aa8b9ff929f1ab7825381bdd786184620a0fdfeddc4ebb472e02260c20
[root@centos7 ~] docker volume ls
DRIVER    VOLUME NAME
local     b9d257c19e03b4e396a8aa904b76e65108b48694eb7837750b5c3edc24b306eb
local     nginxconf

[root@centos7 ~] docker volume ls
DRIVER    VOLUME NAME
local     b9d257c19e03b4e396a8aa904b76e65108b48694eb7837750b5c3edc24b306eb
local     nginxconf
[root@centos7 ~] docker volume inspect nginxconf
[
    {
        "CreatedAt": "2021-06-24T20:41:13+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/nginxconf/_data",
        "Name": "nginxconf",
        "Options": null,
        "Scope": "local"
    }
]
```



==注意==

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/自定义的卷名/_data**下，
**如果指定了目录，docker volume ls 是查看不到的**。



==拓展==

```shell
docker run -d -P --name nginx01 -v /etc/nginx:ro nginx
docker run -d -P --name nginx01 -v /etc/nginx:rw nginx
```

ro：read only 仅允许宿主机写，容器只读

rw：read write 容器可读可写



# DockerFile

## 初识DockerFile

Dockerfile 就是用来构建docker镜像的**构建文件**（命令脚本）

通过这个脚本可以**生成镜像**，镜像是一层一层的，脚本是一个个的命令，每个命令都是一层！





## 与commit区别

commit 也是生成镜像





# Docker网络





































