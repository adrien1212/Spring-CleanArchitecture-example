# Configuration en MicroService
La configuration en microservice necessite quelque adaptation dans la configuration de notre projet. Nous y utiliserons :
- Maven comme gestionnaire de dépendances
- Spring et ses librairies comme framework de développement

## Découpage du projet
Comme evoqué quand le chapitre précédent nous allons déployer 4 microservices. Cela signifie donc 4 projets Maven indépendant. Cependant afin de faciliter l'utilisation des librairies nous allons regrouper ces 4 projets sous un projet parent `prixbanque`

## POM parant
### Propriétés
```XML
<properties>
    <microservice.version>0.0.1-SNAPSHOT</microservice.version>
    <java.version>17</java.version>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <spring.boot.dependencies.version>2.7.4</spring.boot.dependencies.version>
</properties>
```

### Microservice
Le pom parent déclare également les 4 microservices que nous allons développer

```XML
<modules>
    <module>banktransfert</module>
    <module>bankaccount</module>
    <module>registration</module>
    <module>gateway</module>
</modules>
```

On doit également ajouter ces modules comme dépendance du project. Ainsi dans le block `<dependencyManagement></dependencyManagement>` nous rajoutons

```XML
<!-- MicroServices-->
<dependency>
    <groupId>uqac.groupe6</groupId>
    <artifactId>registration</artifactId>
    <version>${microservice.version}</version>
</dependency>
<dependency>
    <groupId>uqac.groupe6</groupId>
    <artifactId>connection</artifactId>
    <version>${microservice.version}</version>
</dependency>
<dependency>
    <groupId>uqac.groupe6</groupId>
    <artifactId>banktransfert</artifactId>
    <version>${microservice.version}</version>
</dependency>
<dependency>
    <groupId>uqac.groupe6</groupId>
    <artifactId>bankaccount</artifactId>
    <version>${microservice.version}</version>
</dependency>
```

### Les autres dépendances
```XML
<!-- Microservice -->
...

<!-- Spring -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>${spring.boot.dependencies.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>${spring.boot.dependencies.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>${spring.boot.dependencies.version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
    <version>${spring.boot.dependencies.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
    <version>3.1.4</version>
</dependency>


<!-- Base de données -->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.1</version>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.5.0</version>
    <scope>runtime</scope>
</dependency>

<!-- Utility -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
    <scope>provided</scope>
</dependency>

<!-- Tests -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-runner</artifactId>
    <version>1.9.1</version>
    <scope>test</scope>
</dependency>
```

### Le build
```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>${spring.boot.dependencies.version}</version>
        </plugin>
    </plugins>
</build>
```