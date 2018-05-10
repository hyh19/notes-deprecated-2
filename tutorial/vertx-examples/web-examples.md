# [Vert.x-Web examples](https://github.com/vert-x3/vertx-examples/tree/master/web-examples)

@(Vert.x)[vertx,example]

---

[TOC]

## [Logging](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#logging)

配置文件 `src/main/resources/vertx-default-jul-logging.properties`

## [Dependencies required](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#dependencies-required)

Maven 依赖 [`pom.xml`](https://github.com/vert-x3/vertx-examples/blob/master/web-examples/pom.xml)

```xml
<dependency>
  <groupId>io.vertx</groupId>
  <artifactId>vertx-web</artifactId>
  <version>3.5.1</version>
</dependency>
```

## [Hello World](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#hello-world)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/helloworld)

```java
// 创建路由器
Router router = Router.router(vertx);

// 添加路由
router.route().handler(routingContext -> {
  routingContext.response().putHeader("content-type", "text/html").end("Hello World!");
});

// 指定某个路由器来处理网络请求
vertx.createHttpServer().requestHandler(router::accept).listen(8080);
```

Java API

- [Interface Route](https://vertx.io/docs/apidocs/io/vertx/ext/web/Route.html)
- [Interface Router](https://vertx.io/docs/apidocs/io/vertx/ext/web/Router.html)
- [Interface RoutingContext](https://vertx.io/docs/apidocs/io/vertx/ext/web/RoutingContext.html)

## Simple REST Micro-service [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#simple-rest-micro-service)

[Java Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/rest)

```java
// 传入一个请求体处理器
router.route().handler(BodyHandler.create());

// 添加多个路由
router.get("/products/:productID").handler(this::handleGetProduct);
router.put("/products/:productID").handler(this::handleAddProduct);
router.get("/products").handler(this::handleListProducts);

// 读取路径变量的值
String productID = routingContext.request().getParam("productID");

// 读取请求体的数据
JsonObject product = routingContext.getBodyAsJson();
```

Java API

- [Interface BodyHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/BodyHandler.html)
- [Class JsonArray](https://vertx.io/docs/apidocs/io/vertx/core/json/JsonArray.html)
- [encodePrettily](https://vertx.io/docs/apidocs/io/vertx/core/json/JsonObject.html#encodePrettily--)
- [getBodyAsJson](https://vertx.io/docs/apidocs/io/vertx/ext/web/RoutingContext.html#getBodyAsJson--)

复习到这

## Sessions example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#sessions-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/sessions)

**API**

- [Interface Session](https://vertx.io/docs/apidocs/io/vertx/ext/web/Session.html)
- [Interface CookieHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/CookieHandler.html)
- [Interface SessionHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/SessionHandler.html)
- [Interface LocalSessionStore](https://vertx.io/docs/apidocs/io/vertx/ext/web/sstore/LocalSessionStore.html)
- [Interface SessionStore](https://vertx.io/docs/apidocs/io/vertx/ext/web/sstore/SessionStore.html)

## Cookie example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#cookie-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/cookie)

**API**

- [Interface CookieHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/CookieHandler.html)
- [Interface StaticHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/StaticHandler.html)

## Upload example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#upload-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/upload)

**API**

- [Interface FileUpload](https://vertx.io/docs/apidocs/io/vertx/ext/web/FileUpload.html)
- [setUploadsDirectory](https://vertx.io/docs/apidocs/io/vertx/ext/web/handler/BodyHandler.html#setUploadsDirectory-java.lang.String-)

## HTML Form example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#html-form-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/form)

**API**

- [Class HttpHeaders](http://vertx.io/docs/apidocs/io/vertx/core/http/HttpHeaders.html)

## Blocking handler example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#blocking-handler-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/blockinghandler)

**API**

- [blockingHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/Route.html#blockingHandler-io.vertx.core.Handler-boolean-)

## Static web server example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#static-web-server-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/staticsite)

## JDBC example [>>](https://github.com/vert-x3/vertx-examples/tree/master/web-examples#jdbc-example)

[Source Code](https://github.com/vert-x3/vertx-examples/tree/master/web-examples/src/main/java/io/vertx/example/web/jdbc)

**API**

- [addHeadersEndHandler](https://vertx.io/docs/apidocs/io/vertx/ext/web/RoutingContext.html#addHeadersEndHandler-io.vertx.core.Handler-)
- [next](https://vertx.io/docs/apidocs/io/vertx/ext/web/RoutingContext.html#next--)



