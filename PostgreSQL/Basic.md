
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

- **Create Database**

    To create a PostgreSQL database:
    ```sql
    CREATE DATABASE <db_name>
    [WITH] 
    [ OWNER [=] <user> ] --user who will have total control over the database
    [ TEMPLATE [=] <template> ] --by default template1
    [ TABLESPACE [=] <tablespace_name> ] --table space (different area of disk) where files are stored
    [ ALLOW_CONNECTIONS [=] allowconn ] --allowconn = true/false. Defines if the new connection is allowed.
    [ CONNECTION LIMIT [=] connlimit ] --max number of connections. For infinite -1
    ```
    - For more : [CREATE DATABASE](https://www.postgresql.org/docs/current/sql-createdatabase.html)

    **Example**
   ```sql
	CREATE DATABASE my_database
    WITH 
    OWNER = my_user  -- The user 'my_user' will have full control over the database
    TEMPLATE = template1  -- Using the default template 'template1'
    TABLESPACE = my_tablespace  -- Store the database in the specified tablespace
    ALLOW_CONNECTIONS = true  -- Allow connections to this database
    CONNECTION LIMIT = 100;  -- Limit the number of concurrent connections to 100
    ```
