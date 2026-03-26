# Upgrade Plan

## Overview

Upgrade the Java project to the latest LTS versions: **Java 21** and **Spring Boot 3.4**.

## Tasks

See `tasks.json` for detailed task breakdown.

### 001 - Upgrade Java 21 and Spring Boot 3.4

- Upgrade JDK to **Java 21**
- Upgrade Spring Boot to **3.4** and Spring Framework to **6.x**
- Migrate `javax.*` imports to `jakarta.*` namespaces where required
- Update compatible Spring ecosystem dependencies (Spring Security, Spring Data, Spring Cloud, etc.)
- Update Maven/Gradle build plugin versions to support Java 21
