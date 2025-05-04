---
theme: default
background: /cover.png
title: Four Approaches to Reducing Java Startup Time
layout: cover
class: text-center
drawings:
  persist: false
selectable: true
remoteAssets: true
canvasWidth: 800
colorSchema: 'dark'
---

## Four Approaches to Reducing Java Startup Time:

## AppCDS, Native Image, Project Leyden, CRaC

<img src="/bellsoft.png" width="200px" class="absolute right-10px bottom-5px"/>

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>

---
layout: image-right
image: '/cat.jpg'
---

# Catherine Edelveis
<img/>
<v-clicks>
🥑 Developer Advocate at BellSoft

😍Love Java, Spring, JavaFX

👩‍💻Tech writer

<span><line-md-twitter-x /> cat_edelveis</span>

<span><logos-bluesky /> cat-edelveis.bsky.social</span>

</v-clicks>


---
layout: image-left
image: '/pasha.jpeg'
---

# Pasha Finkelshteyn
<img/>
<v-clicks>

🥑 Developer Advocate at BellSoft

≈15 years in JVM, mostly <logos-java /><logos-kotlin-icon/><logos-spring-icon/>

<span><logos-bluesky /> @asm0dey.site</span>

<span><line-md-twitter-x /> asm0di0</span>

</v-clicks>

---
class: text-center
layout: cover
background: /cat_pasha.png
---

# Together, we are...

## <v-click>The BellSoft's DevRel Gang!</v-click>

<style>
h1 {
    position: relative;
    bottom: 100px;
}
h2 {
    font-size: 30px;
    position: relative;
    bottom: 50px;
}
</style>

---
layout: image-right
image: '/qr.png'
---

# About BellSoft

* Liberica JDK Vendor
* OpenJDK Contributor
* <simple-icons-cncf/> <simple-icons-linuxfoundation/> Member
* Alpaquita Linux Developer
* ARM32 Java Port Author
* Own base images

<br/>
Liberica is the JDK officially recommended by <logos-spring-icon />


---
class: text-center
layout: cover
background: /Bg-1.png
---

# Enough about us :)

## We are here today to tell you a story about...
<v-click>you!</v-click>

<style>
h1 {
    position: relative;
    bottom: 100px;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    color: #FFFFFF;
    font-size: 36px;
    position: relative;
    bottom: 30px;
}
div {
        font-size: 36px;
}
</style>

---
class: text-center
layout: cover
background: /Bg-1.png
---

# But first, four quick questions ;)

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows one way to reduce the startup of their Java application?

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>


---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows two ways to reduce the startup of their Java application?

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>


---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows three ways to reduce the startup of their Java application?

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>


---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows four ways to reduce the startup of their Java application?

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Once upon a time, all was well in one enterprise...

<style>
h1 {
    font-size: 30px;
    line-height: 1.2;
}
</style>


---
layout: image
image: /Bg-8.png
---

# Our startup is doing well!

<br/>

- Java 21 LTS
- Spring Boot 3.4
- Spring AI
- MongoDB, Redis
- JTE
- Deploying to the cloud

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 20px;
}
</style>

---
layout: image
image: /Bg-8.png
---

# Starting point: `Dockerfile`

<br/>

```docker {none|1|8|10|17|14|all}{maxHeight:'300px'}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true package

FROM bellsoft/liberica-runtime-container:jre-21-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
COPY --from=builder /app/${project}/target/*.jar /app/app.jar
```


---
layout: image
image: /Bg-8.png
---

# Starting point: Startup, Mem usage, image size

<br/>

```bash {1|2|3|all}
docker compose logs chat-api bot-assistant | grep Started
Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```

---
layout: image
image: /Bg-8.png
---

# Starting point: Startup, Mem usage, image size

<br/>

```bash {1|3|4|all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%
```

---
layout: image
image: /Bg-8.png
---

# Starting point: Startup, Mem usage, image size

<br/>

```bash {1|3|4|all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```

---
layout: image
image: /Bg-8.png
---

# We are growing!

<br/>

- So many new users!
- Scale the services to meet the traffic

<br/>

<div v-click class="text-xl"> But wait... </div>
<br/>
<div v-click class="text-xl"> Why are we paying so much? </div>


---
layout: image
image: /Bg-8.png
---

# Something is rotten in our cloud infrastructure!
<br/>
<v-clicks>

