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
colorSchema: "dark"
---

## Four Approaches to Reducing Java Startup Time

<img src="/bellsoft.png" width="200px" class="absolute right-10px bottom-5px"/>

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>

---
layout: image-right
image: "/cat.jpg"
---

# Catherine Edelveis

<img/>

🥑 Developer Advocate at BellSoft

😍Love Java, Spring, JavaFX

👩‍💻Tech writer

<span><line-md-twitter-x /> cat_edelveis</span>

<span><logos-bluesky /> cat-edelveis.bsky.social</span>


---
layout: image-left
image: "/pasha.jpeg"
---

# Pasha Finkelshteyn

<img/>

🥑 Developer Advocate at BellSoft

≈15 years in JVM, mostly <logos-java /><logos-kotlin-icon/><logos-spring-icon/>

<span><logos-bluesky /> @asm0dey.site</span>

<span><line-md-twitter-x /> asm0di0</span>


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
image: "/members.png"
---

# About BellSoft

Founded in 2017 by Java and Linux experts with 15+ years of experience working at Sun/Oracle.

- Member of:
  - JCP Executive Committee
  - OpenJDK Vulnerability Group
  - GraalVM Advisory Board
  - Linux Foundation
  - Cloud Native Computing Foundation


---
layout: image-right
image: "/qr.png"
---

# About BellSoft


- Products:
  - Liberica JDK
  - Liberica Native Image Kit
  - Alpaquita Linux

<br/>
Liberica is the JDK officially recommended by <logos-spring-icon />


---
class: text-center
layout: cover
background: /Bg-1.png
---

# Enough about us :)

## We are here today to tell you a story about...

<v-click>YOU!</v-click>

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
class: statement
layout: cover
background: /Bg-1.png
---

# But first, four quick questions ;)

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows

# one way

## to reduce the startup of their Java application?

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows

# two ways

## to reduce the startup of their Java application?

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows

# three ways

## to reduce the startup of their Java application?

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Who knows

# four ways

## to reduce the startup of their Java application?


---
class: statement
layout: cover
background: /Bg-1.png
---

# Once upon a time, all was well in one startup...

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


- Java 21 LTS
- Spring Boot 3.4
- Spring AI
- MongoDB, Redis
- JTE
- Deploying to the cloud

```java
@GetMapping(value = "/assistant", produces = TEXT_HTML_VALUE)
public JteModel assistant() {
  return templates.assistantView("Bot Assistant - Chat");
}
```

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

```docker {none|1,8|10,16|17}{maxHeight:'300px'}
FROM bellsoft/liberica-runtime-container:jdk-21-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw package

FROM bellsoft/liberica-runtime-container:jre-21-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar /app/app.jar
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

---
layout: image
image: /Bg-8.png
---

# Starting point: Startup

<br/>

```bash
docker compose logs chat-api bot-assistant | grep Started
Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```

Total startup time of both services: 6.5 s

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
<div v-click class="text-xl"> Why the rollout is so slow? </div>

---
layout: image
image: /Bg-8.png
---

# We are an agile team, we need a fast feedback loop!

<br/>
<v-clicks>

Need faster rollout for faster feedback  

The longer the start, the slower the rollout

<b><br/> Maybe Java is not cut out for the cloud? </b>

<b> Maybe it's not too late to migrate to Go? </b>
</v-clicks>


---
class: text-center
layout: cover
background: /Bg-1.png
---

Luckily, four brave developers work at this startup:

The Defender

The Sage

The Explorer

The Rebel

---
class: text-center
layout: cover
background: /Bg-1.png
---

They are very different, but have something in common:

They care about their software product and can't sit idle in the troubled time.

---
class: text-center
layout: cover
background: /Bg-1.png
---

# And that's where

# YOU

# take the stage!

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
layout: two-cols
---

# AppCDS

- Save class metadata to the disc and read it from the archive
- Even better with Spring AOT <br/> (load even more classes!)

Considerations:

- Use the same JVM
- Use the same classpath
- Bigger Docker image due to the archive

<v-click>Let's take it for a spin!</v-click>

::right:: 
<Youtube id="_oXnnQcD_wc"/>




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

```docker {1,9|11,16,17|20,24-27|28,29,30|31,32,33}{maxHeight:'300px'}
FROM bellsoft/liberica-runtime-container:jdk-21.0.7_9-musl as builder

ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw package

FROM bellsoft/liberica-runtime-container:jre-21.0.7_9-cds-musl as optimizer
ARG project
ENV project=${project}

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --destination extracted


FROM bellsoft/liberica-runtime-container:jre-21.0.7_9-cds-musl

