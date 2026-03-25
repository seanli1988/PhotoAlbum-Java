# Architecture Diagram

This diagram shows the high-level architecture of the Photo Album application, a Spring Boot web application that stores and serves photos using an Oracle database.

## Application Architecture

```mermaid
flowchart TD
    Browser["**Browser**\nUser Interface"]

    subgraph Docker["Docker Compose Environment"]
        subgraph AppContainer["Spring Boot Container - Eclipse Temurin JRE 8"]
            subgraph Presentation["Presentation Layer"]
                HC["HomeController\nGET / and POST /upload"]
                PFC["PhotoFileController\nGET /photo/{id}"]
                DC["DetailController\nGET and POST /detail/{id}"]
                TH["Thymeleaf Templates\nindex.html · detail.html · layout.html"]
                Static["Static Assets\nCSS · JavaScript"]
            end

            subgraph Business["Business Logic Layer"]
                PS["PhotoServiceImpl\nSpring Service"]
                IIO["Java ImageIO\nDimension Extraction"]
                VAL["Validation\nMIME Type · File Size up to 10MB"]
            end

            subgraph DataAccess["Data Access Layer"]
                PR["PhotoRepository\nSpring Data JPA"]
                HB["Hibernate ORM\nOracleDialect · ddl-auto create"]
                JDBC["Oracle JDBC Driver\nojdbc8"]
            end
        end

        subgraph DBContainer["Oracle XE 21.3 Container"]
            ODB["Oracle Database\nPHOTOS table"]
            BLOB["BLOB Storage\nBinary Image Data"]
        end
    end

    Browser -- "HTTP Requests port 8080" --> HC
    Browser -- "HTTP Requests port 8080" --> PFC
    Browser -- "HTTP Requests port 8080" --> DC

    HC -- "renders" --> TH
    DC -- "renders" --> TH
    TH -- "serves" --> Static

    HC -- "upload files" --> PS
    PFC -- "serve binary" --> PS
    DC -- "delete navigate" --> PS

    PS -- "extract dimensions" --> IIO
    PS -- "validate input" --> VAL
    PS -- "CRUD operations" --> PR

    PR -- "ORM mapping" --> HB
    HB -- "SQL queries" --> JDBC
    JDBC -- "TCP 1521" --> ODB
    ODB -- "stores" --> BLOB
```
