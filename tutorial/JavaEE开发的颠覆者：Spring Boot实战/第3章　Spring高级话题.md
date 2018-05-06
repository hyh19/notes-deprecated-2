# 第3章　Spring高级话题

## 3.1　Spring Aware

[Interface BeanNameAware](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/BeanNameAware.html)

[Interface ResourceLoaderAware](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/ResourceLoaderAware.html)

[Interface ResourceLoader](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/ResourceLoader.html)

[Interface Resource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/Resource.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/aware/AwareService.java`
```java
package com.wisely.highlight_spring4.ch3.aware;

import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.BeanNameAware;
import org.springframework.context.ResourceLoaderAware;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Service;

import java.io.IOException;

@Service
public class AwareService implements BeanNameAware, ResourceLoaderAware {

    private String name;
    private ResourceLoader loader;

    @Override
    public void setBeanName(String s) {
        this.name = s;
    }

    @Override
    public void setResourceLoader(ResourceLoader resourceLoader) {
        this.loader = resourceLoader;
    }

    public void outputResult() {
        System.out.println("Bean Name: " + name);

        Resource resource = loader.getResource("test.txt");
        try {
            System.out.println("test.txt: " + IOUtils.toString(resource.getInputStream()));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/aware/AwareConfig.java`
```java
package com.wisely.highlight_spring4.ch3.aware;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch3.aware")
public class AwareConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/aware/Main.java`
```java
package com.wisely.highlight_spring4.ch3.aware;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AwareConfig.class);
        AwareService awareService = context.getBean(AwareService.class);
        awareService.outputResult();
        context.close();
    }
}
```

## 3.2　多线程

[Annotation Type EnableAsync](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/annotation/EnableAsync.html)

[Interface AsyncConfigurer](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/annotation/AsyncConfigurer.html)

[Class ThreadPoolTaskExecutor](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html)

[Annotation Type Async](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/annotation/Async.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskexecutor/TaskExecutorConfig.java`
```java
package com.wisely.highlight_spring4.ch3.taskexecutor;

import org.springframework.aop.interceptor.AsyncUncaughtExceptionHandler;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.AsyncConfigurer;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

import java.util.concurrent.Executor;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch3.taskexecutor")
@EnableAsync
public class TaskExecutorConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(25);
        executor.initialize();
        return executor;
    }

    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return null;
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskexecutor/AsyncTaskService.java`
```java
package com.wisely.highlight_spring4.ch3.taskexecutor;

import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class AsyncTaskService {

    @Async
    public void executeAsyncTask(Integer i) {
        System.out.println(i);
    }

    @Async
    public void executeAsyncTaskPlus(Integer i) {
        System.out.println("Plus: " + (i+1));
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskexecutor/Main.java`
```java
package com.wisely.highlight_spring4.ch3.taskexecutor;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(TaskExecutorConfig.class);
        AsyncTaskService asyncTaskService = context.getBean(AsyncTaskService.class);
        for (int i = 0; i < 10; i++) {
            asyncTaskService.executeAsyncTask(i);
            asyncTaskService.executeAsyncTaskPlus(i);
        }
        context.close();
    }
}
```

## 3.3　计划任务

[Annotation Type EnableScheduling](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/annotation/EnableScheduling.html)

[Annotation Type Scheduled](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskscheduler/TaskSchedulerConfig.java`
```java
package com.wisely.highlight_spring4.ch3.taskscheduler;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableScheduling;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch3.taskscheduler")
@EnableScheduling
public class TaskSchedulerConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskscheduler/ScheduledTaskService.java`
```java
package com.wisely.highlight_spring4.ch3.taskscheduler;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Service;

import java.text.SimpleDateFormat;
import java.util.Date;

@Service
public class ScheduledTaskService {

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println(dateFormat.format(new Date()));
    }

    @Scheduled(cron = "0 8 15 ? * *")
    public void fixTimeExecution() {
        System.out.println("Hello World");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/taskscheduler/Main.java`
```java
package com.wisely.highlight_spring4.ch3.taskscheduler;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(TaskSchedulerConfig.class);
    }
}
```

## 3.4　条件注解@Conditional

[Interface Condition](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Condition.html)

[Interface ConditionContext](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/ConditionContext.html)

[Annotation Type Conditional](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Conditional.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/WindowsCondition.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class WindowsCondition implements Condition {
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        return conditionContext.getEnvironment().getProperty("os.name").contains("Windows");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/LinuxCondition.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class LinuxCondition implements Condition {
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        return conditionContext.getEnvironment().getProperty("os.name").contains("Mac");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/ListService.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

public interface ListService {
    public String showListCmd();
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/WindowsListService.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

public class WindowsListService implements ListService {
    @Override
    public String showListCmd() {
        return "dir";
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/LinuxListService.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

public class LinuxListService implements ListService {
    @Override
    public String showListCmd() {
        return "ls";
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/ConditionConifg.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ConditionConifg {

    @Bean
    @Conditional(WindowsCondition.class)
    public ListService windowsListService() {
        return new WindowsListService();
    }

    @Bean
    @Conditional(LinuxCondition.class)
    public ListService linuxListService() {
        return new LinuxListService();
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/conditional/Main.java`
```java
package com.wisely.highlight_spring4.ch3.conditional;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConditionConifg.class);
        ListService listService = context.getBean(ListService.class);
        System.out.println(context.getEnvironment().getProperty("os.name") + ": " + listService.showListCmd());
        context.close();
    }
}
```

## 3.5　组合注解与元注解

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/annotation/WiselyConfiguration.java`
```java
package com.wisely.highlight_spring4.ch3.annotation;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import java.lang.annotation.*;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
@ComponentScan
public @interface WiselyConfiguration {
    String[] value() default {};
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/annotation/DemoConfig.java`
```java
package com.wisely.highlight_spring4.ch3.annotation;

@WiselyConfiguration("com.wisely.highlight_spring4.ch3.annotation")
public class DemoConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/annotation/DemoService.java`
```java
package com.wisely.highlight_spring4.ch3.annotation;

import org.springframework.stereotype.Service;

@Service
public class DemoService {
    public void outputResult() {
        System.out.println("Hello World");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch3/annotation/Main.java`
```java
package com.wisely.highlight_spring4.ch3.annotation;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context =
                new AnnotationConfigApplicationContext(DemoConfig.class);
        DemoService demoService = context.getBean(DemoService.class);
        demoService.outputResult();
        context.close();
    }
}
```

## 3.6　@Enable*注解的工作原理

跳过

## 3.7　测试

跳过




