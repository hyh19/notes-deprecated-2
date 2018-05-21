# [Exec Maven Plugin](https://www.mojohaus.org/exec-maven-plugin/)

---

[TOC]

## [Usage](https://www.mojohaus.org/exec-maven-plugin/usage.html)

### Exec goal

```xml
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.6.0</version>
        <executions>
          <execution>
            ...
            <goals>
              <goal>exec</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <executable>maven</executable>
          <!-- optional -->
          <workingDirectory>/tmp</workingDirectory>
          <arguments>
            <argument>-X</argument>
            <argument>myproject:dist</argument>
            ...
          </arguments>
        </configuration>
      </plugin>
    </plugins>
  </build>
   ...
</project>
```

### Java goal

```xml
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.6.0</version>
        <executions>
          <execution>
            ...
            <goals>
              <goal>java</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <mainClass>com.example.Main</mainClass>
          <arguments>
            <argument>argument1</argument>
            ...
          </arguments>
          <systemProperties>
            <systemProperty>
              <key>myproperty</key>
              <value>myvalue</value>
            </systemProperty>
            ...
          </systemProperties>
        </configuration>
      </plugin>
    </plugins>
  </build>
   ...
</project>
```

## [Goals](https://www.mojohaus.org/exec-maven-plugin/plugin-info.html)

### [`exec:exec`](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)

运行可执行程序 如 `java com.example.Main`

配置参数 | 说明
--- | ---
[`arguments`](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html#arguments) | 对应命令行中的参数
[`executable`](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html#executable) | 对应命令行中的可执行程序

### [`exec:java`](https://www.mojohaus.org/exec-maven-plugin/java-mojo.html)

运行 Java 类

配置参数 | 说明
--- | ---
[`mainClass`](https://www.mojohaus.org/exec-maven-plugin/java-mojo.html#mainClass) | 要运行的 Java 类
[`arguments`](https://www.mojohaus.org/exec-maven-plugin/java-mojo.html#arguments) | Java 类的参数

## Examples

### [Running Java programs with the exec goal](https://www.mojohaus.org/exec-maven-plugin/examples/example-exec-for-java-programs.html)

```xml
<configuration>
  <executable>java</executable>
  <arguments>
    <argument>-classpath</argument>
    <!-- automatically creates the classpath using all project dependencies,
         also adding the project build directory -->
    <classpath/>
    <argument>com.example.Main</argument>
    ...
  </arguments>
</configuration>
```

相当于命令行

```bash
java -classpath ... com.example.Main
```