RUN apk add curl
WORKDIR /app
COPY --from=optimizer /app/extracted/dependencies/ ./
COPY --from=optimizer /app/extracted/spring-boot-loader/ ./
COPY --from=optimizer /app/extracted/snapshot-dependencies/ ./
COPY --from=optimizer /app/extracted/application/ ./
RUN java -Dspring.aot.enabled=true \
-XX:ArchiveClassesAtExit=./application.jsa \
-Dspring.context.exit=onRefresh -jar /app/app.jar
ENTRYPOINT ["java", "-Dspring.aot.enabled=true", \
"-XX:SharedArchiveFile=application.jsa", \
"-jar", "/app/app.jar"]
```

---
layout: image
image: /charts/cds-startup.svg
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

# Project Leyden

- Beyond AppCDS: AOT Cache with pre-compiled code
- Condensers -> optimizations

Considerations:
- Still in the makings
- The more condensers applied, the bigger the cache

<br/>

<v-click>Meanwhile, we can experiment!</v-click>


---

# Let's start with what is already implemented

<img/>

[JEP 483](https://openjdk.org/jeps/483): Ahead-of-Time Class Loading & Linking (JDK 24)

> Improve startup time by making the classes of an application instantly available, in a loaded and linked state, when the HotSpot Java Virtual Machine starts. Achieve this by monitoring the application during one run and storing the loaded and linked forms of all classes in a cache for use in subsequent runs. Lay a foundation for future improvements to both startup and warmup time.


---

```docker {1,9|11,16,17|19,23-26|28-31|33,34}{maxHeight:'350px'}
FROM bellsoft/liberica-runtime-container:jdk-24-musl AS builder

ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw package

FROM bellsoft/liberica-runtime-container:jre-24-cds-musl AS optimizer
ARG project
ENV project=${project}

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --destination extracted

FROM bellsoft/liberica-runtime-container:jre-24-cds-musl AS runner

RUN apk add curl
WORKDIR /app
COPY --from=optimizer /app/extracted/dependencies/ ./
COPY --from=optimizer /app/extracted/spring-boot-loader/ ./
COPY --from=optimizer /app/extracted/snapshot-dependencies/ ./
COPY --from=optimizer /app/extracted/application/ ./

RUN java -Dspring.aot.enabled=true -XX:AOTMode=record \
    -XX:AOTConfiguration=app.aotconf -Dspring.context.exit=onRefresh -jar /app/app.jar
RUN java -Dspring.aot.enabled=true -XX:AOTMode=create \
    -XX:AOTConfiguration=app.aotconf -XX:AOTCache=app.aot -jar /app/app.jar
  
ENTRYPOINT ["java", "-Dspring.aot.enabled=true", "-XX:AOTCache=app.aot",
 "-jar", "/app/app.jar"]
```

---
layout: image
image: /charts/aot-cache-startup.svg
---

---

# Not enough?

<div></div>

Let's push it even further and use the builds of premain in Leyden!

<sub>We will be happy to get your feedback on what's broken!</sub>


---

```docker {1,2,4|6,10-13,16|18,22-23,26|28,33-37|39,40|42}{maxHeight:'350px'}
FROM bellsoft/alpaquita-linux-base:glibc AS downloader
ADD https://is.gd/tZhyPF /java.tar.gz # just a placeholder
RUN apk add tar
RUN tar -zxvf java.tar.gz && mv /jdk-24 /java

FROM bellsoft/alpaquita-linux-base:glibc AS builder
ARG project

WORKDIR /app
COPY --from=downloader /java /java
ADD ../pom.xml ./
ADD ${project} /app/${project}
ENV JAVA_HOME=/java \
    project=${project}

RUN cd ${project} && ./mvnw package

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
image: /charts/leyden-aot-cache-startup.svg
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

# GraalVM Native Image

- Ahead-of-time compilation
- Standalone executable (.exe)
- No OpenJDK required to run

---
layout: image
image: /Bg-12.png
---

# Native Image: Any Considerations?

- Resource-demanding build process
- Several minutes to build the image
- Static compilation instead of dynamic one

<br/>
<v-click>Let the journey begin!</v-click>

---

# First, let's try it locally

<div></div>
Build a fat JAR


Build the image:

```bash
$JAVA_HOME/bin/native-image \
-jar bot-assistant/target/bot-assistant-1.0-SNAPSHOT-spring-boot.jar
```

The image was built, let's run it!

---
layout: image
image: /uni.jpg
---

# I never knew the journey can be so pleasant!

## <v-click>Uh-oh...</v-click>

<style>
h1 {
    color: #000000;
}
h2 {
    color: #000000;
    text-align: center;
}
</style>

