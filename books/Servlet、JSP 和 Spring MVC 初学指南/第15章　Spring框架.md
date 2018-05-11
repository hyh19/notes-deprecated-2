[TOC]

# 第15章　Spring框架

## 15.1　Spring入门

https://spring.io/

http://projects.spring.io/spring-framework

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.3.14.RELEASE</version>
</dependency>
```

## 15.2　依赖注入

> 很多人在使用中并不区分依赖注入和控制反转（IoC），尽管Martin Fowler在其文章中已分析了二者的不同。
> 
> http://martinfowler.com/articles/injection.html

## 15.3　XML配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd">
  
</beans>
```

模块化的配置，导入其他配置。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="config1.xml"/>
    <import resource="module2/config2.xml"/>
    <import resource="/resources/config3.xml"/>
</beans>
```

## 15.4　Spring控制反转容器的使用

[Interface ApplicationContext](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)

[Class ClassPathXmlApplicationContext](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html)

http://www.springframework.org/schema/beans/spring-beans.xsd

`app15a/pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example.app</groupId>
  <artifactId>app15a</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>app15a</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>

  </dependencies>
</project>
```

`app15a/src/main/java/app15a/bean/Product.java`
```java
package app15a.bean;

import java.io.Serializable;

public class Product implements Serializable {

    private static final long serialVersionUID = 748392348L;

    private String name;
    private String description;
    private float price;

    public Product() {
    }

    public Product(String name, String description, float price) {
        this.name = name;
        this.description = description;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public float getPrice() {
        return price;
    }

    public void setPrice(float price) {
        this.price = price;
    }
}
```

`app15a/src/main/java/app15a/bean/Address.java`
```java
package app15a.bean;

public class Address {

    private String line1;
    private String line2;
    private String city;
    private String state;
    private String zipCode;
    private String country;

    public Address(String line1, String line2, String city, String state, String zipCode, String country) {
        this.line1 = line1;
        this.line2 = line2;
        this.city = city;
        this.state = state;
        this.zipCode = zipCode;
        this.country = country;
    }

    public String getLine1() {
        return line1;
    }

    public void setLine1(String line1) {
        this.line1 = line1;
    }

    public String getLine2() {
        return line2;
    }

    public void setLine2(String line2) {
        this.line2 = line2;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    public String getZipCode() {
        return zipCode;
    }

    public void setZipCode(String zipCode) {
        this.zipCode = zipCode;
    }

    public String getCountry() {
        return country;
    }

    public void setCountry(String country) {
        this.country = country;
    }

    @Override
    public String toString() {
        return "Address{" +
                "line1='" + line1 + '\'' +
                ", line2='" + line2 + '\'' +
                ", city='" + city + '\'' +
                ", state='" + state + '\'' +
                ", zipCode='" + zipCode + '\'' +
                ", country='" + country + '\'' +
                '}';
    }
}
```

`app15a/src/main/java/app15a/bean/Employee.java`
```java
package app15a.bean;

public class Employee {

    private String firstName;
    private String lastName;
    private Address homeAddress;

    public Employee() {
    }

    public Employee(String firstName, String lastName, Address homeAddress) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.homeAddress = homeAddress;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Address getHomeAddress() {
        return homeAddress;
    }

    public void setHomeAddress(Address homeAddress) {
        this.homeAddress = homeAddress;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", homeAddress=" + homeAddress +
                '}';
    }
}
```

`app15a/src/main/resources/spring-config.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="product" class="app15a.bean.Product"/>

    <bean name="featuredProduct" class="app15a.bean.Product">
        <constructor-arg name="name" value="Ultimate Olive Oil"/>
        <constructor-arg name="description" value="The purest olive oil on the market"/>
        <constructor-arg name="price" value="9.95"/>
    </bean>

    <bean id="calendar" class="java.util.Calendar" factory-method="getInstance"/>

    <bean name="simpleAddress" class="app15a.bean.Address">
        <constructor-arg name="line1" value="151 Corner Street"/>
        <constructor-arg name="line2" value=""/>
        <constructor-arg name="city" value="Albany"/>
        <constructor-arg name="state" value="NY"/>
        <constructor-arg name="zipCode" value="99999"/>
        <constructor-arg name="country" value="US"/>
    </bean>

    <bean name="employee1" class="app15a.bean.Employee">
        <property name="firstName" value="Junior"/>
        <property name="lastName" value="Moore"/>
        <property name="homeAddress" ref="simpleAddress"/>
    </bean>

    <bean name="employee2" class="app15a.bean.Employee">
        <constructor-arg name="firstName" value="Senior"/>
        <constructor-arg name="lastName" value="Moore"/>
        <constructor-arg name="homeAddress" ref="simpleAddress"/>
    </bean>

</beans>
```

`app15a/src/main/java/app15a/Main.java`
```java
package app15a;

import app15a.bean.Employee;
import app15a.bean.Product;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.Calendar;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring-config.xml"});

        Product product1 = context.getBean("product", Product.class);
        product1.setName("Excellent snake oil");
        System.out.println("product1: " + product1.getName());

        Product product2 = context.getBean("product", Product.class);
        System.out.println("product2: " + product2.getName());

        // 两个对象是同一个，说明对象被重用了。
        System.out.println(product1 == product2);

        Product featuredProduct = context.getBean("featuredProduct", Product.class);
        System.out.println(featuredProduct.getName() + ", " + featuredProduct.getDescription()
                + ", " + featuredProduct.getPrice());

        Calendar calendar = context.getBean("calendar", java.util.Calendar.class);
        System.out.println(calendar.getTime());

        Employee employee1 = context.getBean("employee1", Employee.class);
        System.out.println(employee1.getFirstName() + " " + employee1.getLastName());
        System.out.println(employee1.getHomeAddress());

        Employee employee2 = context.getBean("employee2", Employee.class);
        System.out.println(employee2.getFirstName() + " " + employee2.getLastName());
        System.out.println(employee2.getHomeAddress());
    }
}
```

学习结束