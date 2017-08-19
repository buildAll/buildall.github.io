title: 让你的Nodejs项目泡在Docker上
date: 2016-10-04 8:30:51
tags: docker, docker-compose, nodejs docker
---

## 本文目的
如题，让你的Nodejs运行在Docker上

## 什么是Docker?
为了解释这个问题，我想先列出一些名词：

- Container(容器)
- Docker platform(Docker平台)
- Docker Engine(Docker引擎)

1. ### Container，容器
日常生活中，装水的杯子是一个容器，盛饭的碗是一个容器。维基百科对[* 容器 *](https://zh.wikipedia.org/wiki/%E5%AE%B9%E5%99%A8)的解释是_用来包装或装载物品的贮存器（如箱、罐、坛）或者成形或柔软不成形的包覆材料_。
对于Docker里的容器Docker官网有这样一句话Docker provides the ability to package and run an application in a loosely isolated environment called a container. 翻译一下就是可以独立包装并运行应用程序的一个独立的环境。可以看到，这和日常生活中容器的概念很像。

2. Docker platform，Docker平台
我们说的Docker，基本上就是这个Docker平台。它把应用封装到Docker容器里,并且确保这个容器可以运行在各种主流的操作系统里，如Windows,Linux等。

3. Dokcer Engine，Docker引擎
Docker引擎是一个client-server模型，它包括：
- 服务端，这个服务端是一个叫Docker daemon的进程，驻留在操作系统里，随时为Docker客户端提供服务。
- 客户端，我们在使用docker时，总是要通过命令行输入以docker开头的命令

Docker的内核，管理Docker中的容器、image、网络通讯、Docker API和命令行。



1. 用过类似npm、pod这样的包管理工具
2. 写过package.json或Podfile这类的文件
3. 了解一些基本的Linux命令
4. 本文的例子用的是nodejs项目，你了解nodejs就更好了

如果你想跟着本文中的命令一起操作，那么就请去到[Docker官网](https://www.docker.com/)找到安装链接，点击并安装好Docker吧。

OK. Docker is so simple, 让我们开始学会它吧。

## Docker是什么？

先看一个例子，新建一个文件名为Dockerfile的文件，输入以下内容:








    原理: docker本身提供一个最基本的image，用来虚拟一个操作系统容器，对于该虚拟系统的每次改动都成为一个image，该image保存下来后，可以生成新的容器。docker就这样通过image这个概念来记录、还原开发环境，从而达到统一环境的目的。

    docker做了什么？

    1. 在各个主流操作系统中虚拟Linux操作系统。
    2. 生成、管理docker image
    3. 通过实例化image创建程序运行的container

    相关概念：

    - docker image: 用来创建容器的图纸，docker官方提供主流的Linux系统的image
    - docker run: 实例化image到container的一条命令
    - Dockerfile: 将一组运行容器、安装依赖、配置环境等命令写在一个文件
    - docker build: 运行Dockerfile
    - docker compose: 通过配置.yml文件，然后一键运行多个container

