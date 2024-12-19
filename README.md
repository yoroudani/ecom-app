# Projet Microservices pour la Gestion des Factures

## Description

Ce projet est une application basée sur une architecture microservices pour gérer des factures associées à des clients et des produits. L'application est composée de plusieurs microservices, chacun ayant une responsabilité spécifique, et utilise des outils modernes pour la communication, l'authentification et la découverte des services.

## Architecture des Microservices

1. **Billing-Service**:

   - Gère la création et la gestion des factures.
   - Communique avec Customer-Service et Inventory-Service pour enrichir les données des factures.

2. **Customer-Service**:

   - Gère les données des clients.
   - Fournit des APIs pour créer, lire, mettre à jour et supprimer des clients.

3. **Inventory-Service**:

   - Gère les données des produits.
   - Fournit des APIs pour gérer les produits (CRUD).

4. **Gateway-Service**:

   - Sert de point d'entrée unique pour tous les microservices.
   - Gère le routage des requêtes vers les microservices appropriés via Spring Cloud Gateway.

5. **Discovery-Service**:

   - Implémente le registre des services à l'aide d'Eureka pour permettre la découverte des microservices.

6. **Config-Service**:

   - Centralise la gestion des configurations des microservices.
   - Charge les configurations à partir d'un dépôt Git.

7. **Config-Git-Repo**:

   - Répertoire contenant les fichiers de configuration pour chaque microservice.

## Technologies Utilisées

- **Spring Boot** pour le développement des microservices.
- **Spring Cloud** (Eureka, Feign, Gateway, Config) pour la communication et la gestion des services.
- **JPA/Hibernate** pour la gestion des bases de données.
- **H2 Database** pour les données en mémoire pendant le développement.
- **Angular** pour le frontend.
- **Feign Client** pour la communication inter-microservices.

## Configuration des Microservices

### Config-Service

Le fichier `application.yml` de Config-Service contient :

```yaml
server:
  port: 9999
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/nom-utilisateur/config-git-repo
  application:
    name: config-service
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

### Discovery-Service

Le fichier `application.yml` de Discovery-Service contient :

```yaml
server:
  port: 8761
spring:
  application:
    name: discovery-service
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

### Billing-Service

Le fichier `application.yml` de Billing-Service contient :

```yaml
server:
  port: 8083
spring:
  application:
    name: billing-service
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

## Fonctionnalités

- Gestion des clients via Customer-Service.
- Gestion des produits via Inventory-Service.
- Création des factures avec des données enrichies (client et produits).
- Routage dynamique des requêtes via Gateway-Service.
- Découverte et enregistrement automatique des services via Discovery-Service.

## Lancer le Projet

### Étapes :

1. **Démarrer Config-Service**

   ```bash
   mvn spring-boot:run -pl config-service
   ```

2. **Démarrer Discovery-Service**

   ```bash
   mvn spring-boot:run -pl discovery-service
   ```

3. **Démarrer les autres services** (Customer-Service, Inventory-Service, Billing-Service, Gateway-Service).

   ```bash
   mvn spring-boot:run -pl <service-name>
   ```

4. **Lancer le frontend Angular** :

   ```bash
   ng serve
   ```

### Accès aux Services :

- **Eureka Dashboard** : [http://localhost:8761](http://localhost:8761)
- **Config-Service** : [http://localhost:9999](http://localhost:9999)
- **Gateway-Service** : [http://localhost:8888](http://localhost:8888)
- **Angular Frontend** : [http://localhost:4200](http://localhost:4200)

## Exemple d'API

### Récupérer une facture par ID

**URL** : `GET /bills/{id}`

### Récupérer tous les clients

**URL** : `GET /CUSTOMER-SERVICE/customers`

### Récupérer tous les produits

**URL** : `GET /INVENTORY-SERVICE/products`
