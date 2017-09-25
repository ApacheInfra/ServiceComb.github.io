---
title: "分布式调用链追踪"
lang: cn
ref: quick-start-distributed-tracing
permalink: /cn/docs/quick-start-advance/distributed-tracing/
excerpt: "介绍如何在体质指数应用中使用ServiceComb提供的分布式追踪能力"
last_modified_at: 2017-09-03T10:01:43-04:00
---

{% include toc %}
分布式调用链追踪用于有效地监控微服务的网络延时并可视化微服务中的数据流转。本指南将展示如何在 *体质指数* 应用中使用 **ServiceComb** 提供的分布式调用链追踪能力。

## 前言

在您进一步阅读之前，请确保您已阅读了[微服务应用快速开发指南](/cn/docs/quick-start-bmi/)，并已成功运行体质指数微服务。

## 启用

1. 在 *体质指数计算器* 的 `pom.xml` 文件中添加依赖项：

   ```xml
       <dependency>
         <groupId>io.servicecomb</groupId>
         <artifactId>handler-tracing-zipkin</artifactId>
       </dependency>
   ```

2. 在 *体质指数计算器* 的 `microservice.yaml` 文件中添加分布式追踪的处理链：

   ```yaml
   cse:
     handler:
       chain:
         Provider:
           default: tracing-provider
   ```

3. 在 *体质指数界面* 的 `pom.xml` 文件中添加依赖项：

   ```xml
       <dependency>
         <groupId>io.servicecomb</groupId>
         <artifactId>handler-tracing-zipkin</artifactId>
       </dependency>
       <dependency>
         <groupId>io.servicecomb</groupId>
         <artifactId>spring-cloud-zuul-zipkin</artifactId>
       </dependency>
   ```

4. 在docker容器环境下运行 *体质指数计算器* 微服务和*体质指数界面* 微服务：

　　上述2.和 3.提供的是在非docker容器中运行的方式。下面介绍如何在docker容器中运行的方法。

　　4.1 打包镜像和运行*体质指数计算器* 微服务

　　　假设zipkin在虚拟机192.168.100.101的9411中运行。
   
　　　 dockerfile:

   ```bash
     FROM anapsix/alpine-java:latest
     ADD bmi-calculator-0.3.0-SNAPSHOT.jar /bmi-calculator-tracing.jar
     EXPOSE 7781
     ENTRYPOINT java $JAVA_OPTS -jar bmi-calculator-tracing.jar "-Ptracing"
   ```
      打包image文件：

   ```bash
     root@i-wzmhsx68:/home/ubuntu/docker/bmi-calculator-tracing# ls
     bmi-calculator-0.3.0-SNAPSHOT.jar  dockerfile
     root@i-wzmhsx68:/home/ubuntu/docker/bmi-calculator-tracing# docker build -t="bmi-calculator-tracing" .
   ```
     运行docker容器：

   ```bash
     docker run -d -e JAVA_OPTS="-Dservicecomb.tracing.collector.address=http://192.168.100.101:9411" bmi-calculator-tracing
   ```


　　4.2 打包镜像和运行*体质指数页面* 微服务
   
     用同样的方法打包镜像和运行*体质指数页面* 微服务。重新编写dokerfile，编译jar文件以及打包和运行docker镜像文件。
    （注意将4.1中的bmi-calculator替换为bmi-webapp）。
   

体质指数应用中已配置好了上述配置项，您只需执行以下几步即可：

1. 使用 Docker 运行 *Zipkin* 分布式追踪服务：

   ```bash
   docker run -d -p 9411:9411 openzipkin/zipkin
   ```

2. 重启 *体质指数计算器* 微服务：

   ```bash
   mvn spring-boot:run -Ptracing -Drun.jvmArguments="-Dcse.handler.chain.Provider.default=tracing-provider"
   ```
   
3. 重启 *体质指数界面* 微服务：

   ```bash
   mvn spring-boot:run -Ptracing
   ```

## 验证

1. 访问 <a>http://localhost:8888</a> ，在身高和体重栏处输入正数，并点击 *Submit* 按钮。

2. 访问 <a>http://localhost:9411</a> ，查看分布式调用追踪情况，可得下方界面。

![分布式追踪效果](/assets/images/distributed-tracing-result.png){: .align-center}

## 下一步

* 阅读[基于 ServiceComb 和 Zipkin 的分布式调用链追踪](/cn/docs/tracing-with-servicecomb/)来进一步了解分布式追踪 

* 认识 [**ServiceComb** 微服务开发框架](http://servicecomb.io/cn/users/user-guide/)

* 通过 [Company应用](http://servicecomb.io/cn/docs/linuxcon-workshop-demo/) 更深入地了解微服务开发