Inflated cloud bills: management is not happy

Services eat away at resources

The longer the start, the slower the rollout

<br/>
<b> Maybe Java is not cut out for the cloud? </b>

<b> Maybe it's not too late to migrate to Go? </b>
</v-clicks>


---
class: text-center
layout: cover
background: /Bg-1.png
---

# Four heroes, one story

---
layout: image
image: /Bg-8.png
---

> # I want painless integration with no incompatibilities!

<br/>

The Defender

<style>
h1 {
font-size: 40px;
color: #FFFFFF;
}
div {
    font-size: 24px;
    text-align: right;
}
</style>

---
layout: image
image: /Bg-12.png
---

# Problem


- Application uses thousands of classes
- Classes are loaded every time the app starts
- Can take several seconds
- The process repeats upon each start!

<br/>
<v-click><b>DRY!</b></v-click>



---
layout: image
image: /Bg-12.png
---

# Solution: AppCDS


- Archive of JVM and application classes
- Created dynamically upon app exit
- Supported since Spring Boot 3.3
- Even better with Spring AOT <br/> (load even more classes!)


---
layout: image
image: /Bg-12.png
---

# AppCDS: Any Considerations?


- Use the same JVM
- Use the same classpath
- Bigger Docker image due to the archive

<br/>

<v-click>Let's take it for a spin!</v-click>


---

```xml {all|7,8,9}
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>process-aot</id>
            <goals>
                <goal>process-aot</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

---


```docker {1,9|11,16,17|20,26-29|30,31|24,25}{maxHeight:'300px'}
FROM bellsoft/liberica-runtime-container:jdk-21.0.7_9-stream-musl as builder

ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-21.0.7_9-cds-musl as optimizer
ARG project
ENV project=${project}

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --destination extracted


FROM bellsoft/liberica-runtime-container:jre-21.0.7_9-cds-musl

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-Dspring.aot.enabled=true", "-XX:SharedArchiveFile=application.jsa", \
"-jar", "/app/app.jar"]
COPY --from=optimizer /app/extracted/dependencies/ ./
COPY --from=optimizer /app/extracted/spring-boot-loader/ ./
COPY --from=optimizer /app/extracted/snapshot-dependencies/ ./
COPY --from=optimizer /app/extracted/application/ ./
RUN java -Dspring.aot.enabled=true -XX:ArchiveClassesAtExit=./application.jsa \
-Dspring.context.exit=onRefresh -jar /app/app.jar
```

---
layout: image
image: /charts/cds-startup.svg
---



---
layout: image
image: /charts/cds-mem-usage.svg
---


---
layout: image
image: /charts/cds-image-size.svg
---


---
layout: image
image: /Bg-8.png
---

> # We need to prepare ourselves and gather knowledge!

<br/>

The Sage

<style>
h1 {
    font-size: 40px;
    color: #FFFFFF;
}

div {
    font-size: 24px;
    text-align: right;
}
</style>

---
layout: image
image: /Bg-12.png
---

# Problem

- AppCDS may not be enough for our purposes
- We want faster startup and warmup
- Not all can migrate to Native Image

<br/>

<v-click>Shift some heavy-lifting tasks<br/>
to another point in time?</v-click>

---
layout: image
image: /Bg-12.png
---

# Solution: Project Leyden


- Beyond AppCDS: AOT Cache
- Shift some computations from production <br/> run to an earlier stage
- Condensers: shifting, constraining, and<br/> optimizing transformations
- Flexibly choose which condensers to apply

---
layout: image
image: /Bg-16.png
---

# Project Leyden: Any Considerations?


- Still in the makings
- JEP 483: Ahead-of-Time Class Loading & Linking<br/> (JDK 24)
- The more constraints applied,<br/> the better startup/warmup

<br/>
<v-click>Meanwhile, we can experiment!</v-click>

---


```xml {all|7,8,9}
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>process-aot</id>
            <goals>
                <goal>process-aot</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

---


