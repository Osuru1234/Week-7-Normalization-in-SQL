# Audit Logging, Category Trees, and Safe Migrations

## Overview

This project demonstrates PostgreSQL audit logging, hierarchical category data, versioned database migrations with Flyway, and least-privilege database security.

The work was completed using PostgreSQL 18.6 and Flyway.

## 1. Audit Logging

An `audit_log` table was created to automatically record changes made to the `students` table.

The audit system records:

- Table name
- Operation performed (`INSERT`, `UPDATE`, `DELETE`)
- Previous row data
- New row data
- User who made the change
- Date and time of the change

An audit trigger was created on the `students` table to capture these changes automatically.

### Audit Testing

The audit system was tested by:

- Updating a student's name
- Deleting a student
- Querying the `audit_log` table to verify the recorded changes

The audit log successfully captured the update and delete operations.

## 2. Hierarchical Category Tree

A `categories` table was created with a self-referencing `parent_id` column.

Example category structure:

```text
Electronics
  Computers
    Laptops
  Phones
```

A recursive CTE was used to query and display the category hierarchy.

## 3. Versioned Migrations with Flyway

Flyway was configured to manage the database migration history.

Migration files:

```text
sql/
├── V1__core_tables.sql
├── V2__audit_log.sql
└── V3__categories.sql
```

Because the database schema already existed before Flyway was introduced, the existing schema was baselined before the remaining migrations were applied.

### Final Flyway Status

```text
Database: PostgreSQL 18.6
Schema version: 3

V1  core tables       Ignored (Baseline)
V1  Flyway Baseline   Baseline
V2  audit log         Success
V3  categories        Success
```

Flyway validation was also completed successfully:

```text
Successfully validated 4 migrations
```

This confirms that the Flyway migration history is valid and the database is currently at schema version 3.

## 4. Least-Privilege Database Security

Two database roles were created:

- `app_read` – read-only access
- `app_write` – read and write access

The following permissions were configured:

- Database connection access
- Schema usage
- SELECT permissions for `app_read`
- SELECT, INSERT, UPDATE, and DELETE permissions for `app_write`

An API login user named `api` was created and assigned to the `app_write` role.

This demonstrates the principle of least privilege by giving users only the database permissions required for their intended activities.

## 5. Verification

The implementation was verified using PostgreSQL commands and Flyway commands.

Verification included:

- Checking database tables
- Testing audit logging
- Querying the category tree
- Running Flyway migrations
- Running `flyway info`
- Running `flyway validate`
- Checking database roles and privileges

## Technologies Used

- PostgreSQL 18.6
- SQL
- PL/pgSQL
- Flyway
- Windows Command Prompt
- Git
- GitHub

## Repository Structure

```text
Week-7-Normalization-in-SQL/
│
├── .gitignore
├── README.md
└── sql/
    ├── V1__core_tables.sql
    ├── V2__audit_log.sql
    └── V3__categories.sql
```

## Security Note

The database password is stored in a local `flyway.conf` file and is excluded from Git using `.gitignore`. The password is not included in this repository.

## Learning Outcomes

Through this project, I practiced:

- PostgreSQL triggers and functions
- Audit logging
- JSONB data
- Recursive CTEs
- Hierarchical database structures
- Flyway versioned migrations
- Database baselining
- Database roles and privileges
- Least-privilege security
- Git and GitHub workflow

## Conclusion

This project demonstrates how PostgreSQL can be used to track database changes, represent hierarchical data, manage schema changes safely with Flyway, and control database access using roles and privileges.
