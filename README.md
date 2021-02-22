# Maven demo

## Prerequisites

Most likely your environment is already configured to use Java and Maven. Follow the steps below to confirm it.

1. We need to have `Java 8+` installed. To verify installation run:
    ```
    java -version
    ```
    You should see something similar to this:
    ```
    openjdk version "11" 2018-09-25
    OpenJDK Runtime Environment 18.9 (build 11+28)
    OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)
    ```
    If you don't, go ahead and install Java and make sure to set your `PATH` environment variable correctly.
1. We need to have `mvn` installed. To verify installation run:
    ```
    mvn --version
    ```
    You should see something similar to this:
    ```
    Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
    Maven home: C:\Program Files\apache-maven\bin\..
    Java version: 11, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-11
    Default locale: en_US, platform encoding: Cp1252
    OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
    ```
    If you don't, go ahead and install Maven and make sure to set your `JAVA_HOME` environment variable correctly.
1. Since Maven Central is not supported by your organization, you need to make sure you point your Maven settings to the internal repository, such as Artifactory or the like. You can find the configuration file at:
    ```
    %USERPROFILE%\.m2\settings.xml
    ```
    The Maven configuration is out of scope of this demo, you can find more information on [the Maven documentation site](https://maven.apache.org/settings.html).
1. Optionally, you will need `git` and `python` to be able to checkout this repository and to run the generated documentation website locally.

## Demo 1 - manual

In this demo we will create a Maven based Java application literally from zero, and we will see how simple yet powerful it is.

1. Create and navigate to the project folder:
    ```
    md maven-demo-1
    cd maven-demo-1
    ```
1. Create an empty `pom.xml` file:
    ```
    fsutil file createnew pom.xml 0
    ```
1. Stub out a project definition that we will use to describe our project's lifecycle:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    </project>
    ```
1. Let's see if Maven considers that file valid:
    ```
    mvn validate
    ```
1. Let's add the required elements, such as `groupId`, `artifactId` and `version`:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>com.wellsfargo</groupId>
        <artifactId>maven-demo-1</artifactId>
        <version>1.0-SNAPSHOT</version>
    </project>
    ```
1. Now the validation will succeed. Run this one more time:
    ```
    mvn validate
    ```
1. Although we have a complete Maven based application, we have yet to write any code, so let's stub our tiny Java project:
    ```
    md src\main\java\com\wellsfargo
    md src\test\java\com\wellsfargo
    fsutil file createnew src\main\java\com\wellsfargo\App.java 0
    fsutil file createnew src\test\java\com\wellsfargo\AppTest.java 0
    ```
1. Now we need to add a little bit of Java code to make our application do something. Since we believe in TDD, we will start with a test:
    ```java
    package com.wellsfargo;

    import static org.junit.jupiter.api.Assertions.assertTrue;

    import org.junit.jupiter.api.Test;

    public class AppTest {

        @Test
        public void shouldPass() {
            assertTrue(true);
        }

    }

    ```
1. For the sake of simplicity, we will go ahead and add the actual implementation right away:
    ```java
    package com.wellsfargo;

    public class App {

        public static void main(String[] args) {
            System.out.println();
            System.out.println("+=====================+");
            System.out.println("| Hello Maven Demo 1! |");
            System.out.println("+=====================+");
        }

    }

    ```
1. Before we try to run the application, or even run the tests, we want to make sure it compiles. Let's run the following:
    ```
    mvn compile
    ```
    The compilation most likely (depending on your environment and global Maven settings) will fail.
1. We need to add a little bit more information about our application to the `pom.xml` file:
    ```xml
    ...

        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                </plugin>
            </plugins>
        </build>

    ...
    ```
1. Now let's try to run the tests against our application:
    ```
    mvn test
    ```
    It will fail due to a missing dependency. Let's add it:
    ```xml
    ...

        <dependencies>
            <dependency>
                <groupId>org.junit.jupiter</groupId>
                <artifactId>junit-jupiter-engine</artifactId>
                <version>5.7.1</version>
                <scope>test</scope>
            </dependency>
        </dependencies>

        <build>
            <plugins>
                ...
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.0</version>
                    <configuration>
                        <argLine>
                            --illegal-access=permit
                        </argLine>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-failsafe-plugin</artifactId>
                    <version>2.22.0</version>
                    <configuration>
                        <argLine>
                            --illegal-access=permit
                        </argLine>
                    </configuration>
                </plugin>
            </plugins>
        </build>

    ...
    ```
    and let's run the tests again:
    ```
    mvn test
    ```
1. Now we can create an executable `.jar` file using Maven to run our application and see the output:
    ```
    mvn package
    ```
    and try to run the app:
    ```
    java -jar target\maven-demo-1-1.0-SNAPSHOT.jar
    ```
    Although there is no error, the app will not actually run because when Maven packages it, it doesn't have enough information about the entrypoint. Let's provide it:
    ```xml
    ...

        <plugins>
            ...
            <plugin>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.wellsfargo.App</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>

    ...
    ```
    When we run the app again, it should run successfully and print the following output:
    ```

    +=====================+
    | Hello Maven Demo 1! |
    +=====================+

    ```

## Demo 2 - Automated

This demo is similar to the previous one but unlike the previous one will enable us to leverage some of the Maven built-in automation to create a project.

1. The following command will automatically generate and set up our new application:
    ```
    mvn archetype:generate "-DgroupId=com.wellsfargo" "-DartifactId=maven-demo-2" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DarchetypeVersion=1.4" "-DinteractiveMode=false"
    ```
    Let's navigate to the root of the project:
    ```
    cd maven-demo-2
    ```
1. Let's run some of the Maven commands from the previous demo:
    ```
    mvn validate
    mvn compile
    mvn test
    ```
1. Now we can change the code to make the application do what we want it to do:
    ```java
    ...
        System.out.println();
        System.out.println("+=====================+");
        System.out.println("| Hello Maven Demo 2! |");
        System.out.println("+=====================+");
    ...
    ```
    Also it is important to add configuration to the `jar` plugin, so we can generate an executable `jar` file as we did in the previous demo:
    ```xml
    ...
        <configuration>
            <archive>
                <manifest>
                    <mainClass>com.wellsfargo.App</mainClass>
                </manifest>
            </archive>
        </configuration>
    ...
    ```
1. Let's package the application:
    ```
    mvn package
    ```
    and run it:
    ```
    java -jar target\maven-demo-2-1.0-SNAPSHOT.jar
    ```
    and this is the output we expect to see:
    ```

    +=====================+
    | Hello Maven Demo 2! |
    +=====================+

    ```
1. Let's take advantage of more Maven automation. One more thing that we can do is generate a static website with documentation for our application:
    ```
    mvn site
    ```
    Let's navigate to it:
    ```
    cd target\site
    ```
    and run a local server:
    ```
    python -m http.server 8081
    ```

## Demo 3 - Spring

The purpose of this demo is to go through how Maven can benefit from being extended to support the 12-factor app scenario.

1. Generate a project at [Spring Initializr](https://start.spring.io).

1. Look at the following resources: `.mvn` and `mvnw.cmd`.

1. Find more information about Maven wrapper at [https://github.com/takari/maven-wrapper](https://github.com/takari/maven-wrapper).