```docker {1,9|11,16,17|20,25-28|31,32|24}{maxHeight:'300px'}
FROM bellsoft/liberica-runtime-container:jdk-24-stream-musl as builder

ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-24-cds-musl as optimizer
ARG project
ENV project=${project}

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --destination extracted


FROM bellsoft/liberica-runtime-container:jre-24-cds-musl

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-Dspring.aot.enabled=true", "-XX:AOTCache=app.aot", "-jar", "/app/app.jar"]
COPY --from=optimizer /app/extracted/dependencies/ ./
COPY --from=optimizer /app/extracted/spring-boot-loader/ ./
COPY --from=optimizer /app/extracted/snapshot-dependencies/ ./
COPY --from=optimizer /app/extracted/application/ ./


RUN java -Dspring.aot.enabled=true -XX:AOTMode=record -XX:AOTConfiguration=app.aotconf -Dspring.context.exit=onRefresh -jar /app/app.jar
RUN java -Dspring.aot.enabled=true -XX:AOTMode=create -XX:AOTConfiguration=app.aotconf -XX:AOTCache=app.aot -jar /app/app.jar
```


---


```xml {7,8,9}
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>process-aot</id>
            <goals>
                <goal>process-aot</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

---


```docker {none|1,4,5,6|8,12,15,18|20,24,25,28|30,35-39|41,42|44}{maxHeight:'300px'}
FROM bellsoft/alpaquita-linux-base:glibc AS downloader

RUN apk add curl tar
RUN curl https:/download.java.net/java/early_access/leyden/2/openjdk-24-leyden+2-8_linux-x64_bin.tar.gz -o /java.tar.gz && \
    cd / && tar -zxvf java.tar.gz && mv /jdk-24 /java && \
    rm -f /java.tar.gz

FROM bellsoft/alpaquita-linux-base:glibc AS builder
ARG project

WORKDIR /app
COPY --from=downloader /java /java
ADD ../pom.xml ./
ADD ${project} /app/${project}
ENV JAVA_HOME=/java \
    project=${project}

RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/alpaquita-linux-base:glibc AS optimizer
ARG project

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
COPY --from=downloader /java /java
ENV project=${project}

RUN /java/bin/java -Djarmode=tools -jar app.jar extract --layers --destination extracted

FROM bellsoft/alpaquita-linux-base:glibc AS runner

RUN apk add curl
WORKDIR /app

COPY --from=downloader /java /java
COPY --from=optimizer /app/extracted/dependencies/ ./
COPY --from=optimizer /app/extracted/spring-boot-loader/ ./
COPY --from=optimizer /app/extracted/snapshot-dependencies/ ./
COPY --from=optimizer /app/extracted/application/ ./

RUN /java/bin/java -XX:CacheDataStore=./application.cds \
    -Dspring.context.exit=onRefresh -jar /app/app.jar

ENTRYPOINT ["/java/bin/java", "-XX:CacheDataStore=./application.cds", "-jar", "/app/app.jar"]
```

---
layout: image
image: /charts/leyden-startup.svg
---



---
layout: image
image: /charts/leyden-mem-usage.svg
---



---
layout: image
image: /charts/leyden-image-size.svg
---



---
layout: image
image: /Bg-8.png
---

> # I found something exciting... Can't wait to explore!

<br/>

The Explorer

<style>
h1 {
font-size: 40px;
color: #FFFFFF;
}
div {
    font-size: 24px;
    text-align: right;
}
</style>

---
layout: image
image: /Bg-12.png
---

# Problem


- Need to start up in ~1 second at peak performance
- Compile and optimize code<br/> at runtime -> N minutes
- More memory for profile data<br/> and bytecode cache

<br/>

<v-click>Compile and optimize<br/>
at build time?</v-click>



---
layout: image
image: /Bg-12.png
---

# Solution: GraalVM Native Image


- Ahead-of-time compilation
- Closed world assumption
- Standalone executable (.exe)
- No OpenJDK required to run
- The app starts at peak performance



---
layout: image
image: /aot.png
---

---
layout: image
image: /Bg-12.png
---

# Native Image: Any Considerations?


- Resource-demanding build process
- Several minutes to build the image
- Special treatment of Java's dynamism

<br/>
<v-click>Let the journey begin!</v-click>


---


# First, let's try it locally

Build a fat JAR:

```xml {all}{maxHeight:'200px'}
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal>
            </goals>
            <configuration>
                <classifier>spring-boot</classifier>
                <mainClass>
                    com.github.asm0dey.chatapi.ChatApiApplication
                </mainClass>
            </configuration>
        </execution>
    </executions>
