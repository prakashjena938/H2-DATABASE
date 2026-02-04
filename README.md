# H2 Database 

A complete guide to H2 Database covering architecture, execution modes, storage types, and real-world usage.

## Table of Contents

- [Introduction](#introduction)
- [Why H2 Database](#why-h2-database)
- [Understanding H2 Modes](#understanding-h2-modes)
- [Embedded Mode](#embedded-mode)
- [Server Mode](#server-mode)
- [Hybrid Mode](#hybrid-mode)
- [Architecture](#architecture)
- [Spring Boot Integration](#spring-boot-integration)
- [Production Usage](#production-usage)
- [Quick Reference](#quick-reference)

## Introduction

H2 is a lightweight, open-source SQL database written entirely in Java. It is commonly used for development, testing, and learning purposes. Unlike traditional databases, H2 can run inside an application or as a standalone server.

### Key Features

- Pure Java database engine
- No separate installation required
- Extremely fast for development
- Easy integration with Spring Boot
- Supports in-memory and file-based storage

## Why H2 Database

| Feature | Description |
|---------|-------------|
| Pure Java | Database engine written entirely in Java |
| No Installation | No separate database server setup required |
| Fast Performance | Optimized for development and testing |
| Easy Integration | Seamless integration with Spring Boot |
| Flexible Storage | Supports both in-memory and file-based storage |

## Understanding H2 Modes

H2 database modes are defined by two independent dimensions:

### Execution Modes (How H2 Runs)

- **Embedded Mode** - Runs inside your application's JVM
- **Server (TCP) Mode** - Runs as a standalone database server
- **Hybrid Mode (AUTO_SERVER)** - Combines embedded and server modes

### Storage Modes (Where Data Lives)

- **In-Memory Database** - Data stored in RAM (volatile)
- **File-Based Database** - Data persisted to disk (permanent)

## Embedded Mode

In embedded mode, H2 runs inside the same JVM as the application. No separate database server is required.

### Embedded + In-Memory Database

```
jdbc:h2:mem:devdb
```

**Characteristics:**
- Database exists only in RAM
- All data is lost when the application shuts down
- Ideal for unit testing and temporary data

### Embedded + File-Based Database

```
jdbc:h2:file:./data/devdb
```

**Characteristics:**
- Data is stored on disk
- Persistent across application restarts
- Ideal for local development and prototyping

## Server Mode

In server mode, H2 runs as a standalone process. Applications connect to it using TCP, similar to MySQL or PostgreSQL.

```
jdbc:h2:tcp://localhost/~/devdb
```

**Characteristics:**
- Multiple applications can connect simultaneously
- Database runs independently of applications
- Ideal for integration testing and team development

## Hybrid Mode

Hybrid mode combines embedded and server modes. The database starts embedded and automatically opens a TCP server if another JVM tries to connect.

```
jdbc:h2:file:./data/devdb;AUTO_SERVER=TRUE
```

**Characteristics:**
- Best of both worlds
- Automatic server promotion when needed
- Ideal for flexible development environments

## Architecture

```
+-------------------------+
| Java Application (JVM)  |
|   └── H2 Engine         |
+-------------------------+
            |
            v
      Database Storage
      +-----------------+
      |  In-Memory   ---|---> (Temporary)
      |  or             |
      |  File-Based  ---|---> (Persistent)
      +-----------------+
```

### Mode Comparison

| Mode | Location | Persistence | Multi-JVM | Use Case |
|------|----------|-------------|-----------|----------|
| Embedded + Memory | RAM | No | No | Unit Tests |
| Embedded + File | Disk | Yes | No | Local Development |
| Server (TCP) | Disk | Yes | Yes | Integration Tests |
| Hybrid | Disk | Yes | Yes | Flexible Development |

## Spring Boot Integration

Below is a typical Spring Boot configuration for H2:

### Application Properties

```properties
# Database Configuration
spring.datasource.url=jdbc:h2:file:./data/devdb
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driver-class-name=org.h2.Driver

# H2 Console Configuration
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Maven Dependency

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

### Gradle Dependency

```gradle
runtimeOnly 'com.h2database:h2'
```

### Accessing H2 Console

Once your Spring Boot application is running:

1. Navigate to: `http://localhost:8080/h2-console`
2. Use JDBC URL: `jdbc:h2:file:./data/devdb`
3. Username: `sa`
4. Password: (leave empty)

## Production Usage

H2 is **not recommended for production** systems. It is mainly used for:

- Development
- Testing
- Learning
- Prototyping

### Production Alternatives

For production systems, use enterprise-grade databases:

| Database | Use Case |
|----------|----------|
| PostgreSQL | General purpose, feature-rich |
| MySQL | Web applications, high read volumes |
| Oracle | Enterprise applications |
| SQL Server | Microsoft ecosystem |
| MongoDB | Document-oriented data |

## Quick Reference

### Common JDBC URLs

```
# In-Memory (volatile)
jdbc:h2:mem:testdb

# File-Based (persistent)
jdbc:h2:file:./data/mydb

# Server Mode (TCP)
jdbc:h2:tcp://localhost/~/mydb

# Hybrid Mode
jdbc:h2:file:./data/mydb;AUTO_SERVER=TRUE

# With Options
jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
```

### Useful Options

| Option | Description |
|--------|-------------|
| `DB_CLOSE_DELAY=-1` | Keep database open until JVM exits |
| `DB_CLOSE_ON_EXIT=FALSE` | Don't close database on exit |
| `AUTO_SERVER=TRUE` | Enable hybrid mode |
| `MODE=MySQL` | MySQL compatibility mode |
| `TRACE_LEVEL_FILE=4` | Enable detailed logging |

## Conclusion

H2 is a powerful yet lightweight database that helps developers build, test, and prototype applications quickly. Understanding its modes makes it easier to transition to production-grade databases later.

### Key Takeaways

- Use in-memory mode for unit tests
- Use file-based embedded mode for local development
- Use server mode for team collaboration
- Use hybrid mode for maximum flexibility
- Don't use H2 in production environments

## Additional Resources

- [Official H2 Documentation](http://www.h2database.com/html/main.html)
- [Spring Boot H2 Guide](https://spring.io/guides/gs/accessing-data-jpa/)
- [H2 GitHub Repository](https://github.com/h2database/h2database)

---

© 2026 – H2 Database Learning Guide
