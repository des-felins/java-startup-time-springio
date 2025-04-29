---
theme: seriph
background: pics/Bg-2.png
title: Four Approaches to Reducing Java Startup Time
layout: cover
class: text-center
drawings:
  persist: false
selectable: true
remoteAssets: true
canvasWidth: 1200
colorSchema: 'dark'
---

# Four Approaches to Reducing Java Startup Time:

# AppCDS, Native Image, Project Leyden, CRaC

<img src="/pics/bellsoft.png" width="200px" class="absolute right-10px bottom-5px"/>

---
layout: image-right
image: 'pics/cat.jpg'
---

<div v-click class="text-xl"> 🐈‍⬛ Catherine Edelveis aka Cat </div>
<br>
<div v-click class="text-xl"> 🥑 Developer Advocate at BellSoft </div>
<br>
<div v-click class="text-xl"> 😍 Love Java, Spring, JavaFX </div>
<br>
<div v-click class="text-xl"> 👩‍💻Tech writer </div>
<br>
<div v-click class="text-xl"> <img src="/pics/logos/x-2.png" width="30px"/> cat_edelveis </div>
<br>
<div v-click class="text-xl"> <img src="/pics/logos/bluesky.svg" width="30px"/> cat-edelveis.bsky.social </div>

<style>

div {
    font-size: 30px;
}
img {
float: left;
margin-right: 5px;
}
</style>

---
layout: image-left
image: 'pics/pasha.jpeg'
---

<div v-click class="text-xl"> Pasha Finkelshteyn </div>
<br>
<div v-click class="text-xl"> 🥑 Developer Advocate at BellSoft </div>
<br>
<div v-click class="text-xl"> 👨‍💻≈10 years in JVM: Mostly Java, Kotlin, Spring </div>
<br>
<div v-click class="text-xl"> <img src="/pics/logos/x-2.png" width="30px"/> asm0di0 </div>
<br>
<div v-click class="text-xl"> <img src="/pics/logos/bluesky.svg" width="30px"/> @asm0dey.site </div>

<style>
div {
    font-size: 30px;
}
img {
float: left;
margin-right: 5px;
}
</style>

---
class: text-center
layout: cover
background: pics/cat_pasha.png
---

# Together, we are...

## <v-click>The BellSoft's DevRel Gang!</v-click>

<style>
h1 {
    position: relative;
    bottom: 200px;
}
h2 {
    font-size: 48px;
    position: relative;
    bottom: 50px;
}
</style>

---
layout: image-right
image: 'pics/logo1.png'
---

# About BellSoft
<br>


* Liberica JDK Vendor
* OpenJDK Contributor
* [TODO: logos ] Member
* Alpaquita Linux Developer
* ARM32 Java Port Author
* Own base images

<br>
Liberica is the JDK officially recommended by <img src="/pics/logos/spring.svg" width="30px"/>

<br><br>
<v-click><b>We know what's up!</b></v-click>

<style>
h1 {
    font-weight: bold;
    font-size: 44px;
    color: #FFFFFF;
}
div {
    background-image: url("pics/Bg-18.png");
    background-size: 100%;
    font-size: 26px;
}
img {
display: inline-block;
margin-left: 5px;
}
</style>

---
class: text-center
layout: cover
background: pics/Bg-1.png
---

# Enough about us :)

## We are here today to tell you a story about...
<v-click>you!</v-click>

<style>
h1 {
    position: relative;
    bottom: 200px;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    color: #FFFFFF;
    font-size: 36px;
    position: relative;
    bottom: 50px;
}
div {
        font-size: 36px;
}
</style>

---
class: text-center
layout: cover
background: pics/Bg-1.png
---

# But first, a quick question:

<v-click>

## Who knows how to reduce the startup time

<br>

## of their Spring application?

</v-click>

<style>
h1 {
    position: relative;
    bottom: 200px;
}
h2 {
    color: #FFFFFF;
    font-size: 46px;
    position: relative;
    bottom: 50px;
}
</style>

---
class: text-center
layout: cover
background: pics/Bg-1.png
---

# Once upon a time...

---
layout: image
image: pics/Bg_18.png
---

# Something is rotten in our cloud infrastructure!

<br>