</plugin>
```

<br/>

```bash
mvn clean package
```


---


Collect the metadata with the Tracing Agent:

```bash
$JAVA_HOME/bin/java -agentlib:native-image-agent=config-output-dir=./bot-assistant/agent-data \
-jar bot-assistant/target/bot-assistant-1.0-SNAPSHOT-spring-boot.jar
```

Build the image:

```bash
$JAVA_HOME/bin/native-image -H:ConfigurationFileDirectories=./bot-assistant/agent-data \
-jar bot-assistant/target/bot-assistant-1.0-SNAPSHOT-spring-boot.jar
```

The image was built, let's run it!


---
layout: image
image: /Bg-8.png
---

# Oops!
<br/>

```bash
Exception in thread "main" java.lang.IllegalStateException: java.util.zip.ZipException: zip END header not found
        at org.springframework.boot.loader.ExecutableArchiveLauncher.<init>(ExecutableArchiveLauncher.java:57)
        at org.springframework.boot.loader.JarLauncher.<init>(JarLauncher.java:42)
        at org.springframework.boot.loader.JarLauncher.main(JarLauncher.java:65)
Caused by: java.util.zip.ZipException: zip END header not found
```

<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: /Bg-8.png
---


Apparently, [a known issue](https:/github.com/oracle/graal/issues/7034)<br/>

<img src="/zip-ex.png"/>

➡️ Use ```mvn -Pnative native:compile``` to build the image.<br/>

<v-click><b>🤷‍♀️ Alright, let's try with a plugin.</b></v-click>

---


Add ```native``` profile:
```xml {all|6,7,8|21|22-24|27-29}{maxHeight:'200px'}
<profiles>
    <profile>
        <id>native</id>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.graalvm.buildtools</groupId>
                    <artifactId>native-maven-plugin</artifactId>
                    <executions>
                        <execution>
                            <id>build-native</id>
                            <goals>
                                <goal>build</goal>
                            </goals>
                            <phase>package</phase>
                        </execution>
                    </executions>
                    <configuration>
                        <imageName>bot-assistant</imageName>
                        <outputDirectory>target/native</outputDirectory>
                        <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                        <buildArgs>
                            <buildArg>-H:ConfigurationFileDirectories=./agent-data</buildArg>
                        </buildArgs>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <classifier>exec</classifier>
                            <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

Run

```bash
mvn -Pnative native:compile
```

---
layout: image
image: /Bg-8.png
---

# Oops!
<br/>

```bash{all|1,6}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.CommonConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.CommonConfig
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time. To see why com.ctc.wstx.stax.WstxInputFactory got initialized use --trace-class-initialization=com.ctc.wstx.stax.WstxInputFactory
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.ReaderConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.ReaderConfig
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time. To see why com.ctc.wstx.util.DefaultXmlSymbolTable got initialized use --trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable
To see how the classes got initialized, use --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
```



<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---


Add the `--trace-class-initialization` argument:
```xml {none|23}{maxHeight:'200px'}
<profile>
    <id>native</id>
    <build>
        <plugins>
            <plugin>
                <groupId>org.graalvm.buildtools</groupId>
                <artifactId>native-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>build-native</id>
                        <goals>
                            <goal>build</goal>
                        </goals>
                        <phase>package</phase>
                    </execution>
                </executions>
                <configuration>
                    <imageName>bot-assistant</imageName>
                    <outputDirectory>target/native</outputDirectory>
                    <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                    <buildArgs>
                        <buildArg>-H:ConfigurationFileDirectories=./agent-data</buildArg>
                        <buildArgs>--trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable,com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig</buildArgs>
                    </buildArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</profile>
```

Run

```bash
mvn -Pnative native:compile
```


---


