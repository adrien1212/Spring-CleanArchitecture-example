# Configuration

## Pom

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>uqac.groupe6</groupId>
		<artifactId>prixbanque</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<artifactId>gateway</artifactId>

	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-gateway</artifactId>
		</dependency>
	</dependencies>
</project>
```

## Application.properties 

Dans le fichier `src/main/resources/application.properties`

```properties
spring.main.web-application-type=reactive
```

## Application.yaml
**Note**   
On aurait pu mettre la configuration suivante directement dans `application.properties`. 

Nous configurons les redirections à faire sur notre serveur *gateway*

```yml
server:
  port: 8080

spring:
  cloud:
    gateway:
      routes:
      - id: bankAccountModule
        uri: http://localhost:8081/
        predicates:
        - Path=/account/**
      - id: authentificationModule
        uri: http://localhost:8083/
        predicates:
        - Path=/customer/**
```