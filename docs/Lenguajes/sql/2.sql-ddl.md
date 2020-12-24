# Definiendo la estructura de los datos

!!! info "Fuentes"
    - [W3Schools](https://www.w3schools.com/sql/sql_create_db.asp)
    - [Chancrovsky's blog](http://chancrovsky.blogspot.com/2014/12/constraints-o-restricciones.html)
    - [Documentación de MySQL](https://dev.mysql.com/doc/refman/8.0/en/create-table.html)

## Crear la base de datos 
Lo primero es crear una base de datos donde guardar las tablas:

```mysql
CREATE DATABASE nombre;
```

!!! note "Privilegios"
    Para crear una base de datos el usuario con el que hayas iniciado sesión debe tener los permisos necesarios. Una vez creada puedes consultar la lista de bases existentes con:

    ```mysql
    SHOW DATABASES;
    ```

## Crear las tablas

=== "Sintaxis"
    ```mysql
    CREATE TABLE nombre (
        columna1 tipoDato,
        columna2 tipoDato,
        columna3 tipoDato,
        ...
    );
    ```
=== "Ejemplo"
    ```mysql
    CREATE TABLE usuarios (
        id int,
        nombre varchar(255),
        apellidos varchar(255),
        dirección varchar(255)
    );
    ```

=== "A partir de otra tabla"
    ```mysql
    CREATE TABLE nuevo_nombre AS
        SELECT columna1, columna2,...
        FROM tabla_existente
        WHERE ...;

    -- Ejemplo
    CREATE TABLE prueba AS
        SELECT nombrecliente, nombrecontacto
        FROM clientes;
    ```

### Restricciones o _Constraints_
Las restricciones pueden especificarse al _crear_ las tablas o cuando se _modifican_.

=== "Con nombre"
    ```mysql
    CREATE TABLE nombre (
        columna1 tipo_dato,
        columna2 tipo_dato,
        columna3 tipo_dato,
        ...
        CONSTRAINT [PK|FK|UC|DF]_nombre (columna1[,columna2, columnan...])
    );
    ```
=== "En línea"
    ```mysql
    CREATE TABLE nombre (
        columna1 tipo_dato constraint,
        columna2 tipo_dato constraint,
        columna3 tipo_dato constraint,
        ...
    );
    ```
=== "Al final (sin nombre)"
    ```mysql
    CREATE TABLE nombre (
        columna1 tipo_dato constraint,
        columna2 tipo_dato,
        PRIMARY KEY (columna1)
    );
    ```

!!! note "Nombrar las restricciones"
    Existen variaciones en la forma exacta en la que se declaran las restricciones entre los dialectos de SQL, pero todos admiten **poner nombre** a las restricciones, al final de la declaración de columnas. Esta es también la única manera de crear **claves compuestas** con más de una columna.

#### `NOT NULL`
Impide que se dejen registros vacíos o _nulos_ en las columnas a las que se les aplica.

#### `UNIQUE`
Asegura que todos los valores de una columna _son diferentes_.

```mysql
-- sin nombre
ALTER TABLE personas
ADD UNIQUE (id);
-- con nombre
ADD CONSTRAINT UC_persona UNIQUE (id, apellidos);
```

```mysql
-- en mysql
ALTER TABLE personas
DROP INDEX UC_persona;
```

```tsql
ALTER TABLE personas
DROP CONSTRAINT UC_persona;
```

#### `PRIMARY KEY`
La clave primaria identifica de manera única cada registro de una tabla. Una restricción de clave primaria implica inmediatamente que los valores deben ser **únicos**.

#### `REFERENCES` y `FOREIGN KEY`
Una clave foránea _conecta_ varias tablas, enlazando una o varias columnas de la _tabla hijo_ con la **clave primaria** de una o varias _tablas padre_.

=== "MySQL"
    ```mysql
    CREATE TABLE Orders (
        OrderID int NOT NULL,
        OrderNumber int NOT NULL,
        PersonID int,
        PRIMARY KEY (OrderID),
        FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
    );
    ```

=== "SQL Server / Oracle / MS Access"
    ```sql
    CREATE TABLE Orders (
        OrderID int NOT NULL PRIMARY KEY,
        OrderNumber int NOT NULL,
        PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
    );
    ```

=== "Con nombre (MySQL/ SQL Server / Oracle / MS Access)"
    ```mysql
    CREATE TABLE Orders (
        OrderID int NOT NULL,
        OrderNumber int NOT NULL,
        PersonID int,
        PRIMARY KEY (OrderID),
        CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
        REFERENCES Persons(PersonID)
    );
    ```

#### `CHECK`
Esta restricción limita el rango de valores posibles para la columna indicada. Se puede definir:

- para una columna
    ```mysql
    CREATE TABLE Persons (
        ID int NOT NULL,
        LastName varchar(255) NOT NULL,
        FirstName varchar(255),
        Age int,
        CHECK (Age>=18)
    );
    ```

    ```sql
    CREATE TABLE Persons (
        ID int NOT NULL,
        LastName varchar(255) NOT NULL,
        FirstName varchar(255),
        Age int CHECK (Age>=18)
    );
    ```

- para varias columnas en función del valor de otras en la misma tabla
    ```mysql
    CREATE TABLE Persons (
        ID int NOT NULL,
        LastName varchar(255) NOT NULL,
        FirstName varchar(255),
        Age int,
        City varchar(255),
        CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
    );
    ```

#### `DEFAULT`
Proporciona un valor por defecto para una columna, que se añadirá si no se proporciona ninguno.

```mysql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255) DEFAULT 'Sandnes'
);
```

!!! tip "Insertar valores del sistema"
    La restricción `CHECK` también sirve para insertar valores proporcionados por el sistema, como la fecha actual:

    ```mysql
    CREATE TABLE Orders (
        ID int NOT NULL,
        OrderNumber int NOT NULL,
        OrderDate date DEFAULT GETDATE()
    );
    ```

#### `ON DELETE | UPDATE`

## Modificar la definición de una tabla

!!! note "Nota"
    Cuando hablamos de modificar aquí nos referimos a alterar la definición de las columnas que componen una tabla: cambiar el tipo de dato, añadir o quitar restricciones, o añadir y quitar columnas.

    Para modificar los **datos** introducidos se usan los comandos `INSERT` y `UPDATE` que se ven en el apartado siguiente.

=== "Sintaxis"
    ```mysql
    ALTER TABLE nombre
    ADD nombre_columna tipo_dato;
    ```
=== "Ejemplo"
    ```mysql
    ALTER TABLE clientes
    ADD email varchar(255);
    ```

### Eliminar una columna

```mysql
ALTER TABLE clientes
DROP COLUMN email;
```

### Modificar una columna

=== "SQL Server / MS Access"
    ```mysql
    ALTER TABLE nombre
    ALTER COLUMN nombre_columna tipo_dato;
    ```
=== "MySQL/ Oracle (&lt; v.10G)"
    ```mysql
    ALTER TABLE nombre
    MODIFY COLUMN nombre_columna tipo_dato;
    ```
=== "Oracle >= 10G"
    ```mysql
    ALTER TABLE nombre
    MODIFY nombre_columna tipo_dato;
    ```

## Borrar tablas y bases de datos

```mysql
-- Elimina una base de datos
DROP DATABASE nombre;
-- Elimina una tabla y su contenido
DROP TABLE nombre;
-- Elimina el contenido de una tabla pero NO la tabla
TRUNCATE TABLE nombre;
```

!!! note "Copia de seguridad"
    En **SQL server** se puede crear una copia de la base de datos desde terminal:

    ```tsql
    BACKUP DATABASE nombre
    TO DISK = 'ruta';
    -- Si ya existe, se puede actualizar añadiendo la opción "diferencial"
    BACKUP DATABASE nombre
    TO DISK = 'ruta'
    WITH DIFFERENTIAL;
    ```
