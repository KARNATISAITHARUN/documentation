# Integrating with Docker Builds

Many microservices built today are containerized, with a large portion built and deployed as Docker containers. This section describes how the Shiftleft CLI and the ShiftLeft Microagent can be integrated during Docker image builds and deployments.

## Definitions

- **Target Application** : The JAR artifact of the target application built using either a custom build script or a CI such as Jenkins or Travis CI. In this section, we will identify this as a Java application called `app.jar`
 
- **Target Image**: The container image that is run in production which contains the target jar. The ShiftLeft CLI and ShiftLeft Microagent will be configured into this Docker image. 

- **Dockerfile**: The Dockerfile used to build the target image. In order to provision the Shiftleft CLI and ShiftLeft Microagent in the target image, this file is customized prior to a `docker build` command.

## Deployment Options

There are two ways to integrate the complete ShiftLeft solution with the containerized target application:

* **Container-only**: Both ShiftLeft code analysis and ShiftLeft's microagent execute from within the container during application container run.
* **Host + Container**: Shiftleft code analysis is carried on the host prior to application container run. During container run, only the ShiftLeft Microagent is run for each execution.

## Option 1. Container-only Mode
In this mode, during the container build, we need to prepare the image with all ShiftLeft dependencies bundled in it with the application. We first need to fetch the `sl` binary and copy the ShiftLeft configuration (`config.json`) generated from the authentication steps outlined in [Authenticating with ShiftLeft](../using-cli/authenticating.md). There can be two ways to achieve the container-only solution.

### Method A 
In the first method, we would separately do code analysis and application execution with `sl`. A typical Dockerfile that performs these steps during for a target app (`app.jar`) has been shown below.

#### Example Dockerfile 

```Dockerfile
FROM alpine
WORKDIR /usr/src/app

RUN apk --update --no-cache add curl openjdk8

COPY app.jar /user/src/app/app.jar

# Install ShiftLeft CLI
RUN curl https://cdn.shiftleft.io/download/sl-latest-linux-x64.tar.gz | tar xvz -C /usr/local/bin
ENV SHIFTLEFT_ORG_ID=...
ENV SHIFTLEFT_ACCESS_TOKEN=...

# Run ShiftLeft code analysis
RUN sl analyze --wait --app MyApplication app.jar

# Run target app with ShiftLeft Microagent
CMD sl run -- java -jar app.jar
```

### Method B

In the second method, we can avoid the need to separately analyze the project and bundle it as part of the `sl run` command. This also allows us to specify an application name such as _MyApplication_ using the `--app` option to `sl`. A sample Dockerfile is shown below

#### Example Dockerfile 

```Dockerfile
FROM alpine
WORKDIR /usr/src/app

RUN apk --update --no-cache add curl openjdk8

COPY app.jar /user/src/app/app.jar

# Install ShiftLeft CLI
RUN curl https://cdn.shiftleft.io/download/sl-latest-linux-x64.tar.gz | tar xvz -C /usr/local/bin
ENV SHIFTLEFT_ORG_ID=...
ENV SHIFTLEFT_ACCESS_TOKEN=...

# Do ShiftLeft code analysis and run target app with ShiftLeft Microagent
CMD sl run --app MyApplication --analyze app.jar -- java -jar app.jar
``` 

## Option 2. Host + Container Mode

In this mode, the code analysis performed by Shiftleft CLI (`sl`) is performed on the host immediately after the target jar is built by the CI and before the target Docker image is built. For example, If you are using Maven as the build system and Jenkins as the CI, the analysis can be added as a post-build step using the [Exec Maven Plugin](https://www.mojohaus.org/exec-maven-plugin/). As an example, an excerpt from the `pom.xml` file for the Maven project is shown below. 

### Example `pom.xml` Snippet

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.6.0</version>
    <executions>
        <execution>
            <id>shiftleft-analyze</id>
            <phase>package</phase>
            <configuration>
                <executable>sl</executable>
                <workingDirectory></workingDirectory>
                <arguments>
                    <argument>analyze</argument>
                </arguments>
                <arguments>
                    <argument>--wait</argument>
                </arguments>
            </configuration>
            <goals>
                <goal>exec</goal>
            </goals>
        </execution>
    </executions>
</plugin>

```
For any other build system, all we need to remember is to analyze on the host beforehand using `sl analyze --wait` such that the Application Configuration  `shifteft.json` file is generated in the same directory. Note that this file is distinct from the `config.json` file, which is generated after a successful ShiftLeft authentication. In the target image building steps that follow, we would need to copy the `shiftleft.json` file in the container image so that it can be used when the application is run. A sample Dockerfile that does this has been presented below.    

### Example Dockerfile

```Dockerfile
FROM alpine
WORKDIR /usr/src/app

RUN apk --update --no-cache add curl openjdk8

COPY app.jar /user/src/app/app.jar

# Install ShiftLeft CLI
RUN curl https://cdn.shiftleft.io/download/sl-latest-linux-x64.tar.gz | tar xvz -C /usr/local/bin

# Copy ShiftLeft Application Configuration app directory
COPY shiftleft.json /usr/src/app/.shiftleft.json

# Run target app with ShiftLeft Microagent
CMD sl run -- java -jar app.jar
```
