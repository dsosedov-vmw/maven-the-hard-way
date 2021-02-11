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
- add `groupId`, `artifactId` and `version`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.wfb</groupId>
    <artifactId>maven-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</project>
```
- validate our `pom.xml` file
```
mvn validate
```
- let's create a tiny Java application
```
md src\main\java\com\wfb\mavendemo
fsutil file createnew src\main\java\com\wfb\mavendemo\App.java 0
```
with the following content:
```java
package com.wfb.mavendemo;

public class App {

    public static void main(String[] args) {
        System.out.print("Hello Maven!");
    }

}
```
- create a `.jar` file using Maven
```
mvn package
```
- run the app
```
"%JAVA_HOME%"\bin\java -jar target\maven-demo-0.0.1-SNAPSHOT.jar
```

## Demo 2 - Spring