More detailed output:
```bash{all|2}{maxHeight:'200px'}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. org.springframework.http.codec.xml.XmlEventDecoder caused initialization of this class with the following trace:
        at com.ctc.wstx.api.CommonConfig.<clinit>(CommonConfig.java:59)
        at com.ctc.wstx.stax.WstxInputFactory.<init>(WstxInputFactory.java:149)
        at java.lang.invoke.DirectMethodHandle$Holder.newInvokeSpecial(DirectMethodHandle$Holder)
        at java.lang.invoke.Invokers$Holder.invokeExact_MT(Invokers$Holder)
        at jdk.internal.reflect.DirectConstructorHandleAccessor.invokeImpl(DirectConstructorHandleAccessor.java:86)
        at jdk.internal.reflect.DirectConstructorHandleAccessor.newInstance(DirectConstructorHandleAccessor.java:62)
        at java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:486)
        at java.util.ServiceLoader$ProviderImpl.newInstance(ServiceLoader.java:789)
        at java.util.ServiceLoader$ProviderImpl.get(ServiceLoader.java:729)
        at java.util.ServiceLoader$3.next(ServiceLoader.java:1403)
        at javax.xml.stream.FactoryFinder$1.run(FactoryFinder.java:305)
        at java.security.AccessController.executePrivileged(AccessController.java:778)
        at java.security.AccessController.doPrivileged(AccessController.java:319)
        at javax.xml.stream.FactoryFinder.findServiceProvider(FactoryFinder.java:293)
        at javax.xml.stream.FactoryFinder.find(FactoryFinder.java:264)
        at javax.xml.stream.FactoryFinder.find(FactoryFinder.java:210)
        at javax.xml.stream.XMLInputFactory.newInstance(XMLInputFactory.java:166)
        at org.springframework.util.xml.StaxUtils$$Lambda/0x00000080027d8000.get(Unknown Source)
        at org.springframework.util.xml.StaxUtils.createDefensiveInputFactory(StaxUtils.java:77)
        at org.springframework.util.xml.StaxUtils.createDefensiveInputFactory(StaxUtils.java:67)
        at org.springframework.http.codec.xml.XmlEventDecoder.<clinit>(XmlEventDecoder.java:87)

```

<br/>

<v-click>We found the culprit!</v-click>


---


Alright, let's add the option to initialize ```org.springframework.http.codec.xml.XmlEventDecoder``` at runtime:
```xml {none|23}{maxHeight:'200px'}
<profile>
    <id>native</id>
    <build>
        <plugins>
            <plugin>
                <groupId>org.graalvm.buildtools</groupId>
                <artifactId>native-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>build-native</id>
                        <goals>
                            <goal>build</goal>
                        </goals>
                        <phase>package</phase>
                    </execution>
                </executions>
                <configuration>
                    <imageName>bot-assistant</imageName>
                    <outputDirectory>target/native</outputDirectory>
                    <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                    <buildArgs>
                        <buildArg>-H:ConfigurationFileDirectories=./agent-data</buildArg>
                        <buildArgs>--initialize-at-run-time=org.springframework.http.codec.xml.XmlEventDecoder</buildArgs>
                    </buildArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</profile>
```

Run

```bash
mvn -Pnative native:compile
```

---
layout: image
image: /Bg-8.png
---

# Oops!
<br/>

```bash{all}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.CommonConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.CommonConfig
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time. To see why com.ctc.wstx.stax.WstxInputFactory got initialized use --trace-class-initialization=com.ctc.wstx.stax.WstxInputFactory
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.ReaderConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.ReaderConfig
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time. To see why com.ctc.wstx.util.DefaultXmlSymbolTable got initialized use --trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable
To see how the classes got initialized, use --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
```
<br/>

The same error!


<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
div {
  font-size: 34px;
}
</style>

---
layout: image
image: /Bg-8.png
---

