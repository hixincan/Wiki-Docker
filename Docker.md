*弱小和无知不是生存的障碍，傲慢才是*



# Docker为什么出现？

一款产品开发、线上至少有两套环境，每一台机器都要配置环境，费时费力，并且有一些配置时不跨平台的。

java -> api -> 发布（应用商店） -> 下载api -> 安装即用

java -> jar -> 打包项目带上环境（镜像）-> Docker仓库（商店）-> 下载我们发布的镜像 -> 直接运行

Docker的思想打包装箱，互相隔离。通过隔离机制，将服务器利用到极致！



# Docker历史

2010 公司成立

2013 开源

2014 Docker 1.0 发布



> 聊聊Docker 

Docker 是 go 语言开发

https://docs.docker.com/  官方文档，非常详细

https://hub.docker.com/ Docker镜像仓库，有 push 和 pull 操作



# Docker对比

| Feature        | Vwware虚拟机软件 | Docker     |
| -------------- | ---------------- | ---------- |
| 实现方式       | 虚拟化技术       | 容器化技术 |
| 隔离           | 是               | 是         |
| 完整的操作系统 | 是               | 否         |
| 资源占用       | 几G              | 几M        |
| 启动           | 几分钟           | 几秒       |
| 操作步骤       | 冗余步骤多       | 无         |



> 容器化架构对比

|        | 容器化                               | 虚拟化                     |
| ------ | ------------------------------------ | -------------------------- |
| 内核   | 没有自己的内核，直接依赖宿主机的内核 | 虚拟硬件，有完整的操作系统 |
| 环境库 | 每个容器有独立的环境库               | 共享环境库                 |
| 隔离性 | 容器之间互相隔离，且有自己的文件系统 | 没有隔离环境               |



# Docker用途

> DevOps（两个单词组合    开发、运维）

1. 应用更快速的交付和部署

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行



2. 更便捷的升级和扩容

使用Docker之后，部署应用就和搭积木一样

环境升级、水平扩容



3. 更简单的系统运维

在容器化之后，我们的开发、测试环境都高度一致



4. 更高效的计算资源利用

Docker 是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！服务器的性能可以压榨到极致



# Docker名词

1. 镜像：Images，相当于 Java 的类

Docker 镜像相当于模板，通过运行镜像创建多个容器



2. 容器：Containers，相当于 Java 的对象（类的实例化）

Docker利用容器技术，独立运行一个或一组应用，容器由镜像创建。

启动、停止、删除

可以理解容器是一个简易的linux操作系统



3. 仓库：Repository

仓库是存放镜像的地方

仓库分为共有仓库和私有仓库

需要配置镜像加速





# Docker安装

> 基于 CentOS 7 安装

1. 官网安装参考手册：https://docs.docker.com/engine/install/centos/

2. 确定你是CentOS7及以上版本

    ```
    [root@192 Desktop]# cat /etc/redhat-release
    CentOS Linux release 7.2.1511 (Core)
    ```

3. yum安装gcc相关

    ```
    yum -y install gcc
    yum -y install gcc-c++
    ```

4. 卸载旧版本

    ```
    yum -y remove docker docker-common docker-selinux docker-engine
    # 官网版本
    yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-engine
    ```

5. 安装需要的软件包

    ```
    yum install -y yum-utils device-mapper-persistent-data lvm2
    ```

6. 设置stable镜像仓库

    ```
    # 错误
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ## 报错
    [Errno 14] curl#35 - TCP connection reset by peer
    [Errno 12] curl#35 - Timeout
    
    # 正确推荐使用国内的
    yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    ```

7. 更新yum软件包索引

    ```
    yum makecache fast
    ```

8. 安装Docker CE

    ```
    yum -y install docker-ce docker-ce-cli containerd.io
    ```

9. 启动docker

    ```
    systemctl start docker
    ```

10. 测试

    ```
    docker version
    docker run hello-world
    docker images
    ```







