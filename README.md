# country-service 1

Micro service REST de gestion de pays, développé avec **Spring Boot**.
Projet d'atelier illustrant la création d'une application autonome packagée en `.jar`.

## Fonctionnalités

API REST exposant les opérations CRUD sur l'entité `Country` (id, nom, capitale).

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| GET | `/getcountries` | Liste tous les pays |
| GET | `/getcountries/{id}` | Récupère un pays par son id |
| GET | `/getcountries/countryname?name=` | Récupère un pays par son nom |
| POST | `/addcountry` | Ajoute un pays |
| PUT | `/updatecountry/{id}` | Met à jour un pays |
| DELETE | `/deletecountry/{id}` | Supprime un pays |

## Stack technique

- Java 25 / Spring Boot 3.5.4
- Spring Data JPA + Hibernate
- Base H2 en mémoire (console activée) ; driver MySQL inclus
- Lombok, Springdoc OpenAPI (Swagger UI)
- Maven (wrapper `mvnw`)

## Architecture

```
com.countryservice.demo
├── CountryServiceApplication     # Point d'entrée
├── beans/Country                 # Entité JPA
├── repositories/CountryRepository
├── services/CountryService       # Logique métier
└── controllers/CountryController # API REST
```

## Démarrage

```bash
./mvnw spring-boot:run
```

Application disponible sur `http://localhost:8082`.
Swagger UI : `http://localhost:8082/swagger-ui.html`
Console H2 : `http://localhost:8082/h2-console`

## Tests

```bash
./mvnw test
```

- Tests unitaires du service (Mockito)
- Tests du contrôleur (Mockito)
- Tests des endpoints (MockMvc)

## Configuration

Voir `src/main/resources/application.properties` (datasource, port, dialecte Hibernate).
