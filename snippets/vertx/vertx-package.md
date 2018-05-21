# Vert.x Package

@(Vert.x)[vertx,snippet]

## Maven

```xml
<properties>
    <main.verticle>com.example.MainVerticle</main.verticle>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <manifestEntries>
                                    <Main-Class>io.vertx.core.Launcher</Main-Class>
                                    <Main-Verticle>${main.verticle}</Main-Verticle>
                                </manifestEntries>
                            </transformer>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                <resource>META-INF/services/io.vertx.core.spi.VerticleFactory</resource>
                            </transformer>
                        </transformers>
                        <outputFile>${project.build.directory}/${project.artifactId}-${project.version}-fat.jar
                        </outputFile>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```