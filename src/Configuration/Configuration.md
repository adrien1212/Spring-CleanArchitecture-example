# Configuration en MicroService
Nous utiliserons Maven pour gérer nos services et les librairies externes utilisées.

## Découpage du projet
Comme evoqué quand le chapitre précédent nous allons déployer 3 microservices. Cela signifie donc 3 projets Maven indépendants. Cependant afin de faciliter l'utilisation des librairies nous allons regrouper ces 3 projets sous un projet parent `prixbanque`

## POM parent
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
Le pom parent déclare également les 3 microservices que nous allons développer ultérieurement.

```XML
<modules>
    <module>bankaccount</module>
    <module>registration</module>
    <module>gateway</module>
</modules>
```

On doit également ajouter ces modules comme dépendance du projet. Ainsi dans le bloc `<dependencyManagement></dependencyManagement>` nous rajoutons

```XML
<!-- MicroServices-->
<dependency>
    <groupId>adriencaubel.fr</groupId>
    <artifactId>registration</artifactId>
    <version>${microservice.version}</version>
</dependency>
<dependency>
    <groupId>adriencaubel.fr</groupId>
    <artifactId>connection</artifactId>
    <version>${microservice.version}</version>
</dependency>
<dependency>
    <groupId>adriencaubel.fr</groupId>
    <artifactId>banktransfert</artifactId>
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