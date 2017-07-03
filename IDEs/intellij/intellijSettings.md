<!-- TOC START min:1 max:3 link:true update:true -->
- [Normal Setting after install](#normal-setting-after-install)
  - [Encoding](#encoding)
  - [git ignore](#git-ignore)
- [import existing projects](#import-existing-projects)
    - [set folder structure](#set-folder-structure)
- [issues](#issues)
  - [issue with JDK version while building](#issue-with-jdk-version-while-building)

<!-- TOC END -->

# Normal Setting after install

## Encoding
Setting -> File Encodings

## git ignore

Use below .gitignore


https://github.com/github/gitignore/blob/master/Global/JetBrains.gitignore

https://github.com/github/gitignore/blob/master/Java.gitignore


# import existing projects

### set folder structure


# issues

## issue with JDK version while building
 * exception
 ```exception
diamond operator is not supported in -source 1.5
```

 * resolution

Change the project pom.xml
https://stackoverflow.com/questions/29258141/maven-compilation-error-use-source-7-or-higher-to-enable-diamond-operator/31734791#31734791

Two options, both work the same.

 Option 1,
 ```xml
  <properties>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>
```
Option2,
 ```xml
 <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
