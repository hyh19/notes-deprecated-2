# [Vert.x core examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples)

运行示例代码的两种方式

- 在 IDE 上执行 `main` 方法
- 在终端上执行以下命令

```bash
mvn clean compile
vertx run fully-qualified-name-of-the-example -cp target/classes
```

---

[TOC]

## [Dependencies required](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#dependencies-required)

Maven 依赖 [`pom.xml`](https://github.com/vert-x3/vertx-examples/blob/master/core-examples/pom.xml)

```xml
<dependency>
  <groupId>io.vertx</groupId>
  <artifactId>vertx-core</artifactId>
  <version>3.5.1</version>
</dependency>
```

## [Embedding](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#embedding)

```java
package io.vertx.example.core.embed;

import io.vertx.core.Vertx;

public class EmbeddedServer {
    public static void main(String[] args) {
        Vertx.vertx().createHttpServer().requestHandler(req -> req.response().end("Hello World!")).listen(8080);
    }
}
```

Java API

- [Interface Vertx](http://vertx.io/docs/apidocs/io/vertx/core/Vertx.html)
- [Interface HttpServer](http://vertx.io/docs/apidocs/io/vertx/core/http/HttpServer.html)
- [Interface Handler<E>](http://vertx.io/docs/apidocs/io/vertx/core/Handler.html)
- [Interface HttpServerRequest](http://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerRequest.html)
- [Interface HttpServerResponse](http://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerResponse.html)
- [Interface ReadStream<T>](http://vertx.io/docs/apidocs/io/vertx/core/streams/ReadStream.html)
- [Interface WriteStream<T>](http://vertx.io/docs/apidocs/io/vertx/core/streams/WriteStream.html)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/embed)

```java
Vertx.vertx().createHttpServer().requestHandler(req -> req.response().end("Hello World!")).listen(8080);
```

## Net examples

跳过

## [HTTP examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#http-examples)

### [Simple](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#simple)

```java
package io.vertx.example.core.http.simple;

import io.vertx.core.AbstractVerticle;
import io.vertx.core.Vertx;

public class Server extends AbstractVerticle {

    public static void main(String[] args) {
        Vertx.vertx().deployVerticle(new Server());
    }

    @Override
    public void start() throws Exception {
        vertx.createHttpServer().requestHandler(req -> {
            req.response().putHeader("content-type", "text/html").end("<html><body><h1>Hello from vert.x!</h1></body></html>");
        }).listen(8080);
    }
}
```

Java API

- [Class AbstractVerticle](https://vertx.io/docs/apidocs/io/vertx/core/AbstractVerticle.html)
- [Interface Verticle](https://vertx.io/docs/apidocs/io/vertx/core/Verticle.html)
- [Interface Future<T>](https://vertx.io/docs/apidocs/io/vertx/core/Future.html)
- [Interface AsyncResult<T>](http://vertx.io/docs/apidocs/io/vertx/core/AsyncResult.html)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/http/simple)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/http/simple)

### HTTPS

跳过

### Proxy connect

跳过

### Proxy

跳过

### [Sendfile](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#sendfile)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/http/sendfile)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/http/sendfile)

### [Simple form](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#simple-form)

Java API

- [Interface MultiMap](http://vertx.io/docs/apidocs/io/vertx/core/MultiMap.html)
- [setChunked](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerResponse.html#setChunked-boolean-)
- [setExpectMultipart](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerRequest.html#setExpectMultipart-boolean-)
- [endHandler](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerRequest.html#endHandler-io.vertx.core.Handler-)
- [formAttributes](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerRequest.html#formAttributes--)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/http/simpleform)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/http/simpleform)

### [Simple form file upload](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#simple-form-file-upload)

Java API

- [Interface HttpServerFileUpload](http://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerFileUpload.html)
- [uploadHandler](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerRequest.html#uploadHandler-io.vertx.core.Handler-)
- [exceptionHandler](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerFileUpload.html#exceptionHandler-io.vertx.core.Handler-)
- [endHandler](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerFileUpload.html#endHandler-io.vertx.core.Handler-)
- [streamToFileSystem](https://vertx.io/docs/apidocs/io/vertx/core/http/HttpServerFileUpload.html#streamToFileSystem-java.lang.String-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/http/simpleformupload)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/http/simpleformupload)

### [Http request body upload](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#http-request-body-upload)

Java API

- [Interface FileSystem](http://vertx.io/docs/apidocs/io/vertx/core/file/FileSystem.html)
- [Interface AsyncFile](http://vertx.io/docs/apidocs/io/vertx/core/file/AsyncFile.html)
- [Interface Pump](http://vertx.io/docs/apidocs/io/vertx/core/streams/Pump.html)
- [open](https://vertx.io/docs/apidocs/io/vertx/core/file/FileSystem.html#open-java.lang.String-io.vertx.core.file.OpenOptions-io.vertx.core.Handler-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/http/upload)

Kotlin Code 跳过

> network io 网络读写

## [Event bus examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#event-bus-examples)

### [Point to point](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#point-to-point)

Java API

- [Interface EventBus](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/EventBus.html)
- [Interface Message<T>](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/Message.html)
- [Interface MessageConsumer<T>](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/MessageConsumer.html)
- [send](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/EventBus.html#send-java.lang.String-java.lang.Object-io.vertx.core.Handler-)
- [consumer](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/EventBus.html#consumer-java.lang.String-io.vertx.core.Handler-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/eventbus/pointtopoint)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/eventbus/pointtopoint)

注意：在命令行运行要加上 `-cluster` 选项，接下来的几个示例不再提示。

```bash
vertx run Receiver.kt -cluster
vertx run Sender.kt -cluster
```

### [Publish / Subscribe](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#publish--subscribe)

Java API

- [publish](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/EventBus.html#publish-java.lang.String-java.lang.Object-)

[Java Code](https://github.com/vert-x3/vertx-examples/blob/master/core-examples/src/main/java/io/vertx/example/core/eventbus/pubsub)

[Kotlin Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/kotlin/io/vertx/example/core/eventbus/pubsub)

### [MessageCodec](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#messagecodec)

Java API

- [Interface MessageCodec<S,R>](https://vertx.io/docs/apidocs/io/vertx/core/eventbus/MessageCodec.html)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/eventbus/messagecodec)

## [Future](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#future)

Java API

- [compose](https://vertx.io/docs/apidocs/io/vertx/core/Future.html#compose-java.util.function.Function-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/future)

## [Verticle examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#verticle-examples)

### [Deploy example](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#deploy-example)

```java
public class DeployExample extends AbstractVerticle {

  @Override
  public void start() throws Exception {

    // 无部署结果的回调
    vertx.deployVerticle("io.vertx.example.core.verticle.deploy.OtherVerticle");

    // 有部署结果的回调
    vertx.deployVerticle("io.vertx.example.core.verticle.deploy.OtherVerticle", res -> {
      if (res.succeeded()) {
        // 成功
      } else {
        // 失败
      }
    });

    // 自定义部署配置
    JsonObject config = new JsonObject().put("foo", "bar");
    vertx.deployVerticle("io.vertx.example.core.verticle.deploy.OtherVerticle", new DeploymentOptions().setConfig(config));

    // 部署多个实例
    vertx.deployVerticle("io.vertx.example.core.verticle.deploy.OtherVerticle", new DeploymentOptions().setInstances(10));

    // 以 Worker 的形式部署
    vertx.deployVerticle("io.vertx.example.core.verticle.deploy.OtherVerticle", new DeploymentOptions().setWorker(true));
  }
}
```

Java API

- [Class DeploymentOptions](https://vertx.io/docs/apidocs/io/vertx/core/DeploymentOptions.html)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/verticle/deploy)

Kotlin Code 跳过

### [Asynchronous deployment example](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#asynchronous-deployment-example)

```java
public class OtherVerticle extends AbstractVerticle {

  @Override
  public void start(Future<Void> startFuture) throws Exception {

    // 执行耗时代码

    // 执行完成，返回结果。
    startFuture.complete();
  }

  @Override
  public void stop(Future<Void> stopFuture) throws Exception {
    // 同上
    stopFuture.complete();
  }
}
```

Java API

- [start](https://vertx.io/docs/apidocs/io/vertx/core/AbstractVerticle.html#start-io.vertx.core.Future-)
- [stop](https://vertx.io/docs/apidocs/io/vertx/core/AbstractVerticle.html#stop-io.vertx.core.Future-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/verticle/asyncstart)

Kotlin Code 跳过

### [Worker Verticle example](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#worker-verticle-example)

```java
vertx.deployVerticle("io.vertx.example.core.verticle.worker.WorkerVerticle", new DeploymentOptions().setWorker(true));
```

Java API

- [setWorker](https://vertx.io/docs/apidocs/io/vertx/core/DeploymentOptions.html#setWorker-boolean-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/verticle/worker)

**TODO** 对 Event Loop 和 Worker Pool 两个线程以后要深入了解

## [Execute blocking example](https://github.com/vert-x3/vertx-examples/tree/master/core-examples#execute-blocking-example)

```java
vertx.<String>executeBlocking(future -> {

  // 执行阻塞代码

  // 执行完成，返回结果。
  future.complete(result);

}, res -> { // 执行结果的回调

  if (res.succeeded()) {
    // 成功
  } else {
    // 失败
  }
});
```

Java API

- [executeBlocking](https://vertx.io/docs/apidocs/io/vertx/core/Vertx.html#executeBlocking-io.vertx.core.Handler-io.vertx.core.Handler-)
- [setWorkerPoolName](https://vertx.io/docs/apidocs/io/vertx/core/DeploymentOptions.html#setWorkerPoolName-java.lang.String-)
- [setMaxWorkerExecuteTime](https://vertx.io/docs/apidocs/io/vertx/core/DeploymentOptions.html#setMaxWorkerExecuteTime-long-)
- [setWorkerPoolSize](https://vertx.io/docs/apidocs/io/vertx/core/DeploymentOptions.html#setWorkerPoolSize-int-)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/core-examples/src/main/java/io/vertx/example/core/execblocking)