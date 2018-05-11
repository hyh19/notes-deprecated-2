# Vert.x Manual

@(Vert.x)[vertx,manual]

<http://vertx.io/>

---

[TOC]

## [Installation](https://vertx.io/download/)

### NPM

```bash
npm install vertx3-min
npm install vertx3-full
```

### SDKMan

```bash
sdk install vertx
```

### Homebrew

```bash
brew install vert.x
```

## Commands

```bash
cd core-examples/src/main/java/io/vertx/example/core
vertx run EchoServer.java

cd core-examples/src/main/js/echo
vertx run echo_server.js
```

## [Core](https://vertx.io/docs/#core)

```xml
<dependency>
  <groupId>io.vertx</groupId>
  <artifactId>vertx-core</artifactId>
  <version>3.5.1</version>
</dependency>
```

```gradle
dependencies {
  compile 'io.vertx:vertx-core:3.5.1'
}
```

[Java Manual](https://vertx.io/docs/vertx-core/java/) [API](https://vertx.io/docs/apidocs/)

[Examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples)

## [Web](https://vertx.io/docs/#web)

### Web

```xml
<dependency>
  <groupId>io.vertx</groupId>
  <artifactId>vertx-web</artifactId>
  <version>3.5.1</version>
</dependency>
```

```gradle
dependencies {
  compile 'io.vertx:vertx-web:3.5.1'
}
```

[Java Manual](https://vertx.io/docs/vertx-web/java/)

[Examples](https://github.com/vert-x3/vertx-examples/tree/master/web-examples)

## [Data access](https://vertx.io/docs/#data_access)

### JDBC client

[Java Manual](https://vertx.io/docs/vertx-jdbc-client/java/)

[Examples](https://github.com/vert-x3/vertx-examples/tree/master/jdbc-examples)

### SQL common

[Java Manual](https://vertx.io/docs/vertx-sql-common/java/)

## Services [>>](https://vertx.io/docs/#services)

[Manual](https://vertx.io/docs/vertx-service-proxy/java) \
[Examples](https://github.com/vert-x3/vertx-examples/tree/master/service-proxy-examples)

## Codegen [>>](https://github.com/vert-x3/vertx-codegen)

Render documentation

```bash
mvn clean package -Pdocs
open target/docs/vertx-codegen/java/index.html
```

## Kafka Client [>>](https://vertx.io/docs/vertx-kafka-client/java/)

<https://github.com/vert-x3/vertx-examples/tree/master/kafka-examples>

## Reactive

### Kotlin coroutines

[Manual](https://vertx.io/docs/vertx-lang-kotlin-coroutines/kotlin/)

[Examples](https://github.com/vert-x3/vertx-examples/tree/master/kotlin-examples/coroutines)

[Soruce](https://github.com/vert-x3/vertx-lang-kotlin/tree/master/vertx-lang-kotlin-coroutines)

## Tutorials

<https://vertx.io/materials/>

<https://github.com/vert-x3/vertx-awesome>

[Vert.x 官方文档中文翻译](https://vertxchina.github.io/vertx-translation-chinese/)

## Examples

<https://github.com/vert-x3/vertx-examples>

## Tools

<https://github.com/vert-x3/vertx-maven-starter>