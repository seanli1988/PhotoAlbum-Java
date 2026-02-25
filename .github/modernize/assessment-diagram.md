# Photo Album Application - Architecture Diagram

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end

    subgraph Presentation["Presentation Layer\n(Thymeleaf Templates)"]
        IndexPage["index.html\n(Photo Gallery)"]
        DetailPage["detail.html\n(Photo Detail)"]
        LayoutPage["layout.html\n(Shared Layout)"]
        StaticFiles["Static Assets\n(CSS / JS)"]
    end

    subgraph Controllers["Controller Layer\n(Spring MVC)"]
        HomeCtrl["HomeController\nGET / POST /upload"]
        DetailCtrl["DetailController\nGET /photos/id"]
        FileCtrl["PhotoFileController\nGET /photos/id/image"]
    end

    subgraph Services["Service Layer"]
        PhotoSvc["PhotoService\n(Interface)"]
        PhotoSvcImpl["PhotoServiceImpl\n(Business Logic)"]
    end

    subgraph DataAccess["Data Access Layer\n(Spring Data JPA)"]
        PhotoRepo["PhotoRepository\n(JpaRepository)"]
    end

    subgraph Model["Domain Model"]
        PhotoEntity["Photo Entity\n(id, fileName, photoData,\n mimeType, uploadedAt)"]
        UploadResult["UploadResult\n(success, photoId, error)"]
    end

    subgraph DataStorage["Data Storage"]
        OracleDB[("Oracle Database XE\nlocalhost:1521\nTable: photos")]
        FileSystem["Local File System\nstatic/uploads"]
    end

    Browser -- "HTTP Requests" --> HomeCtrl
    Browser -- "HTTP Requests" --> DetailCtrl
    Browser -- "HTTP Requests" --> FileCtrl

    HomeCtrl -- "renders" --> IndexPage
    DetailCtrl -- "renders" --> DetailPage
    HomeCtrl -- "uses" --> PhotoSvc
    DetailCtrl -- "uses" --> PhotoSvc
    FileCtrl -- "uses" --> PhotoSvc

    PhotoSvc --> PhotoSvcImpl
    PhotoSvcImpl -- "uses" --> PhotoRepo
    PhotoSvcImpl -- "creates" --> UploadResult
    PhotoRepo -- "maps to" --> PhotoEntity

    PhotoRepo -- "JDBC / Hibernate" --> OracleDB
    PhotoSvcImpl -- "stores files" --> FileSystem
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Language | Java 8 |
| Framework | Spring Boot 2.7.18 |
| Build Tool | Maven |
| Web / MVC | Spring MVC |
| Templating | Thymeleaf |
| Data Access | Spring Data JPA / Hibernate |
| Database | Oracle Database XE |
| File Storage | Local File System |
| Validation | Jakarta Bean Validation |
| Utilities | Apache Commons IO |
| Testing | JUnit 5, Spring Boot Test, H2 (in-memory) |
| Containerization | Docker / Docker Compose |

## Key Architecture Observations

- **Monolithic application**: Single deployable JAR containing all layers
- **Binary data stored in Oracle**: `photo_data` column (BLOB/LOB) holds raw image bytes
- **Dual storage approach**: Images stored both in Oracle DB and local file system (`static/uploads`)
- **Java 8 / Spring Boot 2.7.x**: Uses legacy `javax.*` packages (pre-Jakarta EE namespace)
- **No authentication/authorization**: All endpoints are publicly accessible
- **Single-node deployment**: No horizontal scaling support; file system path is not shared
