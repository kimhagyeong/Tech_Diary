
### spring boot 사용하지 않고 일반 자바에서 dependencies 들을 패키징 하는 pom



```java
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>2.2.1</version>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <manifest>
                            <mainClass>org.ebay.baobab.migrator.Migrator</mainClass>
                        </manifest>
                    </archive>
                    <finalName>baobab-migrator-1.0</finalName>
                    <appendAssemblyId>false</appendAssemblyId>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
</build>
```
- filename을 append 하는 이유는   
jar이 두 개가 생기는데 fusion에 올려서 런을 할 때 해당 패키징 되어있는 jar를 읽기 위해서


### spring boot 로 작성한 코드를 mvn package하면 자동으로 vm 인스턴스에 생성한 jar를 업로드하는 pom

```java
<build>
       ...
        <extensions>
            <extension>
                <groupId>org.apache.maven.wagon</groupId>
                <artifactId>wagon-ssh</artifactId>
            </extension>
        </extensions>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>wagon-maven-plugin</artifactId>
                <version>2.0.0</version>
                <configuration>
                    <url>scp:<!-- instance ip -->
                    </url> 
                    <fromFile>target/porter-1.1.0-SNAPSHOT.jar</fromFile>
                    <toFile>porter-1.1.0-SNAPSHOT.jar</toFile>
                </configuration>
                <executions>
                    <execution>
                        <id>upload-jar</id>
                        <phase>package</phase>
                        <goals>
                            <goal>upload-single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

```

