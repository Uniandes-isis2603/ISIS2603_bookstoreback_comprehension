## Estructura de Carpetas de la Aplicación Completa

La estructura de carpetas sigue el estándar de un proyecto Spring Boot, organizado en capas para facilitar la mantenibilidad y modularidad.

src/
├── main/
│   ├── java/
│   │   └── co/
│   │       └── edu/
│   │           └── uniandes/
│   │               └── dse/
│   │                   └── bookstore/
│   │                       ├── controllers/       # Controladores REST
│   │                       │   ├── BookController.java
│   │                       │   ├── AuthorController.java
│   │                       │   ├── PrizeController.java
│   │                       │   ├── EditorialController.java
│   │                       │   ├── ReviewController.java
│   │                       │   ├── OrganizationController.java
│   │                       │   ├── UserController.java
│   │                       │   ├── CategoryController.java
│   │                       │   ├── PublisherController.java
│   │                       │   ├── LibraryController.java
│   │                       │   └── EventController.java
│   │                       ├── dto/               # Data Transfer Objects
│   │                       │   ├── BookDTO.java
│   │                       │   ├── AuthorDTO.java
│   │                       │   ├── PrizeDTO.java
│   │                       │   ├── EditorialDTO.java
│   │                       │   ├── ReviewDTO.java
│   │                       │   ├── OrganizationDTO.java
│   │                       │   ├── UserDTO.java
│   │                       │   ├── CategoryDTO.java
│   │                       │   ├── PublisherDTO.java
│   │                       │   └── EventDTO.java
│   │                       ├── entities/          # Entidades JPA
│   │                       │   ├── BookEntity.java
│   │                       │   ├── AuthorEntity.java
│   │                       │   ├── PrizeEntity.java
│   │                       │   ├── EditorialEntity.java
│   │                       │   ├── ReviewEntity.java
│   │                       │   ├── OrganizationEntity.java
│   │                       │   └── EventEntity.java
│   │                       ├── exceptions/        # Excepciones personalizadas
│   │                       │   ├── EntityNotFoundException.java
│   │                       │   └── IllegalOperationException.java
│   │                       ├── repositories/      # Repositorios JPA
│   │                       │   ├── BookRepository.java
│   │                       │   ├── AuthorRepository.java
│   │                       │   ├── PrizeRepository.java
│   │                       │   ├── EditorialRepository.java
│   │                       │   ├── ReviewRepository.java
│   │                       │   ├── OrganizationRepository.java
│   │                       │   └── EventRepository.java
│   │                       ├── services/          # Lógica de negocio
│   │                       │   ├── BookService.java
│   │                       │   ├── AuthorService.java
│   │                       │   ├── PrizeService.java
│   │                       │   ├── EditorialService.java
│   │                       │   ├── ReviewService.java
│   │                       │   ├── OrganizationService.java
│   │                       │   └── EventService.java
│   │                       ├── utils/             # Utilidades
│   │                       │   └── ModelMapperConfig.java
│   │                       └── BookstoreApplication.java    # Clase principal que inicializa la aplicación
│   ├── resources/
│   │   ├── sql/             # Scripts SQL
│   │   │   ├── schema.sql           # Definición del esquema de la base de datos
│   │   │   └── data.sql             # Datos iniciales para la base de datos
│   │   ├── application.properties   # Configuración de Spring Boot
│   │   └── static/          # Archivos estáticos (si aplica)
│   └── webapp/              # Archivos web (si aplica)
├── test/                    # Pruebas unitarias
│   └── java/
│       └── co/
│           └── edu/
│               └── uniandes/
│                   └── dse/
│                       └── bookstore/
│                           ├── controllers/
│                           │   ├── BookControllerTest.java
│                           │   ├── AuthorControllerTest.java
│                           │   └── PrizeControllerTest.java
│                           ├── services/
│                           │   ├── BookServiceTest.java
│                           │   ├── AuthorServiceTest.java
│                           │   └── PrizeServiceTest.java
│                           └── repositories/
│                               ├── BookRepositoryTest.java
│                               ├── AuthorRepositoryTest.java
│                               └── PrizeRepositoryTest.java
└── pom.xml                  # Archivo de configuración de Maven


