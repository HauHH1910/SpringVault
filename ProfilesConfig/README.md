# Profile Config
## Description:
In this tutorial, we’ll focus on introducing Profiles in Spring.

Profiles are a core feature of the framework — allowing us to map our beans to different profiles <br/>
— for example:  dev, test, and prod.

## Dependency:
```maven
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```
## Setup
When we start Spring Boot Application, we can see that Spring say that: No active profile set
<a href="https://imgur.com/FC5pqxm"><img src="https://i.imgur.com/FC5pqxm.png" title="source: imgur.com" /></a>
<br>
So how can we activate profiles?

### Maven

We set up a new profile using `<profiles></profiles>` in the `pom.xml` file.

We can have three profiles: dev, test, prod and dev is the default

```maven
<profiles>
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <profiles.active>dev</profiles.active>
        </properties>
    </profile>
    <profile>
        <id>test</id>
        <properties>
            <profiles.active>test</profiles.active>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <profiles.active>prod</profiles.active>
        </properties>
    </profile>
</profiles>
```

and we need to enable resource filtering in `pom.xml`:
```maven
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

### Application file

After setup in the `pom.xml`, we have to use `application.yaml` or `application.properties` to activate the profiles.
You can use `yaml` or `properties` file. But I choose `yaml` file.
<br>
We need to create 4 application files:
- `application.yaml`: used for communication and is considered as the parent file of other application files.
- `application-dev.yaml`: used for **dev** environment
- `application-test.yaml`: used for **test** environment
- `application-prod.yaml`: used for **prod** environment
---
`applcation.yaml` file

```yaml 
spring:
  profiles:
    active: @profiles.active@
```
`profiles.active`: it the placeholder we declare in `pom.xml` used to activate profile.

`applcation-dev.yaml` file
```yaml
server:
  port: 8081
```

`applcation-test.yaml` file
```yaml
server:
  port: 8082
```

`applcation-prod.yaml` file
```yaml
server:
  port: 8083
```

## Test Application

When we run the application, we expect that the `dev` environment it activates, with the port is: `8081`.

<a href="https://imgur.com/VzfpPA4"><img src="https://i.imgur.com/VzfpPA4.png" title="source: imgur.com" /></a>

Congratulation, successfully active profiles `dev`.

### How can we change profile
1. Using IntelliJ
<br>
<a href="https://imgur.com/lq4vK5c"><img src="https://i.imgur.com/lq4vK5c.png" title="source: imgur.com" /></a>
<br>
select profiles you want to choose, and click on the button `Reload All Maven Projects`
<br>
<a href="https://imgur.com/OuKqNlG"><img src="https://i.imgur.com/OuKqNlG.png" title="source: imgur.com" /></a>
2. Using CLI
<br>
Open Terminal:
<br>

```bash
mvn clean install -P prod
```
-P {profiles}: this it for your profiles, depend on you what do you want to choose.
<br>

```bash
mvn clean install -P prod -DskipTests
```
-DskipTests: this it for skip test

After running finish, we will have the `-jar` file in the `target` folder
<br>
<a href="https://imgur.com/xKjxNEP"><img src="https://i.imgur.com/xKjxNEP.png" title="source: imgur.com" /></a>

#### Tips:
You can edit .jar name, in the file `pom.xml`, go to the `<build></build>` tag
<br>
We can put `<finalName></finalName>` for your jar file 
```maven
<finalName>profiles-config</finalName>
```
Running again.
```bash 
mvn clean install -P prod
```
<br>
<a href="https://imgur.com/s8CEd7t"><img src="https://i.imgur.com/s8CEd7t.png" title="source: imgur.com" /></a>

Check again it works or not!!

```bash
java -jar target/profiles-config.jar
```
Result:
<br>
<a href="https://imgur.com/NeeuBly"><img src="https://i.imgur.com/NeeuBly.png" title="source: imgur.com" /></a>
