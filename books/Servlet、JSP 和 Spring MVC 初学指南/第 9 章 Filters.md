# 第 9 章 Filters

## 9.1 Filter API

[Interface Filter](https://docs.oracle.com/javaee/7/api/javax/servlet/Filter.html)

[Interface FilterConfig](https://docs.oracle.com/javaee/7/api/javax/servlet/FilterConfig.html)

[Interface FilterChain](https://docs.oracle.com/javaee/7/api/javax/servlet/FilterChain.html)

## 9.2 Filter 配置

有两种方法可以配置 Filter：一种是通过 `WebFilter` 的 Annotation 来配置 Filter，另一种是通过部署描述来注册。

[Annotation Type WebFilter](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebFilter.html)

**示例 1：**
```java
@WebFilter(filterName = "DataCompressionFilter", urlPatterns = { "/*" })
```

```xml
<filter>
     <filter-name>DataCompressionFilter</filter-name>
     <filter-class>
          the fully-qualified name of the filter class
     </filter-class>
</filter>
<filter-mapping>
     <filter-name>DataCompresionFilter</filter-name>
     <url-pattern>/*</url-pattern>
</filter-mapping>
```

**示例 2：**
```java
@WebFilter(filterName = "Security Filter", urlPatterns = { "/ *" },
          initParams = {
               @WebInitParam(name = "frequency", value = "1909"),
               @WebInitParam(name = "resolution", value = "1024")
          }
)
```

```xml
<filter>
     <filter-name>Security Filter</filter-name>
     <filter-class>filterClass</filter-class>
     <init-param>
          <param-name>frequency</param-name>
          <param-value>1909</param-value>
     </init-param>
     <init-param>
          <param-name>resolution</param-name>
          <param-value>1024</param-value>
     </init-param>
</filter>
<filter-mapping>
     <filter-name>DataCompresionFilter</filter-name>
     <url-pattern>/ *</url-pattern>
</filter-mapping>
```

## 9.3 示例 1：日志 `Filter`

`app09a/pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.app</groupId>
  <artifactId>app09a</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>app09a Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.1</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>app09a</finalName>
  </build>
</project>
```

`app09a/src/main/java/filter/LoggingFilter.java`
```java
package filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.PrintWriter;

@WebFilter(filterName = "LoggingFilter", urlPatterns = {"/*"},
        initParams = {@WebInitParam(name = "logFileName", value = "log.txt"),
                @WebInitParam(name = "prefix", value = "URI: ")})
public class LoggingFilter implements Filter {

    private PrintWriter logger;
    private String prefix;

    public void destroy() {
        System.out.println("destroying filter");
        if (logger != null) {
            logger.close();
        }
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        chain.doFilter(req, resp);
    }

    public void init(FilterConfig config) throws ServletException {
        prefix = config.getInitParameter("prefix");
        String logFileName = config.getInitParameter("logFileName");
        String appPath = config.getServletContext().getRealPath("/");

        System.out.println("logFileName: " + logFileName);
        try {
            logger = new PrintWriter(new File(appPath, logFileName));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
            throw new ServletException(e.getMessage());
        }
    }
}
```

`app09a/src/main/webapp/test.jsp`
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Filters</title>
</head>
<body>
    Testing filters
</body>
</html>
```

## 9.4 示例 2：图像文件保护 Filter

`app09a/src/main/java/filter/ImageProtectorFilter.java`
```java
package filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

@WebFilter(filterName = "ImageProtectorFilter", urlPatterns = {"*.png", "*.jpg", "*.gif"})
public class ImageProtectorFilter implements Filter {
    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println("ImageProtectorFilter");
        HttpServletRequest httpServletRequest = (HttpServletRequest) req;
        String referer = httpServletRequest.getHeader("referer");
        System.out.println("referer: " + referer);
        if (referer != null) {
            chain.doFilter(req, resp);
        } else {
            throw new ServletException("Image not available");
        }
    }

    public void init(FilterConfig config) throws ServletException {

    }

}
```

`app09a/src/main/webapp/index.jsp`
```
<img src='image/logo.png'/>
```

## 9.5 示例 3：下载计数 Filter

跳过，涉及到多线程知识

## 9.6 Filter 顺序

如果多个 Filter 应用于同一个资源，Filter 的触发顺序将变得非常重要，这时就需要使用部署描述来管理 Filter：指定哪个 Filter 先被触发。