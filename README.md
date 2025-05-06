# learnmaven

1. Install 
`sudo apt install maven`

2. Check installed 

```bash
bhavith@bhavith:~/Bhavith/Development/learnmaven$ mvn --version
Apache Maven 3.8.7
Maven home: /usr/share/maven
Java version: 11.0.27, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-arm64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-58-generic", arch: "aarch64", family: "unix"
```

3. Create `pom.xml` 
4. execute `mvn dependency:copy-dependencies`
5. Some reference: https://cs.android.com/android/platform/superproject/main/+/main:prebuilts/tools/common/m2/readme.txt
6. Output to specific dir `mvn dependency:copy-dependencies -DoutputDirectory=./repository/`
7. pom.xml without transit dependency 
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>dummy.project</groupId>
    <artifactId>grpc-no-transitive</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-okhttp</artifactId>
            <version>1.64.0</version>
            <exclusions>
                <exclusion> --------------> start 
                    <groupId>*</groupId>
                    <artifactId>*</artifactId>
                </exclusion> --------------> end 
            </exclusions>
        </dependency>
    </dependencies>
</project>
```
8. IMP COMMAND: `mvn dependency:copy-dependencies -DoutputDirectory=./repository/`


### without pom.xml 
1. execute `mvn dependency:get -DgroupId=io.grpc -DartifactId=grpc-okhttp -Dversion=1.64.0`
2. with the above command it goes to `~/.m2/` directry 


## Good practice 
1. Create a pom.xml 
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>dummy.project</groupId>
    <artifactId>grpc-no-transitive</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-okhttp</artifactId>
            <version>1.64.0</version>
            <exclusions>
                <exclusion>
                    <groupId>*</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
            </exclusions>
        </dependency>


        <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-netty-shaded</artifactId>
            <version>1.72.0</version>
            <exclusions>
                <exclusion>
                    <groupId>*</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

    </dependencies>
</project>
```
2. Execute 
   - for JAR `mvn dependency:copy-dependencies` 
   - for source `mvn dependency:sources -Dsilent=true`
   - for Java doc: `mvn dependency:resolve -Dclassifier=javadoc`
3. Copy required jar manually `cp -r ~/.m2/repository/io repository/`



### References
- https://stackoverflow.com/questions/11361331/how-to-download-sources-for-a-jar-with-maven
- https://cs.android.com/android/platform/superproject/main/+/main:prebuilts/tools/common/m2/readme.txt
- 