Apparently, [another known issue](https:/github.com/spring-projects/spring-framework/issues/31806#issuecomment-1862502951)<br>

<img src="/xml-ex.png"/>

➡️ Need to add the ```--strict-image-heap``` option.<br/>

<v-click><b>🫡 Will do!</b></v-click>


---


Let's add the ```--strict-image-heap``` option:
```xml {none|23}{maxHeight:'200px'}
<profile>
    <id>native</id>
    <build>
        <plugins>
            <plugin>
                <groupId>org.graalvm.buildtools</groupId>
                <artifactId>native-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>build-native</id>
                        <goals>
                            <goal>build</goal>
                        </goals>
                        <phase>package</phase>
                    </execution>
                </executions>
                <configuration>
                    <imageName>bot-assistant</imageName>
                    <outputDirectory>target/native</outputDirectory>
                    <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                    <buildArgs>
                        <buildArg>-H:ConfigurationFileDirectories=./agent-data</buildArg>
                        <buildArgs>--strict-image-heap</buildArgs>
                    </buildArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</profile>
```

Run

```bash
mvn -Pnative native:compile
```



---
class: text-center
layout: cover
background: /Bg-1.png
---

# Success!

## <v-click>...Or is it?</v-click>


---
layout: image
image: /Bg-8.png
---

# Oops!

```bash {all|3} {maxHeight:'200px'}
2025-04-11T12:25:33.511+03:00 ERROR 64619 --- [bot-assistant] [nio-8081-exec-2] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Handler dispatch failed: java.lang.UnsatisfiedLinkError: jdk.jfr.internal.JVM.isExcluded(Ljava/lang/Class;)Z [symbol: Java_jdk_jfr_internal_JVM_isExcluded or Java_jdk_jfr_internal_JVM_isExcluded__Ljava_lang_Class_2]] with root cause

java.lang.UnsatisfiedLinkError: jdk.jfr.internal.JVM.isExcluded(Ljava/lang/Class;)Z [symbol: Java_jdk_jfr_internal_JVM_isExcluded or Java_jdk_jfr_internal_JVM_isExcluded__Ljava_lang_Class_2]
        at org.graalvm.nativeimage.builder/com.oracle.svm.core.jni.access.JNINativeLinkage.getOrFindEntryPoint(JNINativeLinkage.java:152) ~[na:na]
        at org.graalvm.nativeimage.builder/com.oracle.svm.core.jni.JNIGeneratedMethodSupport.nativeCallAddress(JNIGeneratedMethodSupport.java:54) ~[na:na]
        at jdk.jfr@21.0.6/jdk.jfr.internal.JVM.isExcluded(Native Method) ~[na:na]
                at jdk.jfr@21.0.6/jdk.jfr.internal.MetadataRepository.register(MetadataRepository.java:137) ~[na:na]
        at jdk.jfr@21.0.6/jdk.jfr.internal.MetadataRepository.register(MetadataRepository.java:132) ~[na:na]
        at jdk.jfr@21.0.6/jdk.jfr.FlightRecorder.register(FlightRecorder.java:128) ~[na:na]
        at io.lettuce.core.event.connection.JfrConnectionCreatedEvent.<clinit>(JfrConnectionCreatedEvent.java) ~[bot:6.4.2.RELEASE/f4dfb40]
        at java.base@21.0.6/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502) ~[bot:na]
        at java.base@21.0.6/java.lang.reflect.Constructor.newInstance(Constructor.java:486) ~[bot:na]
        at io.lettuce.core.event.jfr.JfrEventRecorder.createEvent(JfrEventRecorder.java:93) ~[na:na]
        at io.lettuce.core.event.jfr.JfrEventRecorder.record(JfrEventRecorder.java:33) ~[na:na]
```

## <v-click>Apparently, a JFR event is created when the user sends<br/> a request to the AI Bot</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    font-size: 24px;
}
</style>


---


Let's enable JFR:
```xml {none|24}{maxHeight:'200px'}
<profile>
    <id>native</id>
    <build>
        <plugins>
            <plugin>
                <groupId>org.graalvm.buildtools</groupId>
                <artifactId>native-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>build-native</id>
                        <goals>
                            <goal>build</goal>
                        </goals>
                        <phase>package</phase>
                    </execution>
                </executions>
                <configuration>
                    <imageName>bot-assistant</imageName>
                    <outputDirectory>target/native</outputDirectory>
                    <mainClass>com.github.asm0dey.botassistant.BotAssistantApplication</mainClass>
                    <buildArgs>
                        <buildArg>-H:ConfigurationFileDirectories=./agent-data</buildArg>
                        <buildArgs>--strict-image-heap</buildArgs>
                        <buildArgs>--enable-monitoring=jfr</buildArgs>
                    </buildArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</profile>
```

Run

```bash
mvn -Pnative native:compile
```

---
class: text-center
layout: cover
background: /Bg-1.png
---

# Finally, everything works!

## <v-click>Yeah, locally</v-click>


---
class: text-center
layout: cover
background: /Bg-1.png
---

## Let's move on to building a native image in a Docker container.

### <v-click>Piece of cake, right? We solved all issues..</v-click>

<v-click>
<img src="/star-wars.png" width="300px" class="center"/>
</v-click>


<style>
h2 {
    font-size: 34px;
    position: relative;
    bottom: 20px;
}
.center {
  margin-left: auto;
  margin-right: auto;
}
</style>

---


