# Master Development Guide for Phase 4: Database Design and Integration

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objectives](#2-objectives)
3. [Technology Stack](#3-technology-stack)
4. [Database Architecture Overview](#4-database-architecture-overview)
   - [Data Models](#data-models)
   - [Database Relationships](#database-relationships)
5. [Setting Up the Development Environment](#5-setting-up-the-development-environment)
   - [Installing PostgreSQL](#installing-postgresql)
   - [Configuring the Database](#configuring-the-database)
6. [Sequelize ORM Setup](#6-sequelize-orm-setup)
   - [Installing Dependencies](#installing-dependencies)
   - [Initializing Sequelize](#initializing-sequelize)
   - [Configuring Sequelize](#configuring-sequelize)
7. [Defining Data Models](#7-defining-data-models)
   - [User Model](#user-model)
   - [Analysis Model](#analysis-model)
   - [Note Model](#note-model)
8. [Establishing Model Associations](#8-establishing-model-associations)
9. [Database Migrations](#9-database-migrations)
   - [Creating Migrations](#creating-migrations)
   - [Running Migrations](#running-migrations)
10. [Database Seeders (Optional)](#10-database-seeders-optional)
11. [Database Operations and Testing](#11-database-operations-and-testing)
    - [CRUD Operations](#crud-operations)
    - [Writing Unit Tests](#writing-unit-tests)
12. [Environment Configuration](#12-environment-configuration)
13. [Security Considerations](#13-security-considerations)
    - [Handling Sensitive Data](#handling-sensitive-data)
    - [Preventing SQL Injection](#preventing-sql-injection)
14. [Performance Optimization](#14-performance-optimization)
    - [Indexing](#indexing)
    - [Query Optimization](#query-optimization)
15. [Backup and Recovery Strategies](#15-backup-and-recovery-strategies)
16. [Deployment Preparation](#16-deployment-preparation)
17. [Conclusion](#17-conclusion)
18. [Appendix](#18-appendix)
    - [Additional Resources](#additional-resources)

---

## 1. Introduction

This Master Development Guide for Phase 4 focuses on **Database Design and Integration** for the **Terms & Conditions (T&C) Decoder Tool**. The guide provides detailed instructions on setting up the PostgreSQL database, defining data models using Sequelize ORM, establishing relationships, performing migrations, and ensuring the database is secure, efficient, and ready for integration with the backend application.

---

## 2. Objectives

- Set up a PostgreSQL database for development and production environments.
- Define data models (`User`, `Analysis`, `Note`) using Sequelize.
- Establish associations between models.
- Create and run database migrations.
- Ensure data integrity and security.
- Optimize database performance.
- Prepare the database for seamless integration with the backend API.

---

## 3. Technology Stack

- **Database**: PostgreSQL
- **ORM**: Sequelize
- **Backend Language**: JavaScript (Node.js)
- **Backend Framework**: Express.js
- **Migration Tool**: Sequelize CLI

---

## 4. Database Architecture Overview

### Data Models

1. **User**

   - **Fields**:
     - `id`: UUID (Primary Key)
     - `username`: STRING, unique, not null
     - `email`: STRING, unique, not null
     - `password`: STRING, not null
     - `createdAt`: DATE
     - `updatedAt`: DATE

2. **Analysis**

   - **Fields**:
     - `id`: UUID (Primary Key)
     - `userId`: UUID (Foreign Key to User)
     - `documentName`: STRING, not null
     - `analysisData`: JSONB, not null
     - `createdAt`: DATE
     - `updatedAt`: DATE

3. **Note**

   - **Fields**:
     - `id`: UUID (Primary Key)
     - `userId`: UUID (Foreign Key to User)
     - `analysisId`: UUID (Foreign Key to Analysis)
     - `content`: TEXT, not null
     - `createdAt`: DATE
     - `updatedAt`: DATE

### Database Relationships

- **User** has many **Analyses**
- **Analysis** belongs to **User**
- **Analysis** has many **Notes**
- **Note** belongs to **Analysis**
- **User** has many **Notes**
- **Note** belongs to **User**

---

## 5. Setting Up the Development Environment

### Installing PostgreSQL

**For Windows and macOS:**

- Download and install PostgreSQL from the official website: [https://www.postgresql.org/download/](https://www.postgresql.org/download/)

**For Ubuntu/Linux:**

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### Configuring the Database

**Create a new PostgreSQL user (optional):**

```bash
sudo -u postgres createuser --interactive
```

**Create a development database:**

```bash
sudo -u postgres createdb tcdecoder_dev
```

---

## 6. Sequelize ORM Setup

### Installing Dependencies

Navigate to your backend project directory and install Sequelize and PostgreSQL dependencies:

```bash
npm install sequelize pg pg-hstore
npm install --save-dev sequelize-cli
```

### Initializing Sequelize

Initialize Sequelize in your project:

```bash
npx sequelize-cli init
```

This command creates the following directories:

- `models/`
- `migrations/`
- `seeders/`
- `config/`

### Configuring Sequelize

Edit the `config/config.json` file to include development, test, and production configurations:

```json
{
  "development": {
    "username": "your_db_username",
    "password": "your_db_password",
    "database": "tcdecoder_dev",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "test": {
    "username": "your_db_username",
    "password": "your_db_password",
    "database": "tcdecoder_test",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "production": {
    "username": "your_db_username",
    "password": "your_db_password",
    "database": "tcdecoder_prod",
    "host": "your_production_db_host",
    "dialect": "postgres",
    "ssl": true
  }
}
```

**Note**: Use environment variables to store sensitive information like username and password.

---

## 7. Defining Data Models

### User Model

Create `models/user.js`:

```javascript
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class User extends Model {
    static associate(models) {
      User.hasMany(models.Analysis, { foreignKey: 'userId' });
      User.hasMany(models.Note, { foreignKey: 'userId' });
    }
  }
  User.init(
    {
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true,
      },
      username: { type: DataTypes.STRING, unique: true, allowNull: false },
      email: { type: DataTypes.STRING, unique: true, allowNull: false },
      password: { type: DataTypes.STRING, allowNull: false },
    },
    {
      sequelize,
      modelName: 'User',
    }
  );
  return User;
};
```

### Analysis Model

Create `models/analysis.js`:

```javascript
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Analysis extends Model {
    static associate(models) {
      Analysis.belongsTo(models.User, { foreignKey: 'userId' });
      Analysis.hasMany(models.Note, { foreignKey: 'analysisId' });
    }
  }
  Analysis.init(
    {
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true,
      },
      userId: { type: DataTypes.UUID, allowNull: false },
      documentName: { type: DataTypes.STRING, allowNull: false },
      analysisData: { type: DataTypes.JSONB, allowNull: false },
    },
    {
      sequelize,
      modelName: 'Analysis',
    }
  );
  return Analysis;
};
```

### Note Model

Create `models/note.js`:

```javascript
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Note extends Model {
    static associate(models) {
      Note.belongsTo(models.User, { foreignKey: 'userId' });
      Note.belongsTo(models.Analysis, { foreignKey: 'analysisId' });
    }
  }
  Note.init(
    {
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true,
      },
      userId: { type: DataTypes.UUID, allowNull: false },
      analysisId: { type: DataTypes.UUID, allowNull: false },
      content: { type: DataTypes.TEXT, allowNull: false },
    },
    {
      sequelize,
      modelName: 'Note',
    }
  );
  return Note;
};
```

---

## 8. Establishing Model Associations

Update `models/index.js` to associate the models:

```javascript
const fs = require('fs');
const path = require('path');
const Sequelize = require('sequelize');
const basename = path.basename(__filename);
const env = process.env.NODE_ENV || 'development';
const config = require(__dirname + '/../config/config.js')[env];
const db = {};

let sequelize;
if (config.use_env_variable) {
  sequelize = new Sequelize(process.env[config.use_env_variable], config);
} else {
  sequelize = new Sequelize(
    config.database,
    config.username,
    config.password,
    config
  );
}

fs.readdirSync(__dirname)
  .filter(file => {
    return (
      file.indexOf('.') !== 0 && file !== basename && file.slice(-3) === '.js'
    );
  })
  .forEach(file => {
    const model = require(path.join(__dirname, file))(
      sequelize,
      Sequelize.DataTypes
    );
    db[model.name] = model;
  });

Object.keys(db).forEach(modelName => {
  if (db[modelName].associate) {
    db[modelName].associate(db);
  }
});

db.sequelize = sequelize;
db.Sequelize = Sequelize;

module.exports = db;
```

---

## 9. Database Migrations

### Creating Migrations

Use Sequelize CLI to generate migration files:

1. **User Migration:**

   ```bash
   npx sequelize-cli migration:generate --name create-user
   ```

   Edit the generated file in `migrations/` to define the `users` table.

2. **Analysis Migration:**

   ```bash
   npx sequelize-cli migration:generate --name create-analysis
   ```

   Edit the file to define the `analyses` table.

3. **Note Migration:**

   ```bash
   npx sequelize-cli migration:generate --name create-note
   ```

   Edit the file to define the `notes` table.

**Example Migration File for User:**

```javascript
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Users', {
      id: {
        type: Sequelize.UUID,
        defaultValue: Sequelize.UUIDV4,
        primaryKey: true,
      },
      username: { type: Sequelize.STRING, unique: true, allowNull: false },
      email: { type: Sequelize.STRING, unique: true, allowNull: false },
      password: { type: Sequelize.STRING, allowNull: false },
      createdAt: { type: Sequelize.DATE },
      updatedAt: { type: Sequelize.DATE },
    });
  },
  down: async queryInterface => {
    await queryInterface.dropTable('Users');
  },
};
```

Repeat similar steps for `Analysis` and `Note` models.

### Running Migrations

Execute migrations to create the tables in your database:

```bash
npx sequelize-cli db:migrate
```

---

## 10. Database Seeders (Optional)

If you need sample data for testing, create seeders:

```bash
npx sequelize-cli seed:generate --name demo-user
```

Edit the seeder file to insert sample data.

Run seeders:

```bash
npx sequelize-cli db:seed:all
```

---

## 11. Database Operations and Testing

### CRUD Operations

Implement CRUD operations in your controllers:

- **Create**: `User.create()`, `Analysis.create()`, `Note.create()`
- **Read**: `User.findByPk()`, `Analysis.findAll()`, `Note.findOne()`
- **Update**: `User.update()`, `Analysis.update()`, `Note.update()`
- **Delete**: `User.destroy()`, `Analysis.destroy()`, `Note.destroy()`

**Example: Creating a New User**

```javascript
const { User } = require('../models');
const bcrypt = require('bcrypt');

async function registerUser(req, res) {
  try {
    const hashedPassword = await bcrypt.hash(req.body.password, 10);
    const user = await User.create({
      username: req.body.username,
      email: req.body.email,
      password: hashedPassword,
    });
    res.status(201).json({ message: 'User registered successfully.' });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
}
```

### Writing Unit Tests

Use a testing framework like **Jest** or **Mocha** with **Supertest**:

- **Install Dependencies:**

  ```bash
  npm install --save-dev jest supertest
  ```

- **Example Test Case:**

  ```javascript
  const request = require('supertest');
  const app = require('../app');
  const { sequelize } = require('../models');

  beforeAll(async () => {
    await sequelize.sync({ force: true });
  });

  afterAll(async () => {
    await sequelize.close();
  });

  describe('User Registration', () => {
    test('It should register a new user', async () => {
      const response = await request(app)
        .post('/api/auth/register')
        .send({
          username: 'testuser',
          email: 'test@example.com',
          password: 'password123',
        });
      expect(response.statusCode).toBe(201);
    });
  });
  ```

---

## 12. Environment Configuration

Use environment variables to manage database configurations securely:

- **Create a `.env` file:**

  ```
  DATABASE_USERNAME=your_db_username
  DATABASE_PASSWORD=your_db_password
  DATABASE_NAME=tcdecoder_dev
  DATABASE_HOST=127.0.0.1
  DATABASE_DIALECT=postgres
  ```

- **Update `config/config.js`:**

  ```javascript
  require('dotenv').config();

  module.exports = {
    development: {
      username: process.env.DATABASE_USERNAME,
      password: process.env.DATABASE_PASSWORD,
      database: process.env.DATABASE_NAME,
      host: process.env.DATABASE_HOST,
      dialect: process.env.DATABASE_DIALECT,
    },
    // ... other environments
  };
  ```

**Important:** Do not commit the `.env` file to version control. Add it to `.gitignore`.

---

## 13. Security Considerations

### Handling Sensitive Data

- **Password Hashing:**

  - Use `bcrypt` to hash user passwords before storing them.

    ```javascript
    const bcrypt = require('bcrypt');
    const hashedPassword = await bcrypt.hash(password, saltRounds);
    ```

- **Data Encryption (if necessary):**

  - For sensitive fields, consider encrypting data at rest.

### Preventing SQL Injection

- **Parameterized Queries:**

  - Sequelize uses parameterized queries by default, reducing the risk of SQL injection.

- **Input Validation:**

  - Validate and sanitize user inputs using libraries like `validator.js`.

    ```javascript
    const validator = require('validator');
    if (!validator.isEmail(req.body.email)) {
      // Handle invalid email
    }
    ```

---

## 14. Performance Optimization

### Indexing

- **Add Indexes:**

  - Add indexes to frequently queried fields to improve query performance.

    ```javascript
    await queryInterface.addIndex('Users', ['email']);
    ```

### Query Optimization

- **Eager Loading:**

  - Use `include` in Sequelize queries to reduce the number of database calls.

    ```javascript
    const userWithAnalyses = await User.findOne({
      where: { id: userId },
      include: [Analysis],
    });
    ```

- **Pagination:**

  - Implement pagination for queries returning large datasets.

    ```javascript
    const analyses = await Analysis.findAll({
      limit: 10,
      offset: 0,
      where: { userId },
    });
    ```

---

## 15. Backup and Recovery Strategies

- **Regular Backups:**

  - Schedule regular backups of the database using tools like `pg_dump`.

- **Automated Backups (Production):**

  - Use managed database services that offer automated backups and point-in-time recovery.

---

## 16. Deployment Preparation

- **Database Configuration:**

  - Update production database configurations in the `config/config.js` file.

- **Migrations in Production:**

  - Run migrations on the production database cautiously.

    ```bash
    NODE_ENV=production npx sequelize-cli db:migrate
    ```

- **Environment Variables:**

  - Set environment variables securely on the production server.

- **SSL Connections:**

  - Ensure that the database connection uses SSL in production.

    ```javascript
    production: {
      // ... other configs
      dialectOptions: {
        ssl: {
          require: true,
          rejectUnauthorized: false,
        },
      },
    }
    ```

---

## 17. Conclusion

Phase 4 of the T&C Decoder Tool development focuses on establishing a robust and secure database foundation. By following this guide, you will set up the PostgreSQL database, define data models with Sequelize, establish relationships, and ensure the database is optimized for performance and security. This foundation is crucial for integrating with the backend API and supporting the application's core functionalities.

---

## 18. Appendix

### Additional Resources

- **PostgreSQL Documentation**: [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/)
- **Sequelize Documentation**: [https://sequelize.org/master/](https://sequelize.org/master/)
- **Sequelize CLI**: [https://sequelize.org/master/manual/migrations.html](https://sequelize.org/master/manual/migrations.html)
- **Node.js**: [https://nodejs.org/en/docs/](https://nodejs.org/en/docs/)
- **Express.js**: [https://expressjs.com/](https://expressjs.com/)
- **bcrypt**: [https://www.npmjs.com/package/bcrypt](https://www.npmjs.com/package/bcrypt)
- **validator.js**: [https://www.npmjs.com/package/validator](https://www.npmjs.com/package/validator)
- **Jest Testing Framework**: [https://jestjs.io/docs/en/getting-started](https://jestjs.io/docs/en/getting-started)

---

**Prepared by**: [Your Name], Lead Backend Developer  
**Date**: [Current Date]

---

If you have any questions or need further assistance during Phase 4 development, please feel free to reach out. Our goal is to ensure a solid database foundation for the T&C Decoder Tool to support current and future functionalities effectively.

---