<div v-click class="text-xl"> Inflated cloud bills: management is not happy </div>
<br>
<div v-click class="text-xl"> Services eat away at resources </div>
<br>
<div v-click class="text-xl"> Scaling is a nightmare </div>
<br><br><br>
<div v-click class="text-xl"><b> Maybe Java is not cut out for the cloud? </b></div>
<br>
<div v-click class="text-xl"><b> Maybe it's not too late to migrate to Go? </b></div>


<style>
h1 {
    font-weight: bold;
    font-size: 34px;
    color: #FFFFFF;
}

div {
    font-size: 26px;
}
</style>

---
layout: image
image: pics/Bg-6.png
---

# Four heroes, one story.

<br>

## Starting point: `Dockerfile`

<br><br>

```docker {none|1|8|10|17|14|all}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-21-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
COPY --from=builder /app/${project}/target/*.jar /app/app.jar
```

<style>
h1 {
    font-size: 44px;
    text-align: right;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    text-align: right;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/Bg-6.png
---

# Four heroes, one story.

<br>

## Starting point: Startup, Mem usage, image size

<br><br>

````md magic-move
```bash {1|2|3|all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
chat-api-1       | Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```
```bash {1|3|4|all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%     236kB / 1MB      0B / 1.41MB     48 
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%     1.6MB / 42kB     0B / 1.47MB     41 
```
```bash {1|3|4|all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
````
<br><br>
<span v-click="12">What can we do about that?</span>


<style>
h1 {
    font-size: 44px;
    text-align: right;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    text-align: right;
}
div {
    font-size: 34px;
}
</style>


---
class: text-center
layout: cover
background: pics/Bg-4.png
---

# First hero:

<br>

## <v-click>"I want painless integration, no incompatibilities!"</v-click>

<style>
h2 {
font-size: 36px;
}
</style>

---
layout: image
image: pics/Bg-12-r.png
---

# Problem

<br>

- Application uses thousands of classes
- Classes are loaded every time the app starts
- Can take several seconds
- The process repeats upon each start!

<br>
<v-click>DRY!</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>


---
layout: image
image: pics/Bg-12-r.png
---

# Solution: AppCDS

<br>

- Archive of JVM and application classes
- Created dynamically upon app exit
- Supported since Spring Boot 3.3
- Even better with Spring AOT <br> (load even more classes!)

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/Bg-12-r.png
---

# AppCDS: Any Considerations?

<br>

Very few constraints:
<br><br>
- Use the same JVM
- Use the same classpath
- Bigger Docker image due to the archive
  <br><br>
<v-click>Let's take it for a spin!</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```{7,8,9}
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
```docker {all}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-21-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
COPY --from=builder /app/${project}/target/*.jar /app/app.jar
```
```docker {11,16,17|24,25,26,27|28,29|23}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder

ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-21-cds-musl as optimizer
ARG project
ENV project=${project}

WORKDIR /app
COPY --from=builder /app/${project}/target/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --launcher

FROM bellsoft/liberica-runtime-container:jre-21-cds-musl

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-Dspring.aot.enabled=true", "-XX:SharedArchiveFile=application.jsa", "org.springframework.boot.loader.launch.JarLauncher"]
COPY --from=optimizer /app/app/dependencies/ ./
COPY --from=optimizer /app/app/spring-boot-loader/ ./
COPY --from=optimizer /app/app/snapshot-dependencies/ ./
COPY --from=optimizer /app/app/application/ ./
RUN java -Dspring.aot.enabled=true -XX:ArchiveClassesAtExit=./application.jsa \
-Dspring.context.exit=onRefresh org.springframework.boot.loader.launch.JarLauncher
```
````

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
chat-api-1       | Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```
```bash {all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 1.317 seconds (process running for 1.536)
chat-api-1       | Started ChatApiApplication in 1.721 seconds (process running for 1.887)
```
````
<br>
<v-click at="1">
Startup time (sum): 6.65 s -> 3.42 s<br>
-48 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%     236kB / 1MB      0B / 1.41MB     48 
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%     1.6MB / 42kB     0B / 1.47MB     41 
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 
51cac621a779   hero-guide-chat-api-1        0.53%     256.1MiB / 15.59GiB   1.60%     15.1kB / 8.34kB  0B / 225kB      46 
a020f5e32c2c   hero-guide-bot-assistant-1   0.52%     241.9MiB / 15.59GiB   1.52%     85.3kB / 2.76kB  0B / 242kB      40 
```
````
<br>
<v-click at="2">
Memory usage (sum): ~ 522.5 MiB -> 498 MiB <br>
-4 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED         SIZE
hero-guide-chat-api                        latest    196046885842   3 minutes ago   283MB
hero-guide-bot-assistant                   latest    7f5b7ba8d8cd   5 minutes ago   292MB
```
````

<br>
<v-click at="3">
Image size (sum): 411 MB -> 575 MB<br>
+39 %
</v-click>

---
class: text-center
layout: cover
background: pics/Bg-4.png
---

# Second hero:

<br>

## <v-click>"We need to prepare ourselves and gather knowledge!"</v-click>

<style>
h2 {
font-size: 36px;
}
</style>

---
layout: image
image: pics/Bg-16.png
---

# Problem

<br>

- Java's dynamism makes it powerful
- The cost:
  - Longer startup/warmup
  - Bigger mem usage
- Not all can migrate to Native Image

<br>

<v-click>Shift some heavy-lifting tasks<br>
to another point in time?</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/Bg-16.png
---

# Solution: Project Leyden

<br>

- Beyond AppCDS: AOT Cache
- Shift some computations<br> from production run to earlier stage
- Condensers: shifting, constraining,<br> and optimizing transformations
- Flexibly choose which condensers to apply


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/Bg-16.png
---

# Project Leyden: Any Considerations?

<br>

- Still in the makings
- JEP 483: Ahead-of-Time Class Loading & Linking<br> (JDK 24)
- The more constraints applied,<br> the better startup/warmup

<br>
<v-click>Meanwhile, we can experiment!</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/bg-0.png
---

```docker {all}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

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
image: pics/bg-0.png
---

```docker {none|1,4,5,|7,12,15,18|20,23,24,27|29,36-40|42,43|34}{maxHeight:'600px'}
FROM bellsoft/alpaquita-linux-base:glibc AS downloader

RUN apk add curl tar
RUN curl https://download.java.net/java/early_access/leyden/2/openjdk-24-leyden+2-8_linux-x64_bin.tar.gz -o /java.tar.gz
RUN cd / && tar -zxvf java.tar.gz && mv /jdk-24 /java

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

RUN /java/bin/java -Djarmode=tools -jar app.jar extract --layers --launcher

FROM bellsoft/alpaquita-linux-base:glibc AS runner

RUN apk add curl
WORKDIR /app

ENTRYPOINT ["/java/bin/java", "-XX:CacheDataStore=./application.cds", "org.springframework.boot.loader.launch.JarLauncher"]

COPY --from=downloader /java /java
COPY --from=optimizer /app/app/dependencies/ ./
COPY --from=optimizer /app/app/spring-boot-loader/ ./
COPY --from=optimizer /app/app/snapshot-dependencies/ ./
COPY --from=optimizer /app/app/application/ ./

RUN /java/bin/java -XX:CacheDataStore=./application.cds \
-Dspring.context.exit=onRefresh org.springframework.boot.loader.launch.JarLauncher
```

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
chat-api-1       | Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```
```bash {all}
docker compose logs chat-api bot-assistant | grep Started

```
````
<br>
<v-click at="1">
Startup time (sum): 6.65 s -> 3.42 s<br>
-48 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%     236kB / 1MB      0B / 1.41MB     48 
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%     1.6MB / 42kB     0B / 1.47MB     41 
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 

```
````

<br>
<v-click at="2">
Memory usage (sum): ~ 522.5 MiB -> 498 MiB <br>
-4 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED         SIZE

```
````

<br>
<v-click at="3">
Image size (sum): 411 MB -> 575 MB<br>
+39 %
</v-click>


---
class: text-center
layout: cover
background: pics/Bg-4.png
---

# Third hero:

<br>

## <v-click>"I found something exciting... Can't wait to explore!"</v-click>

<style>
h2 {
font-size: 36px;
}
</style>

---
layout: image
image: pics/Bg-13.png
---

# Problem

<br>

- Warmup may be much longer than startup
- Compile and optimize code<br> at runtime -> N minutes
- More memory for profile data<br> and bytecode cache

<br>

<v-click>Compile and optimize<br>
at build time?</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/Bg-13.png
---

# Solution: GraalVM Native Image

<br>

- Ahead-of-time compilation
- Closed world assumption
- Standalone executable (.exe)
- No OpenJDK required to run
- The app starts at peak performance

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/aot.png
---

---
layout: image
image: pics/Bg-13.png
---

# Native Image: Any Considerations?

<br>

- Resource-demanding build process
- Several minutes to build the image
- Special treatment of Java's dynamism

<br>
<v-click>Let the journey begin!</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
div {
    text-align: left;
    font-size: 34px;
}
</style>

---
layout: image
image: pics/bg-0.png
---

# First, let's try it locally

<br>

Build a fat JAR:

```{all}
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

<br>

```bash
mvn clean package
```

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: pics/bg-0.png
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

<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br><br>
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
image: pics/bg-0.png
---

<br>

Apparently, [a known issue](https://github.com/oracle/graal/issues/7034)<br>

<img src="/pics/zip-ex.png"/>

➡️ Use ```mvn -Pnative native:compile``` to build the image.<br><br>

<v-click><b>🤷‍♀️ Alright, let's try with a plugin.</b></v-click>


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: pics/bg-0.png
---

Add ```native``` profile:
```{all|6,7,8|21|22-24|27-29}{maxHeight:'300px'}
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


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br><br>
```bash{all|1,6}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.CommonConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.CommonConfig
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time. To see why com.ctc.wstx.stax.WstxInputFactory got initialized use --trace-class-initialization=com.ctc.wstx.stax.WstxInputFactory
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.ReaderConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.ReaderConfig
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time. To see why com.ctc.wstx.util.DefaultXmlSymbolTable got initialized use --trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable
To see how the classes got initialized, use --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
```
<br>

<v-click>
<img src="/pics/mkay.png" width="300px" class="absolute right-60px bottom-50px"/>
</v-click>

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
image: pics/bg-0.png
---

Add the ```--trace-class-initialization``` argument:
```{none|23}{maxHeight:'300px'}
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


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: pics/bg-0.png
---

More detailed output:
```bash{all|2}{maxHeight:'300px'}
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

<br><br>

<v-click>We found the culprit!</v-click>


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: pics/bg-0.png
---

Alright, let's add the option to initialize ```org.springframework.http.codec.xml.XmlEventDecoder``` at runtime:
```{none|23}{maxHeight:'300px'}
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


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>

---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br><br>
```bash{all}
Error: Classes that should be initialized at run time got initialized during image building:
 com.ctc.wstx.api.CommonConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.CommonConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.CommonConfig
com.ctc.wstx.stax.WstxInputFactory was unintentionally initialized at build time. To see why com.ctc.wstx.stax.WstxInputFactory got initialized use --trace-class-initialization=com.ctc.wstx.stax.WstxInputFactory
com.ctc.wstx.api.ReaderConfig was unintentionally initialized at build time. To see why com.ctc.wstx.api.ReaderConfig got initialized use --trace-class-initialization=com.ctc.wstx.api.ReaderConfig
com.ctc.wstx.util.DefaultXmlSymbolTable was unintentionally initialized at build time. To see why com.ctc.wstx.util.DefaultXmlSymbolTable got initialized use --trace-class-initialization=com.ctc.wstx.util.DefaultXmlSymbolTable
To see how the classes got initialized, use --trace-class-initialization=com.ctc.wstx.api.CommonConfig,com.ctc.wstx.stax.WstxInputFactory,com.ctc.wstx.api.ReaderConfig,com.ctc.wstx.util.DefaultXmlSymbolTable
```
<br>

The same error!

<v-click>
<img src="/pics/facepalm.png" width="300px" class="absolute right-60px bottom-50px"/>
</v-click>

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
image: pics/bg-0.png
---

Apparently, [another known issue](https://github.com/spring-projects/spring-framework/issues/31806#issuecomment-1862502951)<br>

<img src="/pics/xml-ex.png"/>

➡️ Need to add the ```--strict-image-heap``` option.<br><br>

<v-click><b>🫡 Will do!</b></v-click>


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: pics/bg-0.png
---

Let's add the ```--strict-image-heap``` option:
```{none|23}{maxHeight:'300px'}
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


<style>
h1 {
    font-size: 44px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
class: text-center
layout: cover
background: pics/Bg-1.png
---

# Success!

## <v-click>...Or is it?</v-click>


---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br><br>

```bash
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

## <v-click>Apparently, a JFR event is created when the user sends<br> a request to the AI Bot</v-click>

<style>
h1 {
    font-size: 44px;
    text-align: center;
    font-weight: bold;
    color: #FFFFFF;
}
h2 {
    font-size: 34px;
    padding-left: 80px;
}
div {
    bottom: 50px;
}
</style>


---
layout: image
image: pics/bg-0.png
---

Let's enable JFR:
```{none|23}{maxHeight:'300px'}
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
background: pics/Bg-1.png
---

# Finally, everything works!

## <v-click>Yeah, locally</v-click>


---
class: text-center
layout: cover
background: pics/Bg-1.png
---

# Let's move on to building a native image in a Docker container.

## <v-click>Piece of cake, right? We solved all issues..</v-click>

<v-click>
<img src="/pics/star-wars.png" width="350px" class="center"/>
</v-click>


<style>
h1 {
    position: relative;
    bottom: 50px;
}
h2 {
    font-size: 34px;
    position: relative;
    bottom: 50px;
}
.center {
  margin-left: auto;
  margin-right: auto;
}
</style>

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```docker {all}
FROM bellsoft/liberica-runtime-container:jdk-21-stream-musl as builder
ARG project
ENV project=${project}

WORKDIR /app
ADD ${project} /app/${project}
ADD ../pom.xml ./
RUN cd ${project} && ./mvnw -Dmaven.test.skip=true clean package

FROM bellsoft/liberica-runtime-container:jre-21-musl
ARG project
ENV project=${project}

RUN apk add curl
WORKDIR /app
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
COPY --from=builder /app/${project}/target/*.jar /app/app.jar
```
```docker {1,8|10,14,17|16}
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
````

<v-click>

Run:

```bash
docker compose up -d --build bot-assistant
```

</v-click>


---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br><br>
```bash{all}
2189.3 [6/8] Compiling methods...    [*************]                                                          (187.0s @ 5.54GB)
2189.3 
2189.3 Fatal error: org.graalvm.compiler.debug.GraalError: org.graalvm.compiler.core.common.PermanentBailoutException: Compilation exceeded 300.000000 seconds during CFG traversal
2189.3  at method: Future io.netty.resolver.AbstractAddressResolver.resolve(SocketAddress)  [Virtual call from Object AddressResolverGroupMetrics$DelegatingAddressResolver$$Lambda/0x60e14e76cf1bdb337c1b0dbb92d2d481fd9de99e0.get(), callTarget Future AddressResolver.resolve(SocketAddress)]
```
<br>

<v-click>
<img src="/pics/eye-roll.png" width="300px" class="absolute right-60px bottom-50px"/>
</v-click>

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
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
chat-api-1       | Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```
```bash {all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 0.806 seconds (process running for 1.029)
chat-api-1       | Started ChatApiApplication in 0.397 seconds (process running for 0.409)
```
````
<br>
<v-click at="1">
Startup time (sum): 6.65 s -> 1.44 s<br>
-78 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O       PIDS 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%     236kB / 1MB      0B / 1.41MB     48 
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%     1.6MB / 42kB     0B / 1.47MB     41 
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O        PIDS 
cdb9e2555739   hero-guide-chat-api-1        0.03%     94.24MiB / 15.59GiB   0.59%     10.4kB / 6.82kB   0B / 0B          28 
0aa86864bd06   hero-guide-bot-assistant-1   0.20%     90.71MiB / 15.59GiB   0.57%     51.8kB / 1.88kB   4.1kB / 0B       22 
```
````
<br>
<v-click at="2">
Memory usage (sum): ~ 522.5 MiB -> 185 MiB <br>
-64 %
</v-click>
<br><br>

````md magic-move
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED          SIZE
hero-guide-chat-api                        latest    8614ec84e9a5   27 seconds ago   229MB
hero-guide-bot-assistant                   latest    0f681e15cf08   19 minutes ago   260MB
```
````

<br>
<v-click at="3">
Image size (sum): 411 MB -> 489 MB<br>
+15 %
</v-click>



---
class: text-center
layout: cover
background: pics/Bg-4.png
---

# Fourth hero:

<br>

## <v-click>"We need drastic changes to stay ahead of the game!"</v-click>

<style>
h2 {
font-size: 36px;
}
</style>