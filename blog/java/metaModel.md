# Meta model

Có khi nào trong source code gặp trong code thấy có những Constant được tự sinh ra nhưng với domain dạng _ ví dụ Role_.PERMISSION

Thông tin constant này sẽ được tự động sinh ra dựa trên domain được khai báo.

## Vậy mục đích để làm gì?

- Ta không muốn hard code hay sử dụng tên cột trong code

- Trong trường hợp sử dụng cho specifications và thay đổi tên của các cột trong table kết hợp với @JsonProperty


## Cài đặt

```java
pom.xml

    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-jpamodelgen</artifactId>
        <version>6.0.2.Final</version>
        <scope>provided</scope>
    </dependency>

<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>

                    // config lombok để có getter, setter

                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>${maven-compiler-plugin.version}</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <annotationProcessorPaths>
                        <!-- For JPA static metamodel generation -->
                        <path>

                        // connect với database thông qua hibernate

                            <groupId>org.hibernate</groupId>
                            <artifactId>hibernate-jpamodelgen</artifactId>
                            <version>${hibernate.version}</version>
                        </path>
<!--                        <path>-->
<!--                            <groupId>org.glassfish.jaxb</groupId>-->
<!--                            <artifactId>jaxb-runtime</artifactId>-->
<!--                            <version>2.3.3</version>-->
<!--                        </path>-->
                        <path>
                        
                        // lombok

                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                            <version>1.18.20</version>
                        </path>
                    </annotationProcessorPaths>
                </configuration>
            </plugin>

        </plugins>
    </build>

```

Run to build

    mvn clean install

build or run application 1 times, and the model file will generate automatically

Now, we can call it with Role_.ID,  Role_.NAME,  Role_.APPROVE_DATE,  Role_.TERMINATE_DATE all columns name

![img.png](blog/java/img/metaModel.png)