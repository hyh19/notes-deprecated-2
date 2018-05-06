# 第 1 章 Servlets

## 1.1 Servlet API 概览

[Package javax.servlet](https://docs.oracle.com/javaee/7/api/javax/servlet/package-summary.html)

[Package javax.servlet.http](https://docs.oracle.com/javaee/7/api/javax/servlet/http/package-summary.html)

[Package javax.servlet.annotation](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/package-summary.html)

## 1.2 [`Servlet`](https://docs.oracle.com/javaee/7/api/javax/servlet/Servlet.html) 

[Interface Servlet](https://docs.oracle.com/javaee/7/api/javax/servlet/Servlet.html)

## 1.3 编写基础的 `Servlet` 应用程序

命令行创建项目
```bash
mvn archetype:generate -B \
-DarchetypeGroupId=org.apache.maven.archetypes \
-DarchetypeArtifactId=maven-archetype-webapp \
-DgroupId=com.example.app \
-DartifactId=app01a \
-Dversion=1.0-SNAPSHOT \
```

`./src/main/java/app01a/MyServlet.java`
```java
package app01a;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "MyServlet", urlPatterns = {"/my"})
public class MyServlet implements Servlet {

    private transient ServletConfig servletConfig;

    public void init(ServletConfig servletConfig) {
        this.servletConfig = servletConfig;
    }

    public ServletConfig getServletConfig() {
        return servletConfig;
    }

    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws IOException {
        String servletName = servletConfig.getServletName();
        servletResponse.setContentType("text/html");
        PrintWriter writer = servletResponse.getWriter();
        writer.print("<html><head></head>"
                + "<body>Hello from " + servletName
                + "</body></html>");
    }

    public String getServletInfo() {
        return "My Servlet";
    }

    public void destroy() {

    }
}

```

`./pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.app</groupId>
  <artifactId>app01a</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>app01a Maven Webapp</name>
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
      <version>3.0.1</version>
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
    <finalName>app01a</finalName>
  </build>
</project>
```

[Annotation Type WebServlet](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebServlet.html)

## 1.4 [`ServletRequest`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html)

[Interface ServletRequest](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html)

[int getContentLength()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getContentLength--)

[String getContentType()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getContentType--)

[String getParameter(String name)](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getParameter-java.lang.String-)

[String getProtocol()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getProtocol--)

## 1.5 [`ServletResponse`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletResponse.html)

[Interface ServletResponse](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletResponse.html)

## 1.6 [`ServletConfig`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletConfig.html)

[Interface ServletConfig](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletConfig.html)

[String getInitParameter(String name)](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletConfig.html#getInitParameter-java.lang.String-)

[Enumeration<String> getInitParameterNames()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletConfig.html#getInitParameterNames--)

`./src/main/java/app01a/ServletConfigDemoServlet.java`
```java
package app01a;

import javax.servlet.*;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "ServletConfigDemoServlet",
        urlPatterns = {"/servletConfigDemo"},
        initParams = {
                @WebInitParam(name = "admin", value = "Harry Taciak"),
                @WebInitParam(name = "email", value = "admin@example.com")
        })
public class ServletConfigDemoServlet implements Servlet {

    private transient ServletConfig servletConfig;

    public void init(ServletConfig servletConfig) throws ServletException {
        this.servletConfig = servletConfig;
    }

    public ServletConfig getServletConfig() {
        return servletConfig;
    }

    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        ServletConfig servletConfig = getServletConfig();
        String admin = servletConfig.getInitParameter("admin");
        String email = servletConfig.getInitParameter("email");
        servletResponse.setContentType("text/html");
        PrintWriter writer = servletResponse.getWriter();
        writer.print("<html><head></head><body>" +
                "Admin:" + admin +
                "<br/>Email:" + email +
                "</body></html>");
    }

    public String getServletInfo() {
        return "ServletConfig demo";
    }

    public void destroy() {

    }
}
```

[public abstract WebInitParam[] initParams](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebServlet.html#initParams--)

[Annotation Type WebInitParam](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebInitParam.html)

## 1.7 [`ServletContext`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html)

[ServletContext getServletContext()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletConfig.html#getServletContext--)

## 1.8 [`GenericServlet`](https://docs.oracle.com/javaee/7/api/javax/servlet/GenericServlet.html)

[Class GenericServlet](https://docs.oracle.com/javaee/7/api/javax/servlet/GenericServlet.html)

[abstract void	service(ServletRequest req, ServletResponse res)](https://docs.oracle.com/javaee/7/api/javax/servlet/GenericServlet.html#service-javax.servlet.ServletRequest-javax.servlet.ServletResponse-)

`./src/main/java/app01a/GenericServletDemoServlet.java`
```java
package app01a;

import javax.servlet.*;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "GenericServletDemoServlet",
        urlPatterns = {"/generic"},
        initParams = {
                @WebInitParam(name = "admin", value = "Harry Taciak"),
                @WebInitParam(name = "email", value = "admin@example.com")
        })
public class GenericServletDemoServlet extends GenericServlet {

    private static final long serialVersionUID = 62500890L;

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        ServletConfig servletConfig = getServletConfig();
        String admin = servletConfig.getInitParameter("admin");
        String email = servletConfig.getInitParameter("email");
        servletResponse.setContentType("text/html");
        PrintWriter writer = servletResponse.getWriter();
        writer.print("<html><head></head><body>" +
                "Admin:" + admin +
                "<br/>Email:" + email +
                "</body></html>");
    }
}
```

## 1.9 Http Servlets

[Package javax.servlet.http](https://docs.oracle.com/javaee/7/api/javax/servlet/http/package-summary.html)

[Class HttpServlet](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServlet.html)

[protected void	doGet(HttpServletRequest req, HttpServletResponse resp)](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServlet.html#doGet-javax.servlet.http.HttpServletRequest-javax.servlet.http.HttpServletResponse-)

[protected void	doPost(HttpServletRequest req, HttpServletResponse resp)](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServlet.html#doPost-javax.servlet.http.HttpServletRequest-javax.servlet.http.HttpServletResponse-)

[Interface HttpServletRequest](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html)

[Interface HttpServletResponse](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletResponse.html)

[Enumeration<String> getParameterNames()](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getParameterNames--)

[String[] getParameterValues(String name)](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html#getParameterValues-java.lang.String-)

`./src/main/java/app01b/FormServlet.java`
```java
package app01b;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

@WebServlet(name = "FormServlet", urlPatterns = {"/form"})
public class FormServlet extends HttpServlet {

    private static final long serialVersionUID = 54L;
    private static final String TITLE = "Order Form";

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        PrintWriter writer = resp.getWriter();
        writer.println("<html>");
        writer.println("<head>");
        writer.println("<title>" + TITLE + "</title></head>");
        writer.println("<body><h1>" + TITLE + "</h1>");
        writer.println("<form method='post'>");
        writer.println("<table>");
        writer.println("<tr>");
        writer.println("<td>Name:</td>");
        writer.println("<td><input name='name'/></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Address:</td>");
        writer.println("<td><textarea name='address' "
                + "cols='40' rows='5'></textarea></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Country:</td>");
        writer.println("<td><select name='country'>");
        writer.println("<option>United States</option>");
        writer.println("<option>Canada</option>");
        writer.println("</select></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Delivery Method:</td>");
        writer.println("<td><input type='radio' " +
                "name='deliveryMethod'"
                + " value='First Class'/>First Class");
        writer.println("<input type='radio' " +
                "name='deliveryMethod' "
                + "value='Second Class'/>Second Class</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Shipping Instructions:</td>");
        writer.println("<td><textarea name='instruction' "
                + "cols='40' rows='5'></textarea></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>&nbsp;</td>");
        writer.println("<td><textarea name='instruction' "
                + "cols='40' rows='5'></textarea></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Please send me the latest " +
                "product catalog:</td>");
        writer.println("<td><input type='checkbox' " +
                "name='catalogRequest'/></td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>&nbsp;</td>");
        writer.println("<td><input type='reset'/>" +
                "<input type='submit'/></td>");
        writer.println("</tr>");
        writer.println("</table>");
        writer.println("</form>");
        writer.println("</body>");
        writer.println("</html>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        PrintWriter writer = resp.getWriter();
        writer.println("<html>");
        writer.println("<head>");
        writer.println("<title>" + TITLE + "</title></head>");
        writer.println("</head>");
        writer.println("<body><h1>" + TITLE + "</h1>");
        writer.println("<table>");
        writer.println("<tr>");
        writer.println("<td>Name:</td>");
        writer.println("<td>" + req.getParameter("name")
                + "</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Address:</td>");
        writer.println("<td>" + req.getParameter("address")
                + "</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Country:</td>");
        writer.println("<td>" + req.getParameter("country")
                + "</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Shipping Instructions:</td>");
        writer.println("<td>");
        String[] instructions = req
                .getParameterValues("instruction");
        if (instructions != null) {
            for (String instruction : instructions) {
                writer.println(instruction + "<br/>");
            }
        }
        writer.println("</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Delivery Method:</td>");
        writer.println("<td>"
                + req.getParameter("deliveryMethod")
                + "</td>");
        writer.println("</tr>");
        writer.println("<tr>");
        writer.println("<td>Catalog Request:</td>");
        writer.println("<td>");
        if (req.getParameter("catalogRequest") == null) {
            writer.println("No");
        } else {
            writer.println("Yes");
        }
        writer.println("</td>");
        writer.println("</tr>");
        writer.println("</table>");
        writer.println("<div style='border:1px solid #ddd;" +
                "margin-top:40px;font-size:90%'>");

        writer.println("Debug Info<br/>");
        Enumeration<String> parameterNames = req
                .getParameterNames();
        while (parameterNames.hasMoreElements()) {
            String paramName = parameterNames.nextElement();
            writer.println(paramName + ": ");
            String[] paramValues = req
                    .getParameterValues(paramName);
            for (String paramValue : paramValues) {
                writer.println(paramValue + "<br/>");
            }
        }
        writer.println("</div>");
        writer.println("</body>");
        writer.println("</html>");
    }
}
```

## 1.11 使用部署描述符

`./src/main/java/app01c/SimpleServlet.java`
```java
package app01c;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class SimpleServlet extends HttpServlet {

    private static final long serialVersionUID = 8946L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        PrintWriter writer = resp.getWriter();
        writer.print("<html><head></head>" +
                "<body>Simple Servlet</body></html");
    }
}
```

`./src/main/java/app01c/WelcomeServlet.java`
```java
package app01c;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class WelcomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        PrintWriter writer = resp.getWriter();
        writer.print("<html><head></head>"
                + "<body>Welcome</body></html>");
    }
}
```

`./src/main/webapp/WEB-INF/web.xml`
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <servlet>
        <servlet-name>SimpleServlet</servlet-name>
        <servlet-class>app01c.SimpleServlet</servlet-class>
        <load-on-startup>10</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>SimpleServlet</servlet-name>
        <url-pattern>/simple</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>WelcomeServlet</servlet-name>
        <servlet-class>app01c.WelcomeServlet</servlet-class>
        <load-on-startup>20</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>WelcomeServlet</servlet-name>
        <url-pattern>/welcome</url-pattern>
    </servlet-mapping>

</web-app>
```
