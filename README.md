# VersioningDemo

A quick demo to display how to handle versioning with maven

In order to get this setup you clone this repository into your own github account. Then you follow the description [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) to set your ssh keys up for github.

When this is done you should be able to push new changes using ssh and that's required to push the new release.

### How to use Maven Release Plugin into your project

#### Add the necessary plugins

In the _build_ section of the pom.xml file add this code snippet:

```
<plugins>
    <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>3.0.0</version>
        <configuration>
            <archive>
                <manifest>
                    <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                    <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                    <mainClass>org.ea.VersioningDemo</mainClass>
                </manifest>
            </archive>
            <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
            </descriptorRefs>
        </configuration>
        <executions>
            <execution>
                <phase>package</phase>
                <goals>
                    <goal>single</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-release-plugin</artifactId>
        <version>2.5.3</version>
        <configuration>
            <goals>pre-integration-test</goals>
            <preparationGoals>package</preparationGoals>
            <allowTimestampedSnapshots>true</allowTimestampedSnapshots>
            <tagNameFormat>@{project.version}</tagNameFormat>
            <remoteTagging>false</remoteTagging>
        </configuration>
    </plugin>
</plugins>
```

#### Add the scm configuration for remote releases

```
<scm>
    <connection>scm:git:git@github.com:jamesattard/javaVersioningDemo.git</connection>
    <developerConnection>
      scm:git:git@github.com:jamesattard/javaVersioningDemo.git
    </developerConnection>
    <url>https://github.com/jamesattard/javaVersioningDemo</url>
    <tag>@{project.version}</tag>
</scm>
```

### Commands for building

To create a package of your build

```
mvn package
```

To prepare a build and create a new release tag on github

```
mvn -B release:prepare
```

If you omit the -B (--batch-mode) flag, then you will enter into interactive mode. This is usually ok, but if you intend to integrate with a build CI/CD system such as Jenkins or Teamcity, you probably need to go batch mode.

To perform the actual release and get the source, javadoc and jar-with-dependencies

```
mvn release:perform
```
