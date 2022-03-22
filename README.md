# The idea

Maven master project for **pom.xml** samples including best practices and tips and tricks

# Introduction

This repo contains a *super pom* as a master example project with multiple sub modules, each of these modules have a particular behaviour based on a need.

## Best practices

- The < dependencyManagement > tag allows to centralize dependencies in all sub modules, version tag in dependencies modules become useless
- You should hard code the plugin version to get an immutable compilation
- Use the < properties > tag for version declaration and sort it by type

# Modules

## quickstart
The standard maven project by definition

```mvn archetype:generate``` 

## wrapper
Wrapper generation for automatic and standalone maven installation

```mvn wrapper:wrapper``` 

## xsd2jar
This pom can generate pojo classes in a compiled jar from a given xsd, place the xsd in *src/main/resources*

```mvn package``` 

## shade
This pom can create a shaded jar that include all the dependencies from < dependencies > tag. 
Very useful for a Talend project that use a *tLibrary* load component.

```mvn package``` 

# Tip and tricks

## Configuration scope

[Official documentation](https://maven.apache.org/ref/3.3.9/maven-settings/settings.html)

- Global scope -> **$M2_HOME/conf/settings.xml**
- User scope -> **${user.home}/.m2/settings.xml**
- Project scope -> **pom.xml**

## Password crypting

[Official documentation](https://maven.apache.org/guides/mini/guide-encryption.html)

Master

```mvn --encrypt-master-password <password>```

Server

```mvn --encrypt-password <password>```


## Manual local repository jar installation

1 - Download mysql driver

```
curl https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.49/mysql-connector-java-5.1.49.jar -o mysql.jar
```

2 - Push the driver to local repository

```
mvn -e install:install-file \
-Dfile=mysql.jar \
-DgroupId=mysql \
-DartifactId=mysql-connector-java \
-Dversion=5.1.49 \
-Dpackaging=jar \
-DlocalRepositoryPath=${user.home}/.m2/repository
```

## Dump full/complete pom

```
mvn help:effective-pom -Doutput=full_pom.xml
```

## See dependency tree

```
mvn dependency:tree -Ddetail=true
```

## Skip unit tests

Inside the pom
```
<properties>
  <maven.test.skip>true</maven.test.skip>
</properties>
```
With command parameter
```
$ mvn test -DskipTests=true
```
With test plugin parameter
```
<plugin>
 <artifactId>maven-surefire-plugin</artifactId>
 <configuration>
  <skip>true</skip>
 </configuration>
</plugin>
```

## Download full dependencies for offline use

[Official documentation](https://access.redhat.com/documentation/en-us/red_hat_jboss_fuse/6.1/html/deploying_into_the_container/locate-customrepo)

```
$ mvn org.apache.maven.plugins:maven-dependency-plugin:2.8:go-offline -Dmaven.repo.local=/tmp/here
```