---
layout: image
image: /dragon.jpg
---


---
layout: image
image: /Bg-8.png
---

# A dragon!

<div></div>

```bash {all|2}
Exception in thread "main" java.lang.IllegalStateException:
java.util.zip.ZipException: zip END header not found
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

➡️ Use `mvn -Pnative native:compile` to build the image.<br/>

Maven plugin calls `native-image java -jar` under the hood but uses lots of flags 
that solve problems like the one above.

<v-click><b>Alright, let's try with a plugin! </b></v-click>

---

If you use Spring Boot Parent POM, the Native Image profile is already configured!

➡️ Enable it by simply running

```bash
mvn -Pnative native:compile
```

---
layout: image
image: /Bg-8.png
---

# Another dragon!

<br/>

```bash{all|1,7,8}
Error: Classes that should be initialized at run time got initialized during image building:
com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time.
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time.
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time.
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time.

To see how the classes got initialized, use
 --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
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

Add `native` profile:

```xml {all|6,7,8|21}{maxHeight:'200px'}
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
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

---

Add the `--trace-class-initialization` argument:

```xml {none|22}{maxHeight:'200px'}
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

```bash{all|4}{maxHeight:'200px'}
Error: Classes that should be initialized at run time got initialized during image building:

 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time.
 org.springframework.http.codec.xml.XmlEventDecoder caused initialization of this class
 with the following trace:
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

```

<br/>

<v-click at="1"">We found the culprit!</v-click>

---

Alright, let's add the option to initialize `org.springframework.http.codec.xml.XmlEventDecoder` at runtime:

```xml {none|22}{maxHeight:'200px'}
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

# The same dragon!

<div></div>

```bash{all}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.CommonConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.CommonConfig
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time. To see why com.ctc.wstx.stax.WstxInputFactory got initialized use --trace-class-initialization=com.ctc.wstx.stax.WstxInputFactory
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.ReaderConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.ReaderConfig
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time. To see why com.ctc.wstx.util.DefaultXmlSymbolTable got initialized use --trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable
To see how the classes got initialized, use --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
```

<br/>

This is one evasive dragon...

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

Apparently, [another known issue](https:/github.com/spring-projects/spring-framework/issues/31806#issuecomment-1862502951)<br/>

<img src="/xml-ex.png"/>

➡️ Need to add the `--strict-image-heap` option.<br/>

> This mode requires only the classes that are stored in the image heap to be marked with
> --initialize-at-build-time. This effectively reduces the number of configuration entries
> necessary to achieve build-time initialization.
>
> Note that --strict-image-heap is enabled by default in Native Image starting from GraalVM for JDK 22.

---

Let's add the `--strict-image-heap` option:

```xml {22}{maxHeight:'200px'}
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

<v-click>

## ...Or is it?

</v-click>


<v-click at="2">

The image was build, let's run it.

</v-click>

---
layout: image
image: /Bg-8.png
---

# Another dragon!

```bash {all|3-6} {maxHeight:'200px'}
2025-04-11T12:25:33.511+03:00 ERROR 64619 --- [bot-assistant] [nio-8081-exec-2] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Handler dispatch failed: java.lang.UnsatisfiedLinkError: jdk.jfr.internal.JVM.isExcluded(Ljava/lang/Class;)Z [symbol: Java_jdk_jfr_internal_JVM_isExcluded or Java_jdk_jfr_internal_JVM_isExcluded__Ljava_lang_Class_2]] with root cause

java.lang.UnsatisfiedLinkError:
jdk.jfr.internal.JVM.isExcluded(Ljava/lang/Class;)Z
[symbol: Java_jdk_jfr_internal_JVM_isExcluded
or Java_jdk_jfr_internal_JVM_isExcluded__Ljava_lang_Class_2]

        at org.graalvm.nativeimage.builder/com.oracle.svm.core.jni.access.JNINativeLinkage.getOrFindEntryPoint(JNINativeLinkage.java:152) ~[na:na]
        at org.graalvm.nativeimage.builder/com.oracle.svm.core.jni.JNIGeneratedMethodSupport.nativeCallAddress(JNIGeneratedMethodSupport.java:54) ~[na:na]
        at jdk.jfr@21.0.6/jdk.jfr.internal.JVM.isExcluded(Native Method) ~[na:na]
                at jdk.jfr@21.0.6/jdk.jfr.internal.MetadataRepository.register(MetadataRepository.java:137) ~[na:na]
```

## <v-click at="1">Apparently, when we create a connection to Redis, Spring creates a custom JFR event</v-click>

<style scoped>
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