```docker {all|1,8|10,14,17|16} {maxHeight:'200px'}
FROM bellsoft/liberica-native-image-kit-container:jdk-21-nik-23.1.6-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true -Pnative native:compile

FROM bellsoft/alpaquita-linux-base:stream-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["/app/app"]
COPY --from=builder /app/${project}/target/native/${project} /app/app
```


---
layout: image
image: /Bg-8.png
---

# Oops!
<br/>

```bash{all}
2189.3 [6/8] Compiling methods...    [*************]                                                          (187.0s @ 5.54GB)
2189.3
2189.3 Fatal error: org.graalvm.compiler.debug.GraalError: org.graalvm.compiler.core.common.PermanentBailoutException: Compilation exceeded 300.000000 seconds during CFG traversal
2189.3  at method: Future io.netty.resolver.AbstractAddressResolver.resolve(SocketAddress)  [Virtual call from Object AddressResolverGroupMetrics$DelegatingAddressResolver$$Lambda/0x60e14e76cf1bdb337c1b0dbb92d2d481fd9de99e0.get(), callTarget Future AddressResolver.resolve(SocketAddress)]
========================================================================================================================
2189.3 Finished generating 'bot' in 10m 17s.
2190.5 [INFO] ------------------------------------------------------------------------
2190.5 [INFO] BUILD FAILURE
2190.5 [INFO] ------------------------------------------------------------------------
2190.5 [INFO] Total time:  36:26 min
2190.5 [INFO] Finished at: 2025-04-15T11:28:49Z
2190.5 [INFO] ------------------------------------------------------------------------
```



<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: /Bg-8.png
---

[A bug?](https:/github.com/abertschi/graalphp/pull/39)<br/>

<img src="/bailout-ex.png"/>

<br/>

<v-click><b>🤔 Doesn't look like MY problem</b></v-click>

---
layout: image
image: /Bg-8.png
---
The process is so slow it fails.<br/>
[Wrong Colima settings](https:/github.com/abiosoft/colima/issues/204)<br/>

<img src="/colima-ex.png"/>

➡️ Switching from ```colima start --vm-type=vz``` to ```colima start --vm-type=qemu``` should do the trick.

<br/>

<v-click><b>Fingers crossed or what?</b></v-click>


---
layout: image
image: /Bg-8.png
---

# Oops!
<br/>

