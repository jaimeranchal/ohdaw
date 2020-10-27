# Bases de Datos con PHP

!!! info "Fuente"
    Lo siguiente es una adaptación de la [guía de W3Schools](https://www.w3schools.com/php/php_mysql_connect.asp).

!!! note "Objetivos"
    - Acceso a bbdd
    - MySQL
        - Instalación y configuración
        - Herramientas de administración
            - mysql y mysqladmin
            - phpMyAdmin
        - Php y MySQL
            - MySQLi
                - Conexión
                - Consultas
                - Transacciones
                - Conjuntos de resultados
                - Consultas preparadas
            - PDO (PHP Data Objects)
                - Conexión
                - Consultas
                - Conjuntos de resultados
                - Consultas preparadas
        - Errores y excepciones

## Extensiones para trabajar con bases de datos

A partir de PHP5 podemos usar:

- **Extensión MySQLi**(mproved)
- **PDO** (PHP Data Objects)

La elección es un poco al gusto del consumidor, o dependiendo de las necesidades:

- PDO es compatible con hasta 12 sistemas diferentes de bases de datos, mientras que MySQLi está limitada a MySQL.
- Ambas están orientadas a Objetos, pero MySQLi ofrece una API procedural
- Ambas soportan _Sentencias preparadas_ (para proteger contra ataques de inyección SQL).

!!! note "Extensión MySQL"
    Es la extensión utilizada en versiones anteriores de PHP; obsoleta desde 2012.

### Instalación

=== "MySQLi"
    Normalmente viene incluida en la instalación por defecto de PHP >= 5.

    Para una instalación manual o en Windows, ver [más información en PHP.net](https://www.php.net/manual/en/mysqli.installation.php)

=== "PDO"
    PDO y el driver PDO_SQLITE están habilitados por defecto en PHP >= 5.1.0. Aún así es posible que tengas que habilitarlo para la base de datos concreta que vayas a usar. Para ello consulta los [controladores PDO específicos](https://www.php.net/manual/en/pdo.drivers.php)

    !!! note "Nota"
        Cuando se instala PDO como una _extensión compartida_ (no recomendado), todos los drivers PDO _deben_ cargarse _después_ del propio PDO.
        
        Para instrucciones sobre la instalación o activación en Windows ver la [documentación en PHP.net](https://www.php.net/manual/en/pdo.installation.php).

## Conexión

=== "MySQLi"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    // Create connection
    $conn = new mysqli($servername, $username, $password);

    // Check connection
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }
    echo "Connected successfully";

    // Cerrar la conexión
    $conn->close();
    ?>
    ```

    !!! note "Connect-error"
        La variable `$connect_error` estuvo rota hasta las versión 5.2.9 y 5.3.0. Si necesitas garantizar la compatibilidad con versiones anteriores usa el siguiente código:

        ```php
        // Comprueba la conexión
        if (mysqli_connect_error()){
            die("Database connection failed:" . mysqli_connect_error());
        }
        ```
=== "MySQLi procedural"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    // Create connection
    $conn = mysqli_connect($servername, $username, $password);

    // Check connection
    if (!$conn) {
      die("Connection failed: " . mysqli_connect_error());
    }
    echo "Connected successfully";

    // Cerrar la conexión
    mysqli_close($conn);
    ?>
    ```
    
=== "PDO"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=myDB", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      echo "Connected successfully";
    } catch(PDOException $e) {
      echo "Connection failed: " . $e->getMessage();
    }

    // Cerrar la conexión
    $conn = null;
    ?>
    ```

    !!! warning "Nombre de la base de datos"
        PDO necesita **especificar el nombre de la base de datos** (`myDB` en el ejemplo); de lo contrario, lanzará un error.

    PDO tiene su propia clase de excepciones para manejar los problemas que ocurran (`$e` en el ejemplo).
    
## Sentencias create
### Crear database

=== "MySQLi"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    // Create connection
    $conn = new mysqli($servername, $username, $password);
    // Check connection
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }

    // Create database
    $sql = "CREATE DATABASE myDB";
    if ($conn->query($sql) === TRUE) {
      echo "Database created successfully";
    } else {
      echo "Error creating database: " . $conn->error;
    }

    $conn->close();
    ?>
    ```

    Sólo se proporcionan los tres argumentos (servidor, usuario, contraseña) para el objeto mysqli.
    
    !!! tip "Puertos específicos"
        Si usas un puerto en particular, añade una cadena vacía para el parámetro del nombre del servidor: `#!php new mysqli("localhost", "username", "password", "", port);`

=== "MySQLi procedural"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    // Create connection
    $conn = mysqli_connect($servername, $username, $password);
    // Check connection
    if (!$conn) {
      die("Connection failed: " . mysqli_connect_error());
    }

    // Create database
    $sql = "CREATE DATABASE myDB";
    if (mysqli_query($conn, $sql)) {
      echo "Database created successfully";
    } else {
      echo "Error creating database: " . mysqli_error($conn);
    }

    mysqli_close($conn);
    ?>
    ```
    
=== "PDO"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";

    try {
      $conn = new PDO("mysql:host=$servername", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $sql = "CREATE DATABASE myDBPDO";
      // use exec() because no results are returned
      $conn->exec($sql);
      echo "Database created successfully<br>";
    } catch(PDOException $e) {
      // echo la sentencia que da el fallo y el código de error
      echo $sql . "<br>" . $e->getMessage();
    }

    $conn = null;
    ?>
    ```

### Crear tablas
Vamos a crear una tabla llamada "MyGuests" con _cinco columnas_: id, firstname, lastname, email y reg_date.

```sql
CREATE TABLE MyGuests (
id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
firstname VARCHAR(30) NOT NULL,
lastname VARCHAR(30) NOT NULL,
email VARCHAR(50),
reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

=== "MySQLi"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDB";

    // Create connection
    $conn = new mysqli($servername, $username, $password, $dbname);
    // Check connection
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }

    // sql to create table
    $sql = "CREATE TABLE MyGuests (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(30) NOT NULL,
    lastname VARCHAR(30) NOT NULL,
    email VARCHAR(50),
    reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    )";

    if ($conn->query($sql) === TRUE) {
      echo "Table MyGuests created successfully";
    } else {
      echo "Error creating table: " . $conn->error;
    }

    $conn->close();
    ?>
    ```

=== "MySQLi procedural"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDB";

    // Create connection
    $conn = mysqli_connect($servername, $username, $password, $dbname);
    // Check connection
    if (!$conn) {
      die("Connection failed: " . mysqli_connect_error());
    }

    // sql to create table
    $sql = "CREATE TABLE MyGuests (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(30) NOT NULL,
    lastname VARCHAR(30) NOT NULL,
    email VARCHAR(50),
    reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    )";

    if (mysqli_query($conn, $sql)) {
      echo "Table MyGuests created successfully";
    } else {
      echo "Error creating table: " . mysqli_error($conn);
    }

    mysqli_close($conn);
    ?>
    ```
    
=== "PDO"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

      // sql to create table
      $sql = "CREATE TABLE MyGuests (
      id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
      firstname VARCHAR(30) NOT NULL,
      lastname VARCHAR(30) NOT NULL,
      email VARCHAR(50),
      reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
      )";

      // use exec() because no results are returned
      $conn->exec($sql);
      echo "Table MyGuests created successfully";
    } catch(PDOException $e) {
      echo $sql . "<br>" . $e->getMessage();
    }

    $conn = null;
    ?>
    ```

## Insertar datos
Una vez creada la base de datos, podemos empezar a añadir datos.

Algunas normas de **sintaxis** básicas:

- Las _queries_ deben estar **entre comillas** en PHP.
- Los _valores de cadena_ dentro de SQL deben estar también **entre comillas**
- Los _valores numéricos_ **no llevan comillas**.
- La palabra `NULL` **nunca lleva comillas**.

Vamos a usar una sentencia `INSERT` de este tipo:

```sql
INSERT INTO table_name (column1, column2, column3,...)
VALUES (value1, value2, value3,...)
```

!!! note "Valores autocalculados"
    Si una columna tiene la opción `AUTO_INCREMENT` o `TIMESTAMP` con el formato de actualización por defecto, **no hace falta indicar sus valores**; MySQL los añadirá automáticamente.

=== "MySQLi"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDB";

    // Create connection
    $conn = new mysqli($servername, $username, $password, $dbname);
    // Check connection
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }

    $sql = "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('John', 'Doe', 'john@example.com')";

    if ($conn->query($sql) === TRUE) {
      echo "New record created successfully";
    } else {
      echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
    ?>
    ```

=== "MySQLi procedural"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDB";

    // Create connection
    $conn = mysqli_connect($servername, $username, $password, $dbname);
    // Check connection
    if (!$conn) {
      die("Connection failed: " . mysqli_connect_error());
    }

    $sql = "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('John', 'Doe', 'john@example.com')";

    if (mysqli_query($conn, $sql)) {
      echo "New record created successfully";
    } else {
      echo "Error: " . $sql . "<br>" . mysqli_error($conn);
    }

    mysqli_close($conn);
    ?>
    ```
    
=== "PDO"
    ```php
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $sql = "INSERT INTO MyGuests (firstname, lastname, email)
      VALUES ('John', 'Doe', 'john@example.com')";
      // use exec() because no results are returned
      $conn->exec($sql);
      echo "New record created successfully";
    } catch(PDOException $e) {
      echo $sql . "<br>" . $e->getMessage();
    }

    $conn = null;
    ?>
    ```
    

### Inserción múltiple

=== "MySQLi"
=== "PDO"

## Obtener la última id
=== "MySQLi"
=== "PDO"

## Sentencias preparadas
=== "MySQLi"
=== "PDO"

## Seleccionar datos
=== "MySQLi"
=== "PDO"

### Condición `where`
=== "MySQLi"
=== "PDO"

### Condición `Order by`
=== "MySQLi"
=== "PDO"

## Borrar datos
=== "MySQLi"
=== "PDO"

## Actualizar datos
=== "MySQLi"
=== "PDO"

## Limitar los datos
=== "MySQLi"
=== "PDO"
