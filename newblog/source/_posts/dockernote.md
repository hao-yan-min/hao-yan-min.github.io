---
title: docker使用笔记
date: 2023-05-01 22:43:54
categories:
- 虚拟机
tags:
- Docker
- 虚拟机
---

# Docker



### 搜索 Docker 镜像

Docker Hub 是 Docker 镜像的官方仓库，可以通过 docker search 命令在 Docker Hub 中搜索需要的镜像。例如，要搜索 Ubuntu 镜像，可以使用以下命令：

```bash
docker search ubuntu
```



### 下载 Docker 镜像

可以使用 docker pull 命令从 Docker Hub 下载需要的镜像。例如，要下载 Ubuntu 18.04 镜像，可以使用以下命令：

```bash
docker pull ubuntu:18.04
```



### 运行 Docker 容器

可以使用 docker run 命令运行 Docker 容器。例如，要在 Ubuntu 18.04 镜像上运行一个新的容器，可以使用以下命令：

```bash
docker run -it --name mycontainer ubuntu:18.04
```

该命令中，-it 表示以交互模式运行容器，--name 指定容器名称，ubuntu:18.04 是镜像名称和标签。



### 查看 Docker 容器

可以使用 docker ps 命令查看正在运行的容器。例如，要查看正在运行的容器，可以使用以下命令：

```bash
docker ps
```



### 进入 Docker 容器

可以使用 docker exec 命令进入正在运行的容器。例如，要进入 mycontainer 容器，可以使用以下命令：

```bash
docker exec -it mycontainer /bin/bash
```

该命令中，-it 表示以交互模式进入容器，mycontainer 是容器名称，/bin/bash 是要进入的终端程序。



### 停止 Docker 容器

可以使用 docker stop 命令停止正在运行的容器。例如，要停止 mycontainer 容器，可以使用以下命令：

```bash
docker stop mycontainer
```



### 删除 Docker 容器

可以使用 docker rm 命令删除已停止的容器。例如，要删除 mycontainer 容器，可以使用以下命令：

```bash
docker rm mycontainer
```



### docker Hadoop

* 创建集群

``` shell
docker-compose up -d
```

* 关闭集群

```shell
docker-compose down
```

* 添加文件夹

```shell
mkdir dirname
```

* 移除文件夹

```shell
rm -r dirname
```

* 将外部文件拷贝到容器中

```shell
docker cp filepath containerID:path
```

* 上传文件到hdfs

```shell
hdfs dfs -put file filepath
```

* 将文件移动到本地

```shell
hdfs dfs -get file filepath
```

* 运行样例

  ![image-20221029140122971](/images/image-20221029140122971.png)

![image-20221029140150160](/images/image-20221029140150160.png)

```shell
hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount /wordcount/input/ /wordcount/output
```









### spark

* 运行指令

  ```shell
  docker run -it apache/spark-py /opt/spark/bin/pyspark
  ```

  