```bash{all|10,12,21}{maxHeight:'200px'}
520.7 [8/8] Creating image...       [*****]                                                                    (0.0s @ 3.06GB)
520.7 ------------------------------------------------------------------------------------------------------------------------
520.7                       53.2s (13.5% of total time) in 2385 GCs | Peak RSS: 6.77GB | CPU load: 3.36
520.7 ------------------------------------------------------------------------------------------------------------------------
520.7 Produced artifacts:
520.7  /app/bot-assistant/target/native/svm_err_b_20250415T153317.420_pid287.md (build_info)
520.7 ========================================================================================================================
520.7 Failed generating 'bot' after 6m 32s.
520.7
520.7 The build process encountered an unexpected error:
520.7
520.7 > java.lang.RuntimeException: There was an error linking the native image: Linker command exited with 1
520.7
520.7 Linker command executed:
520.7 /usr/bin/gcc -z noexecstack -Wl,--gc-sections -Wl,--version-script,/tmp/SVM-10923554795000531792/exported_symbols.list -no-pie -Wl,-x -o /app/bot-assistant/target/native/bot bot.o /usr/lib/jvm/liberica-nik-23-21/lib/svm/clibraries/linux-aarch64/liblibchelper.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libnet.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libextnet.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libnio.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libmanagement_ext.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libjava.a /usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl/libzip.a /usr/lib/jvm/liberica-nik-23-21/lib/svm/clibraries/linux-aarch64/libjvm.a -Wl,--export-dynamic -v -L/tmp/SVM-10923554795000531792 -L/usr/lib/jvm/liberica-nik-23-21/lib/static/linux-aarch64/musl -L/usr/lib/jvm/liberica-nik-23-21/lib/svm/clibraries/linux-aarch64 -lz -ldl -lpthread -lrt -Wl,-u,JNU_CallMethodByName -Wl,-u,JNU_CallStaticMethodByName -Wl,-u,JNU_GetEnv -Wl,-u,JNU_GetStaticFieldByName -Wl,-u,JNU_GetStringPlatformChars -Wl,-u,JNU_IsInstanceOfByName -Wl,-u,JNU_NewObjectByName -Wl,-u,JNU_NewStringPlatform -Wl,-u,JNU_ReleaseStringPlatformChars -Wl,-u,JNU_SetFieldByName -Wl,-u,JNU_ThrowArrayIndexOutOfBoundsException -Wl,-u,JNU_ThrowByName -Wl,-u,JNU_ThrowIllegalArgumentException -Wl,-u,JNU_ThrowInternalError -Wl,-u,JNU_ThrowNullPointerException -Wl,-u,JNU_ThrowOutOfMemoryError -Wl,-u,JNI_CreateJavaVM -Wl,-u,JNI_GetCreatedJavaVMs -Wl,-u,JNI_GetDefaultJavaVMInitArgs -Wl,-u,jio_fprintf -Wl,-u,jio_snprintf
520.7
520.7 Linker command output:
520.7 Using built-in specs.
520.7 COLLECT_GCC=/usr/bin/gcc
520.7 COLLECT_LTO_WRAPPER=/usr/libexec/gcc/aarch64-alpaquita-linux-musl/14.2.0/lto-wrapper
520.7 Target: aarch64-alpaquita-linux-musl
520.7 Configured with: /ws/workspace/aq-build-pkg/aports/core/gcc/src/gcc-14.2.0/configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --build=aarch64-alpaquita-linux-musl --host=aarch64-alpaquita-linux-musl --target=aarch64-alpaquita-linux-musl --enable-checking=release --disable-fixed-point --disable-libstdcxx-pch --disable-multilib --disable-nls --disable-werror --disable-symvers --enable-__cxa_atexit --enable-default-pie --enable-languages=c,c++,d,objc,go,fortran,ada --enable-link-serialization=2 --enable-linker-build-id --with-arch=armv8-a --with-abi=lp64 --disable-libquadmath --disable-libssp --disable-libsanitizer --disable-cet --enable-gnu-indirect-function=yes --enable-shared --enable-threads --enable-tls --with-bugurl='https:/bell-sw.com/support/' --with-system-zlib --with-linker-hash-style=gnu --with-pkgversion='Alpaquita 14.2.0'
```



<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: /Bg-8.png
---

## Linking issue: musl libc lacks required tools

<br/>

- Option 1: add required packages (libstc++, etc.) with ```apk add```
- <span v-mark.highlight.red="1"> Option 2: switch from musl-based Alpaquita to glibc-based </span>

<br/>

```docker {all|1,10}{maxHeight:'200px'}
FROM bellsoft/liberica-native-image-kit-container:jdk-21-nik-23.1.6-stream-glibc as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true -Pnative native:compile

FROM bellsoft/alpaquita-linux-base:stream-glibc
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["/app/app"]
COPY --from=builder /app/${project}/target/native/${project} /app/app
```


<v-click>I feel we are getting close...</v-click>

---
class: text-center
layout: cover
background: /Bg-1.png
---

# We did it!

## <v-click>Both services compile and run successfully</v-click>

---
layout: image
image: /charts/native-image-startup.svg
---



---
layout: image
image: /charts/native-image-mem-usage.svg
---




---
layout: image
image: /charts/native-image-size.svg
---




---


<img src="/frodo.png" class="center"/>


<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>


---
layout: image
image: /Bg-8.png
---

> # We need drastic changes, I won't settle for half-measures!

<br/>

The Rebel

<style>
h1 {
font-size: 40px;
color: #FFFFFF;
}
div {
    font-size: 24px;
    text-align: right;
}
</style>

---
layout: image
image: /Bg-12.png
---

# Problem

- Unfamiliar workflow with C++ code<br/> of Native Image
- Push the limits: reduce<br/> startup/warmup to milliseconds
- Want to preserve JIT-compilation



---
layout: image
image: /Bg-12.png
---

# Solution: Coordinated Restore at Checkpoint (CRaC)


- Pause and restart a Java application
- A snapshot of the current JVM state
- JVM is still there:<br/> dynamic performance optimization<br/> is possible after restore



---
layout: image
image: /Bg-12.png
---

# Project CRaC: Any Considerations?

- A snapshot may contain sensitive data
- May need to augment the code<br/> for reliable checkpoint and restore


