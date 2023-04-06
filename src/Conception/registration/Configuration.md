# Configuration

## Pom
*Spring Boot Starter Security* est un module pratique et efficace pour ajouter des fonctionnalités de sécurité à une application Spring Boot avec une configuration minimale.
```XML
<!-- Spring -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

## Application.properties

Dans le fichier `src/main/resources/application.properties`

   - On spécifie le port sur lequel l'application doit être déployée
   - On spécifie l'ensemble des informations pour accéder au services de gestion de base de données

```properties
# Server configuration
server.port=8083

#PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/customer
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL81Dialect
```