---
layout: page
title: Tomcat项目部署方式汇总
category: Java
date: 2019-01-02
author: "Eric Zong"
---

* content
{:toc}

# Tomcat 项目部署方式

## 拷贝法

将项目拷贝到 ${Tomcat}/webapps 目录下即可。

> 注意：此时项目名（URL）就是发布的目录名或 war 包名。

## Server 配置法

修改 ${Tomcat}/conf/server.xml 配置文件，在最后的 <Host> 中添加 <Context>，内容如下：

```
<Context docBase="${project-dir}" 
         path="${project-name}" 
         reloadable="true" 
         source="org.eclipse.jst.jee.server:ericmanager" />
```

> 注意：这种配置方法项目的位置是任意的，项目名（URL）是配置指定的，而不是目录名。
>
> 问题：这种配置方法可以发布war 包吗？

## Catalina 配置法

在 \${Tomcat}/conf/Catalina/localhost 目录下创建配置文件 ${project-name}.xml。内容如下：

```
<Context docBase="${project-dir}" debug="0" privileged="true" />
```

>
> 注意：项目位置也是任意的，项目名（URL）是配置文件名，而不是目录名。
>
> 问题：可以发布 war 包吗？

## Tomcat 发布管理

以上方法都是手动在 Tomcat 里配置完成发布的，其实 Tomcat 自带有应用管理系统。

访问 http://\${host}:${port}/ 即可访问 Tomcat 的管理界面，点击其中的“manager webapp”链接即可跳转至应用管理界面。

其中，项目发布方式有 2 类，一类是本地发布，一类是远程发布。

### 本地发布

本地发布形式上看就是给上述的拷贝法和Catalina 配置法增加了一个配置界面，然后由 Tomcat 根据配置进行发布。

需要填写 3 个参数：

* Context Path (required)：必需，项目名（URL），注意必须以“/”开头。
* XML Configuration file URL：Catalina 配置法中的 xml 配置文件位置。
* WAR or Director URL：项目位置。

如果指定了 xml 配置文件，那么将以 Catalina 配置的形式发布；如果指定项目位置，则以拷贝发布。

### 远程发布

远程发布很简单，只需要在客户机上选择项目 war 包，点击“Deploy”即可发布。

其原理简单来说跟拷贝发布是类似的，只是 war 包通过 Web 服务上传，因而支持远程发布。

需要注意的是，如果工程足够大，那么 war 文件可能超过管理系统规定的文件上限。

修改 ${Tomcat}/webapps/manager/WEB-INF/web.xml，<max-file-size>。

# 附录

## Tomcat 常用配置

| 配置项      | 配置文件位置                                   |
| -------- | ---------------------------------------- |
| 服务器端口    | ${Tomcat}/conf/server.xml                |
| 运行多个服务   | 同上  修改所有端口号                              |
| 列出页面     | ${Tomcat}/conf/web.xml  参数 listings 的值改为 true |
| 管理系统文件上限 | ${Tomcat}/webapps/manager/WEB-INF/web.xml  <max-file-size> |
| 管理系统用户配置 | ${Tomcat}/conf/tomcat-users.xml          |