# Diagramas de Arquitectura de la Aplicación

## 1. Diagrama de Componentes
Este diagrama muestra los módulos principales de la aplicación y cómo se comunican entre sí.

```mermaid
C4Component
title Diagrama de Componentes

Container_Boundary(bookstoreApp, "Bookstore Application") {
    Component(Controllers, "Controllers", "Spring REST Controllers", "Manejan solicitudes HTTP.")
    Component(Services, "Services", "Business Logic Layer", "Contienen la lógica de negocio.")
    Component(Repositories, "Repositories", "Data Access Layer", "Gestionan operaciones CRUD.")
    Component(Entities, "Entities", "Data Model", "Representan las tablas de la base de datos.")
    Component(DTOs, "DTOs", "Data Transfer Objects", "Intercambian datos entre capas.")
    Component(Database, "Database", "PostgreSQL", "Almacena los datos de la aplicación.")
}

Rel(Controllers, Services, "Llama a")
Rel(Services, Repositories, "Usa")
Rel(Repositories, Entities, "Accede a")
Rel(Repositories, Database, "Lee/Escribe")
Rel(Controllers, DTOs, "Intercambia datos con")

## 2. Diagrama de Despliegue
graph TD
    A[Cliente] -->|HTTP Requests| B[Backend - Spring Boot]
    B -->|JDBC| C[Base de Datos - PostgreSQL]
    subgraph Backend
        B1[Controladores]
        B2[Servicios]
        B3[Repositorios]
    end
    B --> B1
    B1 --> B2
    B2 --> B3
    B3 --> C

## 3. Diagrama de Flujo de Datos
sequenceDiagram
    participant Cliente
    participant Controlador
    participant Servicio
    participant Repositorio
    participant BaseDeDatos

    Cliente->>Controlador: Solicitud HTTP (e.g., GET /books)
    Controlador->>Servicio: Llama al servicio correspondiente
    Servicio->>Repositorio: Solicita datos
    Repositorio->>BaseDeDatos: Ejecuta consulta SQL
    BaseDeDatos-->>Repositorio: Devuelve datos
    Repositorio-->>Servicio: Devuelve entidades
    Servicio-->>Controlador: Devuelve DTOs
    Controlador-->>Cliente: Respuesta HTTP con datos


## 4. Diagrama de Clases enriquecido (UML-like)

classDiagram
    class AuthorEntity {
        +Long id
        +Date birthDate
        +String description
        +String image
        +String name
        +List~PrizeEntity~ prizes
        +List~BookEntity~ books
    }

    class BookEntity {
        +Long id
        +String description
        +String image
        +String isbn
        +String name
        +Date publishingDate
        +EditorialEntity editorial
        +List~AuthorEntity~ authors
        +List~ReviewEntity~ reviews
    }

    class EditorialEntity {
        +Long id
        +String name
        +List~BookEntity~ books
    }

    class OrganizationEntity {
        +Long id
        +String name
        +Integer tipo
        +List~PrizeEntity~ prizes
    }

    class PrizeEntity {
        +Long id
        +String description
        +String name
        +Date premiationDate
        +AuthorEntity author
        +OrganizationEntity organization
    }

    class ReviewEntity {
        +Long id
        +String description
        +String name
        +String source
        +BookEntity book
    }

    %% Relationships
    AuthorEntity "1" --> "0..*" PrizeEntity : "awarded"
    AuthorEntity "1" --> "0..*" BookEntity : "writes"
    BookEntity "1" --> "0..*" ReviewEntity : "has"
    BookEntity "0..*" --> "1" EditorialEntity : "belongs to"
    BookEntity "0..*" --> "0..*" AuthorEntity : "written by"
    PrizeEntity "1" --> "1" AuthorEntity : "awarded to"
    PrizeEntity "1" --> "1" OrganizationEntity : "awarded by"

## 5. Diagrama de Paquetes
graph TD
    A[co.edu.uniandes.dse.bookstore.controllers] --> B[co.edu.uniandes.dse.bookstore.services]
    B --> C[co.edu.uniandes.dse.bookstore.repositories]
    C --> D[co.edu.uniandes.dse.bookstore.entities]
    A --> E[co.edu.uniandes.dse.bookstore.dto]
    C --> F[org.springframework.data.jpa.repository]

## 6. Mapa Funcional
graph TD
    A[src/main/java/co/edu/uniandes/dse/bookstore] --> B[controllers]
    A --> C[services]
    A --> D[repositories]
    A --> E[entities]
    A --> F[dto]
    A --> G[exceptions]
    A --> H[utils]
    A --> I[BookstoreApplication.java]


## 7. Diagramas Individuales por Clase
1. Diagrama de Clases - Book
classDiagram
    class BookController {
        +List~BookDetailDTO~ findAll()
        +BookDetailDTO findOne(Long id)
        +BookDTO create(BookDTO bookDTO)
        +BookDTO update(Long id, BookDTO bookDTO)
        +void delete(Long id)
    }

    class BookService {
        +List~BookEntity~ getBooks()
        +BookEntity getBook(Long id)
        +BookEntity createBook(BookEntity bookEntity)
        +BookEntity updateBook(Long id, BookEntity bookEntity)
        +void deleteBook(Long id)
    }

    class BookRepository {
        +Optional~BookEntity~ findById(Long id)
        +List~BookEntity~ findAll()
        +BookEntity save(BookEntity bookEntity)
        +void deleteById(Long id)
    }

    class BookEntity {
        +Long id
        +String name
        +String description
        +String isbn
        +Date publishingDate
        +EditorialEntity editorial
        +List~AuthorEntity~ authors
        +List~ReviewEntity~ reviews
    }

    class BookDTO {
        +Long id
        +String name
        +String description
        +String isbn
        +Date publishingDate
    }

    BookController --> BookService
    BookService --> BookRepository
    BookRepository --> BookEntity
    BookController --> BookDTO
    BookEntity --> EditorialEntity : "belongs to"
    BookEntity --> AuthorEntity : "written by"
    BookEntity --> ReviewEntity : "has"

2. Diagrama de Clases - Author
classDiagram
    class AuthorController {
        +List~AuthorDTO~ findAll()
        +AuthorDTO findOne(Long id)
        +AuthorDTO create(AuthorDTO authorDTO)
        +AuthorDTO update(Long id, AuthorDTO authorDTO)
        +void delete(Long id)
    }

    class AuthorService {
        +List~AuthorEntity~ getAuthors()
        +AuthorEntity getAuthor(Long id)
        +AuthorEntity createAuthor(AuthorEntity authorEntity)
        +AuthorEntity updateAuthor(Long id, AuthorEntity authorEntity)
        +void deleteAuthor(Long id)
    }

    class AuthorRepository {
        +Optional~AuthorEntity~ findById(Long id)
        +List~AuthorEntity~ findAll()
        +AuthorEntity save(AuthorEntity authorEntity)
        +void deleteById(Long id)
    }

    class AuthorEntity {
        +Long id
        +String name
        +Date birthDate
        +String description
        +String image
        +List~BookEntity~ books
        +List~PrizeEntity~ prizes
    }

    class AuthorDTO {
        +Long id
        +String name
        +Date birthDate
        +String description
        +String image
    }

    AuthorController --> AuthorService
    AuthorService --> AuthorRepository
    AuthorRepository --> AuthorEntity
    AuthorController --> AuthorDTO
    AuthorEntity --> BookEntity : "writes"
    AuthorEntity --> PrizeEntity : "awarded"


3. Diagrama de Clases - Editorial
classDiagram
    class EditorialController {
        +List~EditorialDTO~ findAll()
        +EditorialDTO findOne(Long id)
        +EditorialDTO create(EditorialDTO editorialDTO)
        +EditorialDTO update(Long id, EditorialDTO editorialDTO)
        +void delete(Long id)
    }

    class EditorialService {
        +List~EditorialEntity~ getEditorials()
        +EditorialEntity getEditorial(Long id)
        +EditorialEntity createEditorial(EditorialEntity editorialEntity)
        +EditorialEntity updateEditorial(Long id, EditorialEntity editorialEntity)
        +void deleteEditorial(Long id)
    }

    class EditorialRepository {
        +Optional~EditorialEntity~ findById(Long id)
        +List~EditorialEntity~ findAll()
        +EditorialEntity save(EditorialEntity editorialEntity)
        +void deleteById(Long id)
    }

    class EditorialEntity {
        +Long id
        +String name
        +List~BookEntity~ books
    }

    class EditorialDTO {
        +Long id
        +String name
    }

    EditorialController --> EditorialService
    EditorialService --> EditorialRepository
    EditorialRepository --> EditorialEntity
    EditorialController --> EditorialDTO
    EditorialEntity --> BookEntity : "publishes"

4. Diagrama de Clases - Review
classDiagram
    class ReviewController {
        +List~ReviewDTO~ findAll()
        +ReviewDTO findOne(Long id)
        +ReviewDTO create(ReviewDTO reviewDTO)
        +ReviewDTO update(Long id, ReviewDTO reviewDTO)
        +void delete(Long id)
    }

    class ReviewService {
        +List~ReviewEntity~ getReviews()
        +ReviewEntity getReview(Long id)
        +ReviewEntity createReview(ReviewEntity reviewEntity)
        +ReviewEntity updateReview(Long id, ReviewEntity reviewEntity)
        +void deleteReview(Long id)
    }

    class ReviewRepository {
        +Optional~ReviewEntity~ findById(Long id)
        +List~ReviewEntity~ findAll()
        +ReviewEntity save(ReviewEntity reviewEntity)
        +void deleteById(Long id)
    }

    class ReviewEntity {
        +Long id
        +String name
        +String description
        +String source
        +BookEntity book
    }

    class ReviewDTO {
        +Long id
        +String name
        +String description
        +String source
    }

    ReviewController --> ReviewService
    ReviewService --> ReviewRepository
    ReviewRepository --> ReviewEntity
    ReviewController --> ReviewDTO
    ReviewEntity --> BookEntity : "reviews"


5. Diagrama de Clases - Prize
classDiagram
    class PrizeController {
        +List~PrizeDTO~ findAll()
        +PrizeDTO findOne(Long id)
        +PrizeDTO create(PrizeDTO prizeDTO)
        +PrizeDTO update(Long id, PrizeDTO prizeDTO)
        +void delete(Long id)
    }

    class PrizeService {
        +List~PrizeEntity~ getPrizes()
        +PrizeEntity getPrize(Long id)
        +PrizeEntity createPrize(PrizeEntity prizeEntity)
        +PrizeEntity updatePrize(Long id, PrizeEntity prizeEntity)
        +void deletePrize(Long id)
    }

    class PrizeRepository {
        +Optional~PrizeEntity~ findById(Long id)
        +List~PrizeEntity~ findAll()
        +PrizeEntity save(PrizeEntity prizeEntity)
        +void deleteById(Long id)
    }

    class PrizeEntity {
        +Long id
        +String name
        +String description
        +Date premiationDate
        +AuthorEntity author
        +OrganizationEntity organization
    }

    class PrizeDTO {
        +Long id
        +String name
        +String description
        +Date premiationDate
    }

    PrizeController --> PrizeService
    PrizeService --> PrizeRepository
    PrizeRepository --> PrizeEntity
    PrizeController --> PrizeDTO
    PrizeEntity --> AuthorEntity : "awarded to"
    PrizeEntity --> OrganizationEntity : "awarded by"