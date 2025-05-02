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
canvasWidth: 800
colorSchema: 'dark'
---

## Four Approaches to Reducing Java Startup Time:

## AppCDS, Native Image, Project Leyden, CRaC

<img src="/pics/bellsoft.png" width="200px" class="absolute right-10px bottom-5px"/>

<style>
h2 {
    font-size: 40px;
    line-height: 1.2;
}
</style>

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
    font-size: 20px;
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
    font-size: 20px;
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
image: 'pics/logo1.png'
---

# About BellSoft

* Liberica JDK Vendor
* OpenJDK Contributor
* <img src="/pics/logos/openjdk.svg" width="70px"/> <img src="/pics/logos/graalvm.svg" width="70px"/> <img src="/pics/logos/cncf.svg" width="20px"/> <img src="/pics/logos/linux.svg" width="20px"/> Member
* Alpaquita Linux Developer
* ARM32 Java Port Author
* Own base images

<br>
Liberica is the JDK officially recommended by Spring

<br><br>
<v-click><b>We know what's up!</b></v-click>

<style>
h1 {
    font-weight: bold;
    font-size: 30px;
    color: #FFFFFF;
}

ul {
    font-size: 14px;
}

div {
    background-image: url("pics/Bg-18.png");
    background-size: 100%;
}
img {
float: left;
margin-right: 5px;
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
background: pics/Bg-1.png
---

# But first, four quick questions ;)

---
class: text-center
layout: cover
background: pics/Bg-1.png
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
background: pics/Bg-1.png
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
background: pics/Bg-1.png
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
background: pics/Bg-1.png
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
background: pics/Bg-1.png
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
image: pics/Bg-6-r.png
---

# Our startup is doing well!

<br>

- Cutting-edge tech:
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
image: pics/Bg-6.png
---

# Starting point: `Dockerfile`

<br>

```docker {none|1|8|10|17|14|all}{maxHeight:'300px'}
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
    font-size: 34px;
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

# Starting point: Startup, Mem usage, image size

<br>

````md magic-move
```bash {1|2|3|all}
docker compose logs chat-api bot-assistant | grep Started
bot-assistant-1  | Started BotAssistantApplication in 2.108 seconds (process running for 3.181)
chat-api-1       | Started ChatApiApplication in 2.83 seconds (process running for 3.469)
```
```bash {1|3|4|all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM % 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%
```
```bash {1|3|4|all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
````


<style>
h1 {
    font-size: 34px;
    text-align: right;
    font-weight: bold;
    color: #FFFFFF;
}
</style>


---
layout: image
image: pics/Bg-6-r.png
---

# We are growing!

<br>

- So many new users!
- Scale the services to meet the traffic

<br>

<div v-click class="text-xl"> But wait... </div>
<br>
<div v-click class="text-xl"> Why are we paying so much? </div>

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
image: pics/Bg_18.png
---

# Something is rotten in our cloud infrastructure!


<div v-click class="text-xl"> Inflated cloud bills: management is not happy </div>
<div v-click class="text-xl"> Services eat away at resources </div>
<div v-click class="text-xl"> Scaling is a nightmare </div>
<br><br><br>
<div v-click class="text-xl"><b> Maybe Java is not cut out for the cloud? </b></div>
<div v-click class="text-xl"><b> Maybe it's not too late to migrate to Go? </b></div>


<style>
h1 {
    font-weight: bold;
    font-size: 34px;
    color: #FFFFFF;
}

div {
    font-size: 20px;
}
</style>


---
class: text-center
layout: cover
background: pics/Bg-4.png
---

# Four heroes, one story 

---
layout: image
image: pics/Bg-4.png
---

> # "I want painless integration, no incompatibilities!"

<br>

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
image: pics/Bg-12-r.png
---

# Problem


- Application uses thousands of classes
- Classes are loaded every time the app starts
- Can take several seconds
- The process repeats upon each start!

<br>
<v-click><b>DRY!</b></v-click>

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>


---
layout: image
image: pics/Bg-12-r.png
---

# Solution: AppCDS


- Archive of JVM and application classes
- Created dynamically upon app exit
- Supported since Spring Boot 3.3
- Even better with Spring AOT <br> (load even more classes!)

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
</style>

---
layout: image
image: pics/Bg-12-r.png
---

# AppCDS: Any Considerations?


- Use the same JVM
- Use the same classpath
- Bigger Docker image due to the archive
  <br><br>
<v-click>Let's take it for a spin!</v-click>

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    text-align: left;
    font-size: 24px;
}
</style>

---
layout: image
image: pics/bg-0.png
---

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

---
layout: image
image: pics/bg-0.png
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
bot-assistant-1  | Started BotAssistantApplication in 1.19 seconds (process running for 1.416)
chat-api-1       | Started ChatApiApplication in 1.721 seconds (process running for 1.887)

```
````
<br>
<v-click at="1">
Startup time (sum): 6.65 s -> 3.3 s<br>
-50 %
</v-click>


---
layout: image
image: pics/bg-0.png
---


````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM % 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66% 
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %
2a937f492bdb   hero-guide-chat-api-1        0.40%     221.7MiB / 15.59GiB   1.39%
48a758a1a974   hero-guide-bot-assistant-1   0.42%     208.9MiB / 15.59GiB   1.31%
```
````
<br>
<v-click at="1">
Memory usage (sum): ~ 522.5 MiB -> 429 MiB <br>
-17.8 %
</v-click>

---
layout: image
image: pics/bg-0.png
---

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
hero-guide-chat-api                        latest    196046885842   3 minutes ago   288MB
hero-guide-bot-assistant                   latest    7f5b7ba8d8cd   5 minutes ago   300MB
```
````

<br>
<v-click at="1">
Image size (sum): 411 MB -> 588 MB<br>
+30 %
</v-click>


---
layout: image
image: pics/Bg-4.png
---

> # "We need to prepare ourselves and gather knowledge!"

<br>

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
image: pics/Bg-16.png
---

# Problem

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
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>

---
layout: image
image: pics/Bg-16.png
---

# Solution: Project Leyden


- Beyond AppCDS: AOT Cache
- Shift some computations<br> from production run to earlier stage
- Condensers: shifting, constraining,<br> and optimizing transformations
- Flexibly choose which condensers to<br> apply


<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>

---
layout: image
image: pics/Bg-16.png
---

# Project Leyden: Any Considerations?


- Still in the makings
- JEP 483: Ahead-of-Time Class Loading & Linking<br> (JDK 24)
- The more constraints applied,<br> the better startup/warmup

<br>
<v-click>Meanwhile, we can experiment!</v-click>

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>


---
layout: image
image: pics/bg-0.png
---

```docker {none|1,4,5,6|8,12,15,18|20,24,25,28|30,35-39|41,42|44}{maxHeight:'300px'}
FROM bellsoft/alpaquita-linux-base:glibc AS downloader

RUN apk add curl tar
RUN curl https://download.java.net/java/early_access/leyden/2/openjdk-24-leyden+2-8_linux-x64_bin.tar.gz -o /java.tar.gz && \
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
bot-assistant-1  | Started BotAssistantApplication in 0.879 seconds (process running for 1.16)
chat-api-1       | Started ChatApiApplication in 1.096 seconds (process running for 1.397)
```
````
<br>
<v-click at="1">
Startup time (sum): 6.65 s -> 2.55 s<br>
-61 %
</v-click>

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM % 
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O        PIDS 
73e3b379ce87   hero-guide-chat-api-1        0.33%     274.9MiB / 15.62GiB   1.72%     7.28kB / 5.95kB   0B / 463kB       56 
a96020d0d27d   hero-guide-bot-assistant-1   0.54%     245.6MiB / 15.62GiB   1.54%     894B / 126B       0B / 463kB       46 

```
````

<br>
<v-click at="1">
Memory usage (sum): ~ 522.5 MiB -> 519 MiB <br>
-0.58 %
</v-click>


---
layout: image
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
hero-guide-bot-assistant                   latest    86f46df9228f   9 minutes ago  213MB
hero-guide-chat-api                        latest    f3f6a1c2da35   2 minutes ago  198MB
```
```bash {all}
docker images
REPOSITORY                                 TAG            IMAGE ID       CREATED         SIZE
hero-guide-bot-assistant                   latest         c19d2aaa7125   2 minutes ago   586MB
hero-guide-chat-api                        latest         e62e63a37c00   2 minutes ago   573MB
```
````

<br>
<v-click at="1">
Image size (sum): 411 MB -> 1159 MB<br>
+64 %
</v-click>


---
layout: image
image: pics/Bg-4.png
---

> # "I found something exciting... Can't wait to explore!"

<br>

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
image: pics/Bg-13.png
---

# Problem


- Warmup may be much longer than startup
- Compile and optimize code<br> at runtime -> N minutes
- More memory for profile data<br> and bytecode cache

<br>

<v-click>Compile and optimize<br>
at build time?</v-click>

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>

---
layout: image
image: pics/Bg-13.png
---

# Solution: GraalVM Native Image


- Ahead-of-time compilation
- Closed world assumption
- Standalone executable (.exe)
- No OpenJDK required to run
- The app starts at peak performance

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
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


- Resource-demanding build process
- Several minutes to build the image
- Special treatment of Java's dynamism

<br>
<v-click>Let the journey begin!</v-click>

<style>
h1 {
    font-size: 34px;
    text-align: left;
    font-weight: bold;
    color: #FFFFFF;
}
ul {
    text-align: left;
    font-size: 24px;
}
div {
    font-size: 24px;
}
</style>

---
layout: image
image: pics/bg-0.png
---

# First, let's try it locally

Build a fat JAR:

```{all}{maxHeight:'200px'}
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
    font-size: 34px;
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


---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br><br><br>
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


Apparently, [a known issue](https://github.com/oracle/graal/issues/7034)<br>

<img src="/pics/zip-ex.png"/>

➡️ Use ```mvn -Pnative native:compile``` to build the image.<br><br>

<v-click><b>🤷‍♀️ Alright, let's try with a plugin.</b></v-click>

---
layout: image
image: pics/bg-0.png
---

Add ```native``` profile:
```{all|6,7,8|21|22-24|27-29}{maxHeight:'200px'}
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
image: pics/Bg_21.png
---

# Oops!
<br><br><br>
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
<img src="/pics/mkay.png" width="100px" class="absolute right-60px bottom-50px"/>
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
```{none|23}{maxHeight:'200px'}
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

<br>

<v-click>We found the culprit!</v-click>


---
layout: image
image: pics/bg-0.png
---

Alright, let's add the option to initialize ```org.springframework.http.codec.xml.XmlEventDecoder``` at runtime:
```{none|23}{maxHeight:'200px'}
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
image: pics/Bg_21.png
---

# Oops!
<br><br><br>
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
<img src="/pics/facepalm.png" width="100px" class="absolute right-60px bottom-50px"/>
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


---
layout: image
image: pics/bg-0.png
---

Let's add the ```--strict-image-heap``` option:
```{none|23}{maxHeight:'200px'}
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

## <v-click>Apparently, a JFR event is created when the user sends<br> a request to the AI Bot</v-click>

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
layout: image
image: pics/bg-0.png
---

Let's enable JFR:
```{none|23}{maxHeight:'200px'}
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

## Let's move on to building a native image in a Docker container.

### <v-click>Piece of cake, right? We solved all issues..</v-click>

<v-click>
<img src="/pics/star-wars.png" width="300px" class="center"/>
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
layout: image
image: pics/bg-0.png
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
image: pics/Bg_21.png
---

# Oops!
<br>

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
<br>

<v-click>
<img src="/pics/eye-roll.png" width="100px" class="absolute right-60px bottom-50px"/>
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

[A bug?](https://github.com/abertschi/graalphp/pull/39)<br>

<img src="/pics/bailout-ex.png"/>

<br>

<v-click><b>🤔 Doesn't look like MY problem</b></v-click>


---
layout: image
image: pics/bg-0.png
---
The process is so slow it fails.<br>
[Wrong Colima settings](https://github.com/abiosoft/colima/issues/204)<br>

<img src="/pics/colima-ex.png"/>

➡️ Switching from ```colima start --vm-type=vz``` to ```colima start --vm-type=qemu``` should do the trick.

<br>

<v-click><b>Fingers crossed or what?</b></v-click>


---
layout: image
image: pics/Bg_21.png
---

# Oops!
<br>

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
520.7 Configured with: /ws/workspace/aq-build-pkg/aports/core/gcc/src/gcc-14.2.0/configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --build=aarch64-alpaquita-linux-musl --host=aarch64-alpaquita-linux-musl --target=aarch64-alpaquita-linux-musl --enable-checking=release --disable-fixed-point --disable-libstdcxx-pch --disable-multilib --disable-nls --disable-werror --disable-symvers --enable-__cxa_atexit --enable-default-pie --enable-languages=c,c++,d,objc,go,fortran,ada --enable-link-serialization=2 --enable-linker-build-id --with-arch=armv8-a --with-abi=lp64 --disable-libquadmath --disable-libssp --disable-libsanitizer --disable-cet --enable-gnu-indirect-function=yes --enable-shared --enable-threads --enable-tls --with-bugurl='https://bell-sw.com/support/' --with-system-zlib --with-linker-hash-style=gnu --with-pkgversion='Alpaquita 14.2.0'
```
<br>

<v-click>
<img src="/pics/crazy.png" width="200px" class="absolute right-60px bottom-30px"/>
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

## Linking issue: musl libc lacks required tools 

<br>

- Option 1: add required packages (libstc++, etc.) with ```apk add```
- <span v-mark.highlight.green="1"> Option 2: switch from musl-based Alpaquita to glibc-based </span>

<br>

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
background: pics/Bg-1.png
---

# We did it!

## <v-click>Both services compile and run successfully</v-click>

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

---
layout: image
image: pics/bg-0.png
---

````md magic-move
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %
d35ad859fef3   hero-guide-chat-api-1        0.21%     264.4MiB / 15.59GiB   1.66%
49a09ecb715d   hero-guide-bot-assistant-1   0.43%     258.1MiB / 15.59GiB   1.62%
```
```bash {all}
docker stats
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %
cdb9e2555739   hero-guide-chat-api-1        0.03%     94.24MiB / 15.59GiB   0.59%
0aa86864bd06   hero-guide-bot-assistant-1   0.20%     90.71MiB / 15.59GiB   0.57%
```
````
<br>
<v-click at="1">
Memory usage (sum): ~ 522.5 MiB -> 185 MiB <br>
-64 %
</v-click>

---
layout: image
image: pics/bg-0.png
---

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
<v-click at="1">
Image size (sum): 411 MB -> 489 MB<br>
+15 %
</v-click>


---
class: text-center
layout: cover
background: pics/bg-0.png
---

<img src="/pics/frodo.png" class="center"/>


<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  
}
</style>


---
layout: image
image: pics/Bg-4.png
---

> # "We need drastic changes, I won't settle for half-measures!"

<br>

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