Even the Tracing Agent doesn't detect that!

So, let's enable JFR explicitly:

```xml {23}{maxHeight:'200px'}
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

## <v-click>...on my machine</v-click>

---
class: text-center
layout: cover
background: /Bg-1.png
---

## Let's move on to building a native image in a Docker container.


---

```docker {none|1,8|10,16,17} {maxHeight:'200px'}
FROM bellsoft/liberica-native-image-kit-container:jdk-21-nik-23.1.6-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Pnative native:compile

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

# Another dragon!

<br/>

```bash{all|3-5}{maxHeight:'200px'}
2189.3 [6/8] Compiling methods...    [*************]                                                          (187.0s @ 5.54GB)
2189.3 
2189.3 Fatal error: org.graalvm.compiler.debug.GraalError:
org.graalvm.compiler.core.common.PermanentBailoutException:
Compilation exceeded 300.000000 seconds during CFG traversal
2189.3  at method: Future 
```

---
layout: image
image: /Bg-8.png
---

## The build was taking too long and never finished

<br/>

Solution:
- Increase timeout time
- Build the image on a powerful PC or in the CI
- <span v-mark.box.green="1"> Plug a laptop into a power socket</span>

<br/>

<v-click at="2">Let's give it another try!</v-click>

---
layout: image
image: /Bg-8.png
---

# Another dragon!

<br/>

```bash{all|10,12,13}{maxHeight:'200px'}
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
520.7 > java.lang.RuntimeException: There was an error linking the native image:
Linker command exited with 1
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

- Option 1: add required packages (libstc++, etc.) with `apk add`
- <span v-mark.box.green="1"> Option 2: switch from musl-based Alpaquita to glibc-based </span>

<br/>

````md magic-move
```docker
FROM bellsoft/liberica-native-image-kit-container:jdk-21-nik-23.1.6-stream-musl as builder

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Pnative native:compile

FROM bellsoft/alpaquita-linux-base:stream-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["/app/app"]
COPY --from=builder /app/${project}/target/native/${project} /app/app
```

```docker
FROM bellsoft/liberica-native-image-kit-container:jdk-21-nik-23.1.6-stream-glibc as builder

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Pnative native:compile

FROM bellsoft/alpaquita-linux-base:stream-glibc
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["/app/app"]
COPY --from=builder /app/${project}/target/native/${project} /app/app
```
````

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

# Coordinated Restore at Checkpoint (CRaC)

> The CRaC (Coordinated Restore at Checkpoint) Project researches coordination of Java programs with mechanisms to checkpoint (make an image of, snapshot) a Java instance while it is executing. Restoring from the image could be a solution to some of the problems with the start-up and warm-up times. The primary aim of the Project is to develop a new standard mechanism-agnostic API to notify Java programs about the checkpoint and restore events. Other research activities will include, but will not be limited to, integration with existing checkpoint/restore mechanisms and development of new ones, changes to JVM and JDK to make images smaller and ensure they are correct.

https://openjdk.org/projects/crac/

---

# TL;DR

- Pause and restart a Java application
- A snapshot of the current JVM state
- JVM is still there: dynamic performance optimization is possible after restore

---

# CRaC is not a canonical part of JDK

<div></div>

Only some vendors provide it: BellSoft, Azul

Azul was the first to implement, BellSoft supports a fork

---

# Project CRaC: Any Considerations?

- A snapshot may contain sensitive data
- May need to augment the code for reliable checkpoint and restore

---

# Trivial example

```java {all|1|2|3|4|5-7|8}
public static void main(String args[]) throws InterruptedException {
  // This is a part of the saved state
  long startTime = System.currentTimeMillis();
  for(int counter: IntStream.range(1, 10000).toArray()) {
    Thread.sleep(1000);
    long currentTime = System.currentTimeMillis();
    System.out.println("Counter: " + counter + "(passed " + (currentTime-startTime) + " ms)");
    startTime = currentTime;
  }
}
```

---

# Docker...

<v-click>...is not that simple</v-click>
<v-click at=2>

```docker {none|1|3|5|6}
FROM bellsoft/liberica-runtime-container:jdk-crac-slim

ADD Example.java /app/Example.java
WORKDIR /app
RUN javac Example.java
ENTRYPOINT java -XX:CRaCCheckpointTo=/app/checkpoint Example
```

</v-click>

<v-click>This does not create the checkpoint yet!</v-click>

---

# And then

<div></div>

<v-click>

Build it

```bash
docker build -t pre_crack -f crac2/Dockerfile crac2
```

</v-click>
<v-click>

Run it

````md magic-move
```bash
docker run -d pre_crack
```

```bash
docker run -d pre_crack
# will fail
```

```bash
docker run --privileged -d pre_crack
```

```bash
docker run --privileged -d pre_crack
# will work, but security folks will hate us
```

```bash
docker run --cap-add CAP_SYS_PTRACE --cap-add CAP_CHECKPOINT_RESTORE -d pre_crack
```

```bash
docker run --cap-add CAP_SYS_PTRACE --cap-add CAP_CHECKPOINT_RESTORE -d pre_crack
# Not so frightening if you think about it
```
````

</v-click>

<v-click>

> - `CAP_SYS_PTRACE`: we need to access the whole process tree
>
> transfer data to or from the memory of arbitrary processes using `process_vm_readv(2)` and `process_vm_writev(2)`

</v-click>
<v-click>

> - `CAP_CHECKPOINT_RESTORE`: somehow there is a special cap for this
>
> Update `/proc/sys/kernel/ns_last_pid`; Read the contents of the symbolic links in `/proc/pid/map_files` for other processes

</v-click>

---

# And then

Checkpoint it

```bash {none|1|3|5|6}
ID=$(docker run --cap-add CAP_SYS_PTRACE --cap-add CAP_CHECKPOINT_RESTORE -p8080:8080 -d pre_crack)

# wait some time

docker exec -it $ID jcmd 129 JDK.checkpoint
docker commit $ID cracked
```

<v-click>

Now we're ready!

```bash {none|1|2|3|4}
docker run --rm -d \
    --entrypoint java \
    --network host cracked:latest \
    -XX:CRaCRestoreFrom=/app/checkpoint
```

</v-click>

---
layout: statement
---

# And it just works!

<v-click><h2>Until it doesn't</h2></v-click>

---

# In our startup

<v-clicks>

- `bot-assistant` module just works
- `chat-api` doesn't work!

Only some projects are guaranteed to work.

Others... Request support from the <logos-spring-icon /> team: <logos-mongodb-icon /> is not supported for now :(

</v-clicks>

---

# What are the options?

<v-clicks>

- Fall back to another solution
- Request support
- Implement support on our own!

</v-clicks>

---

# Implementing support on our own.

Custom `MongoClient`

```java {1|2-5|6-8|9}
public class MongoClientProxy implements MongoClient {
    volatile MongoClient delegate;
    public MongoClientProxy(MongoClient initialClient) {
        this.delegate = initialClient;
    }
    public void close() {
        delegate.close();
    }
    // Delegate everything else the same way
```

---

# Implementing support on our own

Custom CRaC Resource

```java {1,2|7-11|4,8|5,9|10|13-16|18-21|20}{maxHeight:'260px'}
@Component
static public class MongoClientResource implements Resource {

    private final MongoClientProxy mongoClientProxy;
    private final MongoConnectionDetails details;

    public MongoClientResource(MongoClient mongoClientProxy, MongoConnectionDetails details) {
        this.mongoClientProxy = (MongoClientProxy) mongoClientProxy;
        this.details = details;
        Core.getGlobalContext().register(this);
    }

    @Override
    public void beforeCheckpoint(Context<? extends Resource> context) {
        mongoClientProxy.delegate.close();
    }

    @Override
    public void afterRestore(Context<? extends Resource> context) {
        mongoClientProxy.delegate = MongoClients.create(details.getConnectionString());
    }
}
```

---

# Implementing support on our own

Replacing `MongoClient` in the context

```java {1,3|4|2,5}
@Bean
@Primary
public MongoClient mongoClient(MongoConnectionDetails details) {
    MongoClient initialClient = MongoClients.create(details.getConnectionString());
    return new MongoClientProxy(initialClient);
}
```

---
layout: image
image: /charts/crac.svg
---


---
class: text-center
layout: cover
background: /Bg-1.png
---

## And they all lived happily ever after!

---
layout: image
image: /Bg-8.png
---

# Quick Recap

Which hero are you?


🛡 → AppCDS for the smoothest integration <br>
🧙‍♂️ → Project Leyden EA builds to prepare to use it in the future <br>
🎒 → Native Image for fast start <br>
✊🏼 → CRaC for almost instant start <br>

<br>
Testing locally? Use a power cord!

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
class: text-center
layout: cover
background: /Bg-1.png
---

## This story came to an end, yours has just begun

<br>

## Go for it!

---
layout: image-right
image: "/qr.png"
---

# Thank you for your attention!

Find us at

- <logos-bluesky /> @asm0dey.site
- <logos-bluesky /> @cat-edelveis.bsky.social
- <logos-twitter /> asm0di0
- <logos-twitter /> cat_edelveis
