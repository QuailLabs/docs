# Creating a PostgreSQL User for Quailrunner with Read-Only Access

This guide outlines the steps to create a PostgreSQL database user with read-only access for Quailrunner. Quailrunner requires read-only access to the tables you want to create tasks for, as well as any tables that need to be joined in the task queries.

## Why Read-Only Access?

Quailrunner performs no database writes to your database. Therefore, read-only access is the most secure configuration.

## Steps

### Connect to your PostgreSQL database as a superuser (usually `postgres`)

```
psql -U postgres
```

### Create a new user (role) for Quailrunner

The user name and password can be anything you want, but it's recommended to use a strong password.

```
CREATE USER quail_runner WITH PASSWORD 'secure_password';
```

### Connect to your specific database

```
\c your_database_name
```

### Grant `USAGE` on the schema (usually `public`)

```
GRANT USAGE ON SCHEMA public TO quail_runner;
```

### Grant `SELECT` privileges on specific tables that Quailrunner needs to access

```
GRANT SELECT ON table_name1 TO quail_runner;
GRANT SELECT ON table_name2 TO quail_runner;
```

Repeat this command for each table Quailrunner needs to access for tasks and joins.

### If you want to grant read access to all existing tables in the current database:

```
GRANT SELECT ON ALL TABLES IN SCHEMA public TO quail_runner;
```

### To ensure Quailrunner has `SELECT` privileges on tables created in the future:

```
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO quail_runner;
```

## Example

Here's a complete example:

```sql
-- Connect as superuser
psql -U postgres

-- Create the Quailrunner user
CREATE USER quail_runner WITH PASSWORD 'very_secure_password';

-- Connect to your database
\c your_database_name

-- Grant schema usage
GRANT USAGE ON SCHEMA public TO quail_runner;

-- Grant SELECT privileges on specific tables
GRANT SELECT ON users TO quail_runner;
GRANT SELECT ON orders TO quail_runner;
GRANT SELECT ON products TO quail_runner;

-- Optionally, grant SELECT on all existing tables
GRANT SELECT ON ALL TABLES IN SCHEMA public TO quail_runner;

-- Optionally, set default privileges for future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO quail_runner;
```

## Notes

- Replace `public` with your schema name if you're not using the default public schema
- Always use strong, unique passwords for the Quailrunner database user
- Regularly review and audit the permissions granted to Quailrunner
- Consider using connection limits for the Quailrunner user:
  ```
  ALTER ROLE quail_runner CONNECTION LIMIT 5;
  ```

## Additional PostgreSQL-Specific Considerations

1. Object Ownership: Ensure tables are owned by a different role to maintain security.
2. Row-Level Security: If you need more granular control over what data Quailrunner can access within tables, consider using PostgreSQL's row-level security features.
3. Revoking Permissions: If you need to revoke permissions from Quailrunner:
   ```
   REVOKE SELECT ON table_name FROM quail_runner;
   ```
4. Monitoring: Regularly monitor Quailrunner's database activity to ensure it's operating as expected.
5. Remember to consult the official PostgreSQL documentation for the most up-to-date information and best practices.

## Quailrunner Configuration

After setting up the PostgreSQL user, make sure to update your Quailrunner configuration with the new database credentials.
