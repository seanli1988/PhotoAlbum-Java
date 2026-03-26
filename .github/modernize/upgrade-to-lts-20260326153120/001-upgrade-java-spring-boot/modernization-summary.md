# Modernization Summary: 001-upgrade-java-spring-boot

## Task
Upgrade to Java 21 and Spring Boot 3.4

## Changes Made

### `pom.xml`
- Upgraded `spring-boot-starter-parent` from `2.7.18` → `3.4.5`
- Updated `java.version` from `1.8` → `21`
- Updated `maven.compiler.source` and `maven.compiler.target` from `8` → `21`

All Spring ecosystem dependencies (Spring Data JPA, Spring MVC, Thymeleaf, Validation, Test) are managed by the Spring Boot 3.4.5 parent BOM and were updated automatically. Spring Boot 3.4.5 pulls in Spring Framework 6.2.x.

### `src/main/java/com/photoalbum/model/Photo.java`
- Migrated `javax.persistence.*` → `jakarta.persistence.*`
- Migrated `javax.validation.constraints.*` → `jakarta.validation.constraints.*`

### `src/main/java/com/photoalbum/service/impl/PhotoServiceImpl.java`
- `javax.imageio.ImageIO` was retained as-is — this is part of the JDK standard library, not a Jakarta EE API, and does not require migration.

## Verification

| Check | Result |
|-------|--------|
| Build | ✅ Passed |
| Unit Tests | ✅ Passed |
