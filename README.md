# Maven demo

## Demo 1 - manual

- download Apache Maven https://mirrors.ocf.berkeley.edu/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip
- unzip and copy to `C:\Program Files\apache-maven`
- prepare the environment
```
set JAVA_HOME=C:\Program Files\Java\jdk-11
set PATH=C:\Program Files\apache-maven\bin;%PATH%
```
- verify that Apache Maven is installed correctly
```
mvn --version
```
- create a project folder
```
md maven-demo-1
cd maven-demo-1
```
- create a `pom.xml` file
```
fsutil file createnew pom.xml 0
```
- add empty project
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
</project>
```
- validate `pom.xml`
```
mvn validate
```
- add `groupId`, `artifactId` and `version`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.wellsfargo</groupId>
    <artifactId>maven-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>
```
- validate our `pom.xml` file
```
mvn validate
```
- let's create a tiny Java application
```
md src\main\java\com\wellsfargo
md src\test\java\com\wellsfargo
fsutil file createnew src\main\java\com\wellsfargo\App.java 0
fsutil file createnew src\test\java\com\wellsfargo\AppTest.java 0
```
- let's create a test first
```java
package com.wellsfargo;

import static org.junit.Assert.assertTrue;

import org.junit.Test;

public class AppTest {

    @Test
    public void shouldPass() {
        assertTrue(true);
    }

}

```
- and the actual app:
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
- try to compile
```
mvn compile
```
- add dependency
```xml
    ...
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.9</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    ...
```
- run tests and pass
```
mvn test
```
- create a `.jar` file using Maven
```
mvn package
```
- try to run the app
```
"%JAVA_HOME%"\bin\java -jar target\maven-demo-1.0-SNAPSHOT.jar
```
- add another plugin
```xml
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
    ...
```
- run the app
```
"%JAVA_HOME%"\bin\java -jar target\maven-demo-1-1.0-SNAPSHOT.jar
```

## Demo 2 - Automated

- create a new project
```
mvn archetype:generate -DgroupId=com.wellsfargo -DartifactId=maven-demo-2 -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
cd mvn-demo-2
```
- change `App.java`
```java
    ...
    System.out.println();
    System.out.println("+=====================+");
    System.out.println("| Hello Maven Demo 2! |");
    System.out.println("+=====================+");
    ...
```
- compile and test
```
mvn compile
mvn test
```
- specify artifact in `pom.xml`
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
- create jar
```
mvn package
```
- run app
```
"%JAVA_HOME%"\bin\java -jar target\maven-demo-2-1.0-SNAPSHOT.jar
```
- generate site
```
mvn site
cd target\site
```
- serve
```
python -m http.server 8081
```

## Demo 3 - Spring

- let's create a simple Spring Boot web application

https://github.com/takari/maven-wrapper

