[TOC]

# 第三节 ServletContextListener

## 1、实用性

将来学习SpringMVC的时候，会用到一个ContextLoaderListener，这个监听器就实现了ServletContextListener接口，表示对ServletContext对象本身的生命周期进行监控。

## 2、具体用法

### ①创建监听器类

```java
public class AtguiguListener implements ServletContextListener {
    @Override
    public void contextInitialized(
            // Event对象代表本次事件，通过这个对象可以获取ServletContext对象本身
            ServletContextEvent sce) {
        System.out.println("Hello，我是ServletContext，我出生了！");

        ServletContext servletContext = sce.getServletContext();
        System.out.println("servletContext = " + servletContext);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("Hello，我是ServletContext，我打算去休息一会儿！");
    }
}
```

### ②注册监听器

```xml
<!-- 每一个listener标签对应一个监听器配置，若有多个监听器，则配置多个listener标签即可 -->
<listener>
    <!-- 配置监听器指定全类名即可 -->
    <listener-class>com.atguigu.listener.AtguiguListener</listener-class>
</listener>
```

事件触发过程中控制台日志的打印：

> Connected to server
> [2021-03-20 04:23:20,982] Artifact pro10-listener:war exploded: Artifact is being deployed, please wait...
> 三月 20, 2021 4:23:21 下午 org.apache.catalina.deploy.WebXml setVersion
> 警告: Unknown version string [4.0]. Default version will be used.
> Hello，我是ServletContext，我出生了！
> servletContext = org.apache.catalina.core.ApplicationContextFacade@6a66017e
> [2021-03-20 04:23:21,426] Artifact pro10-listener:war exploded: Artifact is deployed successfully
> [2021-03-20 04:23:21,426] Artifact pro10-listener:war exploded: Deploy took 444 milliseconds
> 三月 20, 2021 4:23:30 下午 org.apache.catalina.startup.HostConfig deployDirectory
> 信息: Deploying web application directory D:\software\apache-tomcat-7.0.57\webapps\manager
> 三月 20, 2021 4:23:31 下午 org.apache.catalina.startup.HostConfig deployDirectory
> 信息: Deployment of web application directory D:\software\apache-tomcat-7.0.57\webapps\manager has finished in 124 ms
> [2021-03-20 04:24:06,422] Artifact pro10-listener:war exploded: Artifact is being deployed, please wait...
> Hello，我是ServletContext，我打算去休息一会儿！
> Hello，我是ServletContext，我出生了！
> servletContext = org.apache.catalina.core.ApplicationContextFacade@2a55374c
> [2021-03-20 04:24:07,115] Artifact pro10-listener:war exploded: Artifact is deployed successfully
> [2021-03-20 04:24:07,115] Artifact pro10-listener:war exploded: Deploy took 694 milliseconds



[上一节](verse02.html) [回目录](index.html)