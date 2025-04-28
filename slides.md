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
- Bigger Docker image due to archive
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
```docker {all}{maxHeight:'100px'}
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
```docker {11,16,17|24,25,26,27|28,29|23}{maxHeight:'100px'}
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
RUN java -Dspring.aot.enabled=true -XX:ArchiveClassesAtExit=./application.jsa /
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
- The cost: longer startup/warmup, bigger mem usage
- Not all can migrate to Native Image

<v-click>Shift some heavy-lifting tasks to another point in time?</v-click>

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

- Towards static images
- Shift some computations from production run to earlier stage
- Condensers: shifting, constraining, and optimizing transformations
- Flexibly choose which condensers to apply
- Beyond AppCDS: AOT Cache


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

# Project Leyden: Any Consideration?

<br>

- Still in the makings
- JEP 483: Ahead-of-Time Class Loading & Linking (JDK 24)
- The more constraints applied, the better startup/warmup


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