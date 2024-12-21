
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
    <column_name> <data_type> [[CONSTRAINT <constraint_name>] <constraint_type>] | LIKE <source_table>
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
    - Data Types Chart : [Data Types](https://www.geeksforgeeks.org/postgresql-data-types/)
    
    **Example 1 : Column level Constraints**
    ```sql
    CREATE TABLE films(
        code char(5) CONSTRAINT film_code PRIMARY KEY,                          --Column that have string with fix length 5. Constraint name is firstkey and type is PRIMARY KEY
        title VARCHAR(100) NOT NULL,                                            --Column that have string with var length 100. Constraint type is not null
        production_date DATE CHECK(production_date <= CURRENT_DATE),             --Column that have date data type. Constraint type is CHECK(expression)
        kind VARCHAR(10) CHECK (kind IN ('Action', 'Comedy', 'Drama', 'Documentary', 'Thriller')),  -- Check constraint for valid film kinds
        entry_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    ```
    **Example 2 : Table level Constraints** 
    ```sql
    CREATE TABLE employees(
        employee_id SERIAL PRIMARY KEY,
        first_name VARCHAR(100),
        last_name VARCHAR(100),
        dept_id INTEGER NOT NULL,
        salary NUMERIC(10,2),
        CONSTRAINT chk_salary CHECK (Salary > 0 AND FirstName <> '' AND LastName <> ''),
        CONSTRAINT unique_name UNIQUE(FirstName,LastName),
        CONSTRAINT fk_dept FOREIGN KEY(DeptID) REFERENCES Departments(ID)
        ON DELETE CASCADE   --Ensures that when a department id ( basically dept ) is deleted all the data of the emplyees also is deleted
        ON UPDATE CASCADE   --Ensures that when a dept id is updated in Departments the Employees DeptID also updates
    );
    ```
    **Example 3 : Table level Constraint**
    ```sql
    CREATE TABLE circles(
        circle_id SERIAL PRIMARY KEY,
        center POINT NOT NULL,
        radius DOUBLE PRECISION NOT NULL CHECK(radius>0),
        EXCLUDE USING GIST(
            center WITH &&,
            (circle(center, radius)) WITH &&
        )
    )
    ```
    *Column level is recommneded for being faster*
    
    *Learn more about gist and overlaping operator (&&) in INDEX and OPERATOR and FUNCTIONS*
    
    **Example 4 : User Types**
    ```sql
    --Creating ENUM type
    CREATE TYPE mood AS ENUM('happy','sad','neutral');
    --Creating Composite type
    CREATE TYPE address AS (
        street VARCHAR(100),
        city VARCHAR(50),
        zip_code VARCHAR(10)
    );
    CREATE TABLE person OF address(
        name VARCHAR(100),
        person_mood mood
    )
    ```
---
- **Inserting into table :**

    To insert a row in a table : 
    ```sql
    INSERT INTO <table_name> [AS <alias>] 
    (
        column_name[...]  -- Names of column name separated with comma.
    )
    [OVERRIDING {USER | SYSTEM} VALUE]
    {VALUES() | <query>}
    ```
    
    - For more : [INSERT INTO](https://www.postgresql.org/docs/current/sql-insert.html)
    
    **EXAMPLE**
    ```sql
    -- Multiple insertions
    INSERT INTO films (code, title, did, date_prod, kind) VALUES
        ('B6717', 'Tampopo', 110, '1985-02-10', 'Comedy'),
        ('HG120', 'The Dinner Game', 140, DEFAULT, 'Comedy');
    --Insert with query
    INSERT INTO films SELECT * FROM tmp_films WHERE date_prod < '2004-05-07'; -- Insert every entry from tmp_films table where condition is met
    ```
    **Example : Inserting 1D or 2D array**
    ```sql
    CREATE TABLE exam_data(
        q_id SERIAL PRIMARY KEY,
        q_text TEXT,
        answer INTEGER CONSTRAINT valid_option CHECK(answer > 0 AND answer < 5),
        student_response INTEGER[][]
    );
    -- adding options column to store options 
    ALTER TABLE exam_data
    ADD COLUMN options TEXT[];
    
    --check options length is 4
    ALTER TABLE exam_data
    ADD CONSTRAINT valid_options_len
    CHECK(array_length(options,1) = 4);


    INSERT INTO exam_data(q_text,options, answer, student_response)
    VALUES(
        'Who is the inventor of Atomic Bomb?',
        '{'Oppenheimer','Bhor','Leslie Groves','Erwin Schrödinger'}',
        1,
        '{{1,0},{5,1},{3,0}}' -- {1,0} means 1st student attemped once but failed.
    )
    ```
---
- **Select from table**
    
    To select something :
    ```sql
    SELECT [ ALL | DISTINCT [ON(expression[...])] ]             -- ALL: all rows (default) | DISTINCT: distinct rows
    [{* | column_name | expression [AS output_name)] } ]
    [FROM from_item[...]]
    [ WHERE condition ]
    [ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] [HAVING condition] ]
    [ ORDER BY expression [ ASC | DESC ]]                      -- Default ASC
    [ LIMIT { count | ALL } ] 
    [ OFFSET start ]
    [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } { ONLY | WITH TIES } ]    -- ONLY: Exact | WITH TIES: Take all the value with tied. (i.e. 3 WITH TIES -> 1000,1000,2000,2000)
    ```
    *from_items* : table, view or result of subquery (). It also can be from multiple tables.

    *grouping_elements* : can be a column or mlultiple ones which is the of group.
    
    *How group works* : At first it organize the rows based on column(s) making it a group. After that it use calculations. 
    
    - For more: [SELECT](https://www.postgresql.org/docs/current/sql-select.html)

    **EXAMPLE**
    
    ```sql
    SELECT * FROM my_table;
    
    SELECT DISTINCT dept FROM company;
    
    SELECT DISTINCT ON(dept) dept, name FROM company;       -- Gives dept and name column on distinct dept
    
    SELECT * FROM company;
    
    SELECT name FROM (SELECT * FROM company WHERE salary > 5000) AS high_salary; -- Table alias name is high_salary
    
    SELECT seller_id, SUM(sales_amount) AS total_sale
    FROM sales
    GROUP BY seller_id
    HAVING SUM(sales_amount) > 5000
    ORDER BY total_sale
    LIMIT 30 OFFSET 5;
    
    ```

