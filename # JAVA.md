# JAVA
웹 프레임워크: Spring
프로젝트/패키지 관리 : Maven

```bash
sudo apt install openjdk-11-jdk maven
```

```bash
mkdir -p src/main/java/hello
```

`src/main/java/hello/HelloWorld.java`
```java
package hello;

public class HelloWorld {
	public static void main(String[] args) {
		Greeter greeter = new Greeter();
		System.out.println(greeter.sayHello());
	}
}
```

`src/main/java/hello/Greeter.java`
```java
package hello;

public class Greeter {
	public String sayHello() {
		return "Hello World";
	}
}
```

`pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.4</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>hello.HelloWorld</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

패키징
```bash
mvn package
```
-> 타켓 디렉토리 생성

자바 아카이브 데이타 (jar) 실행
```bash
java -jar target/gs-maven-0.1.0.jar
```
-> Hello World 출력

---

### Java Web App

```bash
curl 'https://start.spring.io/starter.zip?type=maven-project&language=java&bootVersion=2.7.1&baseDir=restservice&groupId=com.example&artifactId=restservice&name=restservice&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.restservice&packaging=war&javaVersion=11&dependencies=web' --compressed -o web.zip

```bash
sudo apt install unzip
```

```bash
unzip web.zip
```

```bash
cd restservice
```

```bash
vi src/main/java/com/example/restservice/Greeting.java
```

```java
package com.example.restservice;

public class Greeting {

	private final long id;
	private final String content;

	public Greeting(long id, String content) {
		this.id = id;
		this.content = content;
	}

	public long getId() {
		return id;
	}

	public String getContent() {
		return content;
	}

```

```bash
vi src/main/java/com/example/restservice/GreetingController.java
```

```java
package com.example.restservice;

import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

	private static final String template = "Hello, %s!";
	private final AtomicLong counter = new AtomicLong();

	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
}
```

패키징
```bash
mvn package
```

```bash
./mvnw spring-boot:run
```

```bash
curl localhost:8080/greetings
```
