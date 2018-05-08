# Vert.x Manual

<http://vertx.io/>

- [Vert.x Manual](#vertx-manual)
    - [Installation >>](#installation)
        - [NPM](#npm)
        - [SDKMan](#sdkman)
        - [Homebrew](#homebrew)
    - [Commands](#commands)
    - [Core](#core)
    - [Web](#web)
    - [Services >>](#services)
    - [Codegen >>](#codegen)
    - [Kafka Client >>](#kafka-client)
    - [Reactive](#reactive)
        - [Kotlin coroutines](#kotlin-coroutines)
    - [Tutorials](#tutorials)
    - [Examples](#examples)
    - [Tools](#tools)

## Installation [>>](https://vertx.io/download/)

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

[Examples](https://github.com/vert-x3/vertx-examples/tree/master/core-examples)

Java [Manual](https://vertx.io/docs/vertx-core/java/) [API](https://vertx.io/docs/apidocs/)

## Web

<https://vertx.io/docs/#web>

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