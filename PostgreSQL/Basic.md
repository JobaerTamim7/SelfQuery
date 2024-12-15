
# **Basic PostgreSQL Commands**
---
- **Connect to a Database**  

    To connect to a PostgreSQL database:
    ```bash
    psql -d <db_name> -U <user>  # Default is 'postgres' for both database and username
    ```
    - `-d` or `--dbname` : Specifies the name of the database to connect to.
    - `-U` or `--username` : Specifies the PostgreSQL user for authentication.

    By default, if no `<db_name>` or `<user>` is provided, it will connect to the `postgres` database and the `postgres` user.

    **Example**

    ```bash
    psql -d my_database -U my_user  # Connect to 'my_database' with user 'my_user'
    ```
---
- **Create Role**
    
    To create a role, use the following SQL syntax:
    ```sql
    CREATE ROLE <new_role_name>
    [
      [WITH]
      SUPERUSER | NOSUPERUSER         -- Grant or deny superuser privileges [NOSUPERUSER]
      CREATEDB | NOCREATEDB           -- Grant or deny database creation privileges [NOCREATEDB]
      CREATEROLE | NOCREATEROLE       -- Grant or deny role creation privileges [NOCREATEROLE]
      ENCRYPTED PASSWORD '<password>' -- Set an encrypted password for the role 
      | PASSWORD NULL                 -- Remove the password
      LOGIN | NOLOGIN                 -- LOGIN = user, NOLOGIN = group/role
      BYPASSRLS | NOBYPASSRLS         -- Allow or deny bypassing row-level security
      VALID UNTIL '<timestamp>'       -- Set a validity period for the role
      IN ROLE <role_name>    
      ROLE <role_name>      
      ADMIN <role_name>               -- Grant admin privileges over another role
    ];
    ```
    `CREATE ROLE <child_role> IN ROLE <parent_role>`

    `CREATE ROLE <parent_role> WITH ROLE <child_role>`

    `ROLE` vs `USER` : User is a special kind of role that has Login permission.
    - For more       : [CREATE ROLE](https://www.postgresql.org/docs/current/sql-createrole.html)
    - For ROLE       : [ROLE Attributes](https://www.postgresql.org/docs/current/role-attributes.html)
    - For Membership : [ROLE Membership](https://www.postgresql.org/docs/current/role-membership.html)
    
    *Modifications can be done with ALTER ROLE after creating ROLE*
    
    **Example 1**
    ```sql
    CREATE ROLE employee
    WITH
    LOGIN                   -- He is a user
    PASSWORD 'hackermind'   -- This is password
    NOSUPERUSER             -- Non super user
    NOCREATEDB              -- Cannot create DB
    NOCREATEROLE            -- Cannot create new role (user or group)
    VALID UNTIL '2030-12-31 -- Valid until the end of 2030
    ```
    **Example 2**
    ```sql
    CREATE ROLE developer
    WITH 
    NOLOGIN                     --This role is a group
    CREATEDB                    --Can create DB
    PASSWORD NULL               --No password needed
    NOBYPASSRLS                 --Cannot bypass row level security
    VALID UNTIL '2030-12-31'    --Valid until the end of 2030
    ```
    **Example 3**
    ```sql
    CREATE ROLE manager
    WITH 
    IN ROLE developer      -- Inherits privileges from the `developer` role
    ADMIN employee         -- Owns privileges of the `employee` role
    CREATEROLE             -- Can create new roles
    LOGIN                  -- Managers can log in directly
    PASSWORD 'manager123'
    VALID UNTIL '2030-12-31';
    ```
    *Inherit vs Own : Inherit the previliges where own means can control over the previliges (add or revoke).*
---
- **Create Database**

    To create a PostgreSQL database:
    ```sql
    CREATE DATABASE <db_name>
    [ 
    [WITH] 
    OWNER [=] <user> --user who will have total control over the database
    TEMPLATE [=] <template> --by default template1
    TABLESPACE [=] <tablespace_name> --table space (different area of disk) where files are stored
    ALLOW_CONNECTIONS [=] allowconn  --allowconn = true/false. Defines if the new connection is allowed.
    CONNECTION LIMIT [=] connlimit  --max number of connections. For infinite -1
    ]
    ```
    - For more : [CREATE DATABASE](https://www.postgresql.org/docs/current/sql-createdatabase.html)

    **Example**
   ```sql
	CREATE DATABASE my_database
    WITH 
    OWNER = my_user             -- The user 'my_user' will have full control over the database
    TEMPLATE = template1        -- Using the default template 'template1'
    TABLESPACE = my_tablespace  -- Store the database in the specified tablespace
    ALLOW_CONNECTIONS = true    -- Allow connections to this database
    CONNECTION LIMIT = 100;     -- Limit the number of concurrent connections to 100
    ```
---
- **Create Table**

    To create table : 
    ```sql
    CREATE [TEMP] [UNLOGGED] TABLE [IF NOT EXISTS] <table_name> OF <table_type> (
    <column_name> <data_type> [CONSTRAINT <constraint_name> <constraint_type>] | LIKE <source_table>
    )
    [INHERITS(parent_table)]
    [PARTITION BY {RANGE | LIST | HASH}]
    [ TABLESPACE tablespace_name ];
    ```
    * TEMP : For just one  session
    * UNLOGGED : Faster table sacrificing loging backup
    * INHERIT : 
    
    *Parent to Child:* When you create a child table using INHERITS (parent_table), the child              inherits the columns and constraints of the parent table. However, after the child table is created,           changes to the parent table (e.g., adding/removing columns) will not affect the child table.

    *Child to Parent:* Conversely, if you change the child table (e.g., adding a new column to the child), the parent table will not be affected. The parent table's structure remains the same.
    - For More : [CREATE TABLE](https://www.postgresql.org/docs/current/sql-createtable.html#SQL-CREATETABLE-PARMS-INHERITS)
    
    **Example 1 : Column level Constraints**
    ```sql
    CREATE TABLE films(
        code char(5) CONSTRAINT film_code PRIMARY KEY,                          --Column that have string with fix length 5. Constraint name is firstkey and type is PRIMARY KEY
        title varchar(100) CONSTRAINT NOT NULL,                                 --Column that have string with var length 100. Constraint type is not null
        production_date DATE CONSTRAINT CHECK(production_date <= CURRENT_DATE)  --Column that have date data type. Constraint type is CHECK(expression)
        kind VARCHAR(10) CONSTRAINT CHECK (kind IN ('Action', 'Comedy', 'Drama', 'Documentary', 'Thriller')),  -- Check constraint for valid film kinds
        );
    ```
