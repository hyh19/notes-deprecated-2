# 第 10 章 修饰 Requests 及 Responses

## 10.2 Servlet 封装类

[Class ServletRequestWrapper](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequestWrapper.html)

[Class HttpServletRequestWrapper](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequestWrapper.html)

[Class ServletResponseWrapper](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletResponseWrapper.html)

[Class HttpServletResponseWrapper](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletResponseWrapper.html)

## 10.3 示例：AutoCorrect Filter

`app10a/pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.app</groupId>
  <artifactId>app10a</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>app10a Maven Webapp</name>
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
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>app10a</finalName>
  </build>
</project>
```

`app10a/src/main/java/filter/AutoCorrectFilter.java`
```java
package filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collection;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

@WebFilter(filterName = "AutoCorrectFilter", urlPatterns = {"/*"})
public class AutoCorrectFilter implements Filter {
    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) req;
        AutoCorrectHttpServletRequestWrapper wrapper = new AutoCorrectHttpServletRequestWrapper(httpServletRequest);
        chain.doFilter(wrapper, resp);
    }

    public void init(FilterConfig config) throws ServletException {

    }

    class AutoCorrectHttpServletRequestWrapper extends HttpServletRequestWrapper {

        private HttpServletRequest httpServletRequest;

        public AutoCorrectHttpServletRequestWrapper(HttpServletRequest httpServletRequest) {
            super(httpServletRequest);
            this.httpServletRequest = httpServletRequest;
        }

        @Override
        public String getParameter(String name) {
            return autoCorrect(httpServletRequest.getParameter(name));
        }

        @Override
        public Map<String, String[]> getParameterMap() {
            final Map<String, String[]> parameterMap =
                    httpServletRequest.getParameterMap();

            Map<String, String[]> newMap = new Map<String,
                    String[]>() {

                public int size() {
                    return parameterMap.size();
                }

                public boolean isEmpty() {
                    return parameterMap.isEmpty();
                }

                public boolean containsKey(Object key) {
                    return parameterMap.containsKey(key);
                }

                public boolean containsValue(Object value) {
                    return parameterMap.containsValue(value);
                }

                public String[] get(Object key) {
                    return autoCorrect(parameterMap.get(key));
                }

                public void clear() {
                    // this will throw an IllegalStateException,
                    // but let the user get the original
                    // exception
                    parameterMap.clear();
                }

                public Set<String> keySet() {
                    return parameterMap.keySet();
                }

                public Collection<String[]> values() {
                    return autoCorrect(parameterMap.values());
                }

                public Set<Map.Entry<String,
                        String[]>> entrySet() {
                    return autoCorrect(parameterMap.entrySet());
                }

                public String[] put(String key, String[] value) {
                    // this will throw an IllegalStateException,
                    // but let the user get the original
                    // exception
                    return parameterMap.put(key, value);
                }

                public void putAll(
                        Map<? extends String, ? extends
                                String[]> map) {
                    // this will throw an IllegalStateException,
                    // but let
                    // the user get the original exception
                    parameterMap.putAll(map);
                }

                public String[] remove(Object key) {
                    // this will throw an IllegalStateException,
                    // but let
                    // the user get the original exception
                    return parameterMap.remove(key);
                }
            };
            return newMap;
        }

        @Override
        public String[] getParameterValues(String name) {
            return autoCorrect(httpServletRequest.getParameterValues(name));
        }
    }

    private String autoCorrect(String value) {
        if (value == null) {
            return null;
        }
        value = value.trim();
        int length = value.length();
        StringBuilder temp = new StringBuilder();
        boolean lastCharWasSpace = false;
        for (int i = 0; i < length; i++) {
            char c = value.charAt(i);
            if (c == ' ') {
                if (!lastCharWasSpace) {
                    temp.append(c);
                }
                lastCharWasSpace = true;
            } else {
                temp.append(c);
                lastCharWasSpace = false;
            }
        }
        return temp.toString();
    }

    private String[] autoCorrect(String[] values) {
        if (values != null) {
            int length = values.length;
            for (int i = 0; i < length; i++) {
                values[i] = autoCorrect(values[i]);
            }
            return values;
        }
        return null;
    }

    private Collection<String[]> autoCorrect(
            Collection<String[]> valueCollection) {
        Collection<String[]> newCollection =
                new ArrayList<String[]>();
        for (String[] values : valueCollection) {
            newCollection.add(autoCorrect(values));
        }
        return newCollection;
    }

    private Set<Map.Entry<String, String[]>> autoCorrect(
            Set<Map.Entry<String, String[]>> entrySet) {
        Set<Map.Entry<String, String[]>> newSet = new
                HashSet<Map.Entry<String, String[]>>();
        for (final Map.Entry<String, String[]> entry
                : entrySet) {
            Map.Entry<String, String[]> newEntry = new
                    Map.Entry<String, String[]>() {
                        public String getKey() {
                            return entry.getKey();
                        }

                        public String[] getValue() {
                            return autoCorrect(entry.getValue());
                        }

                        public String[] setValue(String[] value) {
                            return entry.setValue(value);
                        }
                    };
            newSet.add(newEntry);
        }
        return newSet;
    }

}
```

`app10a/src/main/webapp/test1.jsp`
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>User Form</title>
</head>
<body>
    <form action="test2.jsp" method="post">
        <table>
            <tr>
                <td>Name:</td>
                <td><input name="name"/></td>
            </tr>
            <tr>
                <td>Address:</td>
                <td><input name="address"/></td>
            </tr>
            <tr>
                <td colspan="2">
                    <input type="submit" value="Login"/>
                </td>
            </tr>
        </table>
    </form>
</body>
</html>
```

`app10a/src/main/webapp/test2.jsp`
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<html>
<head>
    <title>Form Values</title>
</head>
<body>
    <table>
        <tr>
            <td>Name:</td>
            <td>
                ${param.name}
                (length:${fn:length(param.name)})
            </td>
        </tr>
        <tr>
            <td>Address:</td>
            <td>
                ${param.address}
                (length:${fn:length(param.address)})
            </td>
        </tr>
    </table>
</body>
</html>
```


