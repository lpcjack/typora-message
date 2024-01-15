# 一，springboot简介

---

​	**大多数人把Spring Boot称为搭建程序的脚手架。其==最主要作用就是帮我们快速的构建庞大的spring项目，并且尽可能的减少一切xml配置==，做到开箱即用，迅速上手，让我们关注于业务而非配置。**

​	Spring Boot其实就是一个整合很多**可插拔的组件（框架）**，内嵌了使用工具（比如内嵌了Tomcat、Jetty等），方便开发人员快速搭建和开发的一个框架。

---

# 二，创建springboot项目

---

## 1，创建方式

**Spring Boot项目创建，有两种方式：**

- **方式一：访问网站构建项目，下载到本地导入开发工具**
- **方式二：使用IDEA（IntelliJ IDEA的简称）工具==使用Spring Initializr创建初始化==（推荐使用）**

## 2，**使用IDEA Spring Initializr创建（推荐使用）**

### （1），具体创建步骤

![img](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152053939.png)

![img](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152055917.png)

**注意：**springboot的版本对jdk的版本是有要求的，如果springboot的版本选择3以上，那么上一个步骤jdk的版本就必须选择17，否则会报错 。

![img](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152057955.png)

### （2），创建后文件具体解释

![img](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152059759.png)

### （3），需要导入的依赖

---

- #### 导入SpringBoot的父项目。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>
```

- #### 导入需要的场景启动器，比如：要开发web项目。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

---

