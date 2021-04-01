---
title: 使用 Docker 创建 MySQL 实验环境
date: 2019-09-26 19:42:09
updated: 2021-04-02 02:30:11
tags:
    - Docker
    - MySQL
categories:
    - Tools
---

由于有学习 MySQL 的需求，但是又不想破坏本地的 MySQL(MariaDB)，于是想到了使用 Docker 来创建符合需求的 MySQL 实验环境。
并且通过官方(?)提供的测试数据创建用于测试使用的数据库。

本文默认读者已安装好 Docker 及本地 MySQL。

<!-- more -->

## 拉取已有的 MySQL Docker 镜像。

使用下面的命令搜索可用的 mysql：

```sh
docker search mysql
```

可以看到类似下面的输出：

~~~
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   8621                [OK]
mariadb                           MariaDB is a community-developed fork of MyS…   2997                [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   637                                     [OK]
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   63
~~~

使用下面的命令拉取第一个官方镜像的最新版本：

```sh
docker pull mysql
```

## 创建并运行 Docker 容器中的 MySQL

使用下面的命令运行和配置刚刚拉取的 Docker 镜像：

```sh
docker run --name mysql-lab -p 3307:3306 -e MYSQL_ROOT_PASSWORD=password -d mysql
```

其中：

- `--name TEXT` 表示创建的镜像的名称，如果不提供该参数，则 docker 会随机生成一个名称。该示例中创建的名称为 mysql-lab
- `-e TEXT` 表示提供的环境变量的键值对。此处提供一个名为 MYSQL_ROOT_PASSWORD 的环境变量，其值为 password
- `-d` 表示在后台运行该容器
- `-p` 指定容器外端口到容器内端口的映射，3307 为容器外端口，3306 为容器内端口，即 MySQL 默认运行端口。指定后，即可在容器外通过 `localhost` 地址加上 3307 端口连接到容器内的 MySQL。就如同操作本地安装的 MySQL 一样，而不需要进行下方的容器外连接 MySQL 的操作。

## 检查容器的运行状态

使用下面的命令检查：

```sh
docker ps
```

可以类似下面的输出：

~~~
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                           NAMES
c046b3491396        mysql               "docker-entrypoint.s…"   4 hours ago         Up 4 hours          3306/tcp, 33060/tcp                             mysql-lab
~~~

## 连接 Docker 中的 MySQL

- 直接进入容然后进入 MySQL 命令行

    可以使用下面的命令直接进入 Docker 容器中然后连接该容器中的 MySQL：

    ```sh
    docker exec -it mysql-lab bash
    ```

    进入容器后，和在本地中使用类似，使用下面的命令即可进入 MySQL 命令行:

    ```sh
    mysql -u root -p
    ```

    密码为先前设置的 `password`

- ~~从容器外连接 MySQL~~ **可直接指定端口映射从容器外连接 MySQL**

    更好的方法是在容器外连接 MySQL。

    1. 首先使用下面的命令查找出刚刚创建的 Docker 镜像的地址：

    ```sh
    docker inspect mysql-lab | grep IPAddress
    ```

    可以看到如下的输出：

    ~~~
    "IPAddress": "172.17.0.2"
    ~~~

    2. 根据刚刚查找出的地址连接 MySQL 数据库

    ~~~
    mysql -u root -h 172.17.0.2 -P 3306 -p
    ~~~

## 导入测试数据

1. 下载测试数据

    [测试数据地址](https://github.com/datacharmer/test_db)

2. 按照地址中的 `README` 进行操作即可。下方操作只导入了数据库模型，并未导入实际的数据。且指定端口后不再需要类似下方的操作。

1. 进入到下载的测试数据的目录下

1. 导入到数据库中

    ```sh
    mysql -u root -h 172.17.0.2 -P 3306 -p < employees.sql
    ```

1. 测试导入的数据完整性

    ```sh
    mysql -u root -h 172.17.0.2 -P 3306 -p -t < test_employees_md5.sql
    ```
