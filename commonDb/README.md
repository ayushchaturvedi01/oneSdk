# commonDatabase

**A Vision for Unified Database Access**

`commonDatabase` is a planned SDK within the [oneSdk](#onesdk) ecosystem, designed to simplify database interactions by providing a unified interface for popular Object-Relational Mappers (ORMs) and database drivers. Our research shows developers need a single, consistent API to work with ORMs like **Prisma**, **MikroORM**, **Drizzle**, **Mongoose**, **Cassandra**, and others. `commonDatabase` aims to deliver this with enhanced features tailored to modern development needs.

> **Note**: `commonDatabase` is in the research and planning phase. This README outlines our vision and planned features based on extensive research.

<div align="center">
  <img src="https://img.shields.io/badge/Status-Research-blue" alt="Status: Research">
  <img src="https://img.shields.io/badge/License-MIT-green" alt="License: MIT">
</div>

## 🔍 ORM Architecture Research

To build a robust unified database interface, we’ve conducted in-depth research into the architectures of leading ORMs, focusing on **Prisma** and **MikroORM**. These ORMs are widely adopted in the JavaScript/TypeScript ecosystem, offering distinct approaches to database interaction that inform the design of `commonDatabase`.

**View the [Interactive Architecture Flowchart](https://onesdkresearch.netlify.app/)** in your browser for a comprehensive visual explanation.

### Prisma Architecture Research

Prisma is a modern ORM known for its type-safe client, schema-driven approach, and flexible engine architecture.

#### 📊 Architecture Visualization
Our research includes interactive flowcharts visualizing Prisma’s layered architecture:
- **System Overview**: Schema definition to database interaction.
- **Schema Parsing**: Transformation of schema files into internal representations.
- **Client Generation**: Type-safe client generation from schemas.
- **Query Execution**: Flow from client API to database operations.
- **Engine Layer**: Abstraction for multiple database connectivity strategies.
- **Database Connectors**: Support for various database types.


#### 🧩 Key Insights
1. **Multi-Layered Abstraction**: Prisma separates concerns across Schema, Client, Query, Engine, and Database layers.
2. **Engine Interface**: A unified contract allows different engine implementations (Client, Library, Data Proxy, Accelerate).
3. **Protocol-Driven Communication**: Standardized protocols ensure flexibility.
4. **Database-Agnostic Queries**: Queries are represented in a JSON-like format before database translation.
5. **Type Generation**: Schema-to-types pipeline ensures end-to-end type safety.

#### 🔄 Engine Implementations
Prisma’s engine abstraction is a key inspiration for `commonDatabase`:

| Engine Type       | Purpose                          | Best For                        |
|-------------------|----------------------------------|---------------------------------|
| ClientEngine      | Direct connectivity              | Local development, performance |
| LibraryEngine     | In-process execution             | Traditional server deployments |
| DataProxyEngine   | Remote execution                | Serverless, edge computing     |
| AccelerateEngine  | Cloud-optimized execution        | Scalable performance           |

### MikroORM Architecture Research

MikroORM is a TypeScript-first ORM known for its entity-based approach, support for both SQL and NoSQL databases, and flexible configuration options.

#### 📊 Architecture Visualization
Our research includes flowcharts visualizing MikroORM’s architecture:
- **System Overview**: Entity definitions to database operations.
- **Entity Management**: How entities are mapped to database schemas.
- **Unit of Work**: Transaction management and change tracking.
- **Query Building**: Construction of database-agnostic queries.
- **Driver Layer**: Abstraction for database-specific operations.
- **Connection Management**: Handling database connections and pooling.


#### 🧩 Key Insights
1. **Entity-Centric Design**: MikroORM uses entities as the primary abstraction, with decorators or schema definitions for mapping.
2. **Unit of Work Pattern**: Tracks changes to entities within a transaction, optimizing database operations.
3. **Driver Abstraction**: A unified driver interface supports multiple databases (SQL, MongoDB) with minimal configuration changes.
4. **Query Builder**: Provides a fluent, database-agnostic API for complex queries.
5. **TypeScript Integration**: Native TypeScript support with decorators and metadata for type safety.
6. **Flexible Configuration**: Supports both schema-first and code-first approaches.

#### 🔄 Driver Implementations
MikroORM’s driver abstraction is highly relevant to `commonDatabase`:

| Driver Type       | Supported Databases              | Best For                        |
|-------------------|----------------------------------|---------------------------------|
| PostgreSQLDriver  | PostgreSQL                      | Relational database use cases  |
| MySQLDriver       | MySQL, MariaDB                  | Traditional SQL applications   |
| SQLiteDriver      | SQLite                          | Lightweight, embedded databases |
| MongoDriver       | MongoDB                         | NoSQL, document-based apps     |

MikroORM’s ability to unify SQL and NoSQL databases through a single driver interface aligns with our goal of a database-agnostic API.

### How Prisma and MikroORM Inform commonDatabase
- **Unified Interface**: Both ORMs demonstrate the power of abstracting database-specific details behind a consistent API, which `commonDatabase` will emulate.
- **Type Safety**: Prisma’s schema-driven types and MikroORM’s decorator-based types highlight the importance of TypeScript integration.
- **Extensibility**: Prisma’s engine and MikroORM’s driver abstractions inspire our plugin system and ORM adapter architecture.
- **Query Abstraction**: Both ORMs use intermediate query formats, informing our plan for a database-agnostic query interface.

## 💡 Why commonDatabase?

Integrating multiple ORMs or databases often means dealing with inconsistent APIs, complex configurations, and performance challenges. Our research, including insights from Prisma and MikroORM, highlights that developers want a solution that abstracts these complexities while adding value. `commonDatabase` is designed to:
- Provide a **unified API** for all supported ORMs.
- Enhance functionality with features like caching and query optimization.
- Support a wide range of databases, from SQL to NoSQL.

## 📋 Planned Support

Based on our research, `commonDatabase` will support the following ORMs and databases:

| ORM/Driver   | Planned Databases                     | Status         |
|--------------|---------------------------------------|----------------|
| Prisma       | PostgreSQL, MySQL, SQLite, SQL Server | 🗂️ Planned    |
| MikroORM     | PostgreSQL, MySQL, MariaDB, SQLite, MongoDB | 🗂️ Planned |
| Drizzle      | PostgreSQL, MySQL, SQLite             | 🗂️ Planned    |
| Mongoose     | MongoDB                               | 🗂️ Planned    |
| Cassandra    | Apache Cassandra, ScyllaDB            | 🗂️ Planned    |
| TypeORM      | PostgreSQL, MySQL, MariaDB, SQLite    | 🗂️ Planned    |
| Sequelize    | PostgreSQL, MySQL, SQLite, MSSQL      | 🗂️ Planned    |

*Additional ORMs and databases will be added based on community feedback.*

## ✨ Planned Features

- **Unified Query Interface**: Write queries once, run them with any supported ORM.
- **Connection Management**: Automatic pooling, reconnections, and timeout handling.
- **Query Caching**: In-memory and Redis-based caching for faster queries.
- **Transaction Support**: Simplified, cross-ORM transaction handling inspired by MikroORM’s Unit of Work.
- **Query Optimization**: Automatic query analysis and performance improvements.
- **Plugin System**: Extend functionality with custom plugins (e.g., logging, auditing).
- **Migration Integration**: Seamless integration with ORM-native migration tools.

## 🖥️ Example Vision

Here's how we envision `commonDatabase` usage:

```javascript
import { commonDatabase } from '@onesdk/commondatabase';

// Configure with Prisma
const db = commonDatabase.configure({
  orm: 'prisma',
  connection: 'postgresql://user:pass@localhost:5432/dbname',
});

// Run a unified query
const users = await db.query('SELECT * FROM users WHERE active = ?', [true]);

// Use planned caching feature
const cachedUsers = await db.cachedQuery(
  'SELECT * FROM users WHERE active = ?',
  [true],
  { ttl: 300 } // Cache for 5 minutes
);
```

## 🔌 Architecture Inspiration

Our design for `commonDatabase` draws inspiration from our Prisma architecture research. Below is our planned abstraction approach:

```mermaid
graph TD
    A[Developer API] --> B[Unified Query Interface]
    B --> C{ORM Adapter}
    C -->|Prisma| D[Prisma Engine]
    C -->|Drizzle| E[Drizzle Core]
    C -->|Mongoose| F[Mongoose Driver]
    C -->|Custom| G[Other Adapters]
    D --> H[Database]
    E --> H
    F --> H
    G --> H
    
    I[Feature Enhancement Layer] --> B
    I -->|Caching| J[Cache Providers]
    I -->|Monitoring| K[Telemetry]
    I -->|Optimization| L[Query Optimizer]
```

## 🛣️ Roadmap

- **Phase 1**: Research and Architecture Design (Current)
- **Phase 2**: Core Interface Development and Prisma Adapter
- **Phase 3**: Drizzle and Mongoose Adapters
- **Phase 4**: Feature Enhancements (Caching, Optimization)
- **Phase 5**: Additional ORM Support and Enterprise Features

## 🤝 Contributing

While we're in the research phase, we welcome discussions and ideas! Feel free to open an issue to share your thoughts on what you'd like to see in a unified database interface.

<!-- ## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details. -->

## 🌐 oneSdk Ecosystem

`commonDatabase` is part of the oneSdk ecosystem, which aims to provide unified interfaces for common development tasks across frameworks and platforms.

---

<div align="center">
  <sub>Built with ❤️ as part of the Wasserstoff</sub>
</div>