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

=== "PDO"
    PDO y el driver PDO_SQLITE están habilitados por defecto en PHP >= 5.1.0. Aún así es posible que tengas que habilitarlo para la base de datos concreta que vayas a usar. Para ello consulta los [controladores PDO específicos](https://www.php.net/manual/en/pdo.drivers.php)

    !!! note "Nota"
        Cuando se instala PDO como una _extensión compartida_ (no recomendado), todos los drivers PDO _deben_ cargarse _después_ del propio PDO.
        
        Para instrucciones sobre la instalación o activación en Windows ver la [documentación en PHP.net](https://www.php.net/manual/en/pdo.installation.php).

=== "MySQLi"
    Normalmente viene incluida en la instalación por defecto de PHP >= 5.

    Para una instalación manual o en Windows, ver [más información en PHP.net](https://www.php.net/manual/en/mysqli.installation.php)


## Conexión

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

## Sentencias create
### Crear database

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

### Obtener la última id

Si realizamos un `INSERT` o `UPDATE` en una tabla con un campo _autoincrementado_, podemos obtener el ID del último registro inmediatamente.

Imaginemos esta tabla:

```mysql
CREATE TABLE MyGuests (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(30) NOT NULL,
    lastname VARCHAR(30) NOT NULL,
    email VARCHAR(50),
    reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

Los siguientes ejemplos son los mismos que el apartado anterior, añadiendo una línea que guarda el ID en una variable y otra que la imprime:

=== "PDO"
    ```php hl_lines="11 12"
    <?php
    ...
    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $sql = "INSERT INTO MyGuests (firstname, lastname, email)
      VALUES ('John', 'Doe', 'john@example.com')";
      // use exec() because no results are returned
      $conn->exec($sql);
      $last_id = $conn->lastInsertId();
      echo "New record created successfully. Last inserted ID is: " . $last_id;
    } catch(PDOException $e) {
      echo $sql . "<br>" . $e->getMessage();
    }
    ?>
    ```

=== "MySQLi"
    ```php hl_lines="7 8"
    <?php
    ...
    $sql = "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('John', 'Doe', 'john@example.com')";

    if ($conn->query($sql) === TRUE) {
      $last_id = $conn->insert_id;
      echo "New record created successfully. Last inserted ID is: " . $last_id;
    } else {
      echo "Error: " . $sql . "<br>" . $conn->error;
    }
    ?>
    ```

=== "MySQLi procedural"
    ```php hl_lines="6 7"
    <?php
    $sql = "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('John', 'Doe', 'john@example.com')";

    if (mysqli_query($conn, $sql)) {
      $last_id = mysqli_insert_id($conn);
      echo "New record created successfully. Last inserted ID is: " . $last_id;
    } else {
      echo "Error: " . $sql . "<br>" . mysqli_error($conn);
    }
    ?>
    ```

### Inserción múltiple

Se pueden ejecutar varias sentencias a la vez:

=== "PDO (usando transacciones)"
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

      // begin the transaction
      $conn->beginTransaction();
      // our SQL statements
      $conn->exec("INSERT INTO MyGuests (firstname, lastname, email)
      VALUES ('John', 'Doe', 'john@example.com')");
      $conn->exec("INSERT INTO MyGuests (firstname, lastname, email)
      VALUES ('Mary', 'Moe', 'mary@example.com')");
      $conn->exec("INSERT INTO MyGuests (firstname, lastname, email)
      VALUES ('Julie', 'Dooley', 'julie@example.com')");

      // commit the transaction
      $conn->commit();
      echo "New records created successfully";
    } catch(PDOException $e) {
      // roll back the transaction if something failed
      $conn->rollback();
      echo "Error: " . $e->getMessage();
    }

    $conn = null;
    ?>
    ```

=== "MySQLi (función multi_query)"
    ```php hl_lines="21"
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
    VALUES ('John', 'Doe', 'john@example.com');";
    $sql .= "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('Mary', 'Moe', 'mary@example.com');";
    $sql .= "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('Julie', 'Dooley', 'julie@example.com')";

    if ($conn->multi_query($sql) === TRUE) {
      echo "New records created successfully";
    } else {
      echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
    ?>
    ```

## Sentencias preparadas

!!! note "Seguridad"
    Las sentencias preparadas son muy útiles contra ataques de inyección de código

Una sentencia preparada permite ejecutar de forma repetida la misma sentencia SQL (o con ligeras variaciones), de una forma muy eficiente.

Su funcionamiento básico es el siguiente:

1. **Preparación**: se crea una _plantilla_ de sentencia y se envía a la base de datos.
    - El valor concreto de algunos valores se sustituye por un carácter `?`; son como los _parámetros_ de una función.
    - `#!sql INSERT INTO Invitados VALUES(?,?,?);`
2. La base de datos recibe, compila y optimiza la plantilla, guardando el resultado _sin ejecutarla_.
3. **Ejecución**: más tarde, la aplicación _asigna valores_ a los parámetros y entonces se ejecuta la sentencia. La aplicación puede ejecutar la sentencia tantas veces como se quiera con diferentes valores.

!!! note "Marcadores de posición"
    Las sentencias preparadas usan signos para representar los valores _parametrizados_ (posiciones cuyo valor real se concretará después). Dependiendo del método que usemos puede ser un **signo de interrogación** (`?`) o una **cadena precedida de dos puntos** (`:nombre`).

Comparado con ejecutar sentencias SQL directamente, las sentencias preparadas tienen tres ventajas:

- Reducen el tiempo de procesamiento de las queries (sólo se hace una vez)
- El uso de parámetros reduce el ancho de banda usado (sólo se envían los valores concretos)
- Los valores de los parámetros se envían en un momento distinto y con un protocolo diferente al de la sentencia. Si la plantilla no proviene de una entrada de datos externa, no puede producirse un ataque de inyección SQL.

=== "PDO"
    ```php hl_lines="13 15 23"
     <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

      // prepare sql and bind parameters
      $stmt = $conn->prepare("INSERT INTO MyGuests (firstname, lastname, email)
      VALUES (:firstname, :lastname, :email)");
      $stmt->bindParam(':firstname', $firstname);
      $stmt->bindParam(':lastname', $lastname);
      $stmt->bindParam(':email', $email);

      // insert a row
      $firstname = "John";
      $lastname = "Doe";
      $email = "john@example.com";
      $stmt->execute();

      // insert another row
      $firstname = "Mary";
      $lastname = "Moe";
      $email = "mary@example.com";
      $stmt->execute();

      // insert another row
      $firstname = "Julie";
      $lastname = "Dooley";
      $email = "julie@example.com";
      $stmt->execute();

      echo "New records created successfully";
    } catch(PDOException $e) {
      echo "Error: " . $e->getMessage();
    }
    $conn = null;
    ?>   
    ```

    !!! info "Función bindParam()"
        Para asignar valores a los parámetros de la sentencia se usa la función `bindParam()`. Si los parámetros se indican con dos puntos y un nombre, la asignación se produce como en el ejemplo; si se usa el signo de interrogación como marcador de posición, la asignación se hace según la posición del índice 1:

        ```php
        <?php
        $stmt->bindParam(1, $firstname);
        $stmt->bindParam(2, $lastname);
        ...
        ?>
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

    // prepare and bind
    $stmt = $conn->prepare("INSERT INTO MyGuests (firstname, lastname, email) VALUES (?, ?, ?)");
    $stmt->bind_param("sss", $firstname, $lastname, $email);

    // set parameters and execute
    $firstname = "John";
    $lastname = "Doe";
    $email = "john@example.com";
    $stmt->execute();

    $firstname = "Mary";
    $lastname = "Moe";
    $email = "mary@example.com";
    $stmt->execute();

    $firstname = "Julie";
    $lastname = "Dooley";
    $email = "julie@example.com";
    $stmt->execute();

    echo "New records created successfully";

    $stmt->close();
    $conn->close();
    ?>
    ```

!!! note "Escoger un método"
    A partir de aquí los ejemplos serán sólo con PDO.

## Seleccionar datos

=== "PDO"
    ```php
        <?php
    echo "<table style='border: solid 1px black;'>";
    echo "<tr><th>Id</th><th>Firstname</th><th>Lastname</th></tr>";

    class TableRows extends RecursiveIteratorIterator {
      function __construct($it) {
        parent::__construct($it, self::LEAVES_ONLY);
      }

      function current() {
        return "<td style='width:150px;border:1px solid black;'>" . parent::current(). "</td>";
      }

      function beginChildren() {
        echo "<tr>";
      }

      function endChildren() {
        echo "</tr>" . "\n";
      }
    }

    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $stmt = $conn->prepare("SELECT id, firstname, lastname FROM MyGuests");
      $stmt->execute();

      // set the resulting array to associative
      $result = $stmt->setFetchMode(PDO::FETCH_ASSOC);
      foreach(new TableRows(new RecursiveArrayIterator($stmt->fetchAll())) as $k=>$v) {
        echo $v;
      }
    } catch(PDOException $e) {
      echo "Error: " . $e->getMessage();
    }
    $conn = null;
    echo "</table>";
    ?>
    ```

### Condición `WHERE`

=== "PDO"
    ```php
    <?php
    echo "<table style='border: solid 1px black;'>";
    echo "<tr><th>Id</th><th>Firstname</th><th>Lastname</th></tr>";

    class TableRows extends RecursiveIteratorIterator {
      function __construct($it) {
        parent::__construct($it, self::LEAVES_ONLY);
      }

      function current() {
        return "<td style='width:150px;border:1px solid black;'>" . parent::current(). "</td>";
      }

      function beginChildren() {
        echo "<tr>";
      }

      function endChildren() {
        echo "</tr>" . "\n";
      }
    }

    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $stmt = $conn->prepare("SELECT id, firstname, lastname FROM MyGuests WHERE lastname='Doe'");
      $stmt->execute();

      // set the resulting array to associative
      $result = $stmt->setFetchMode(PDO::FETCH_ASSOC);
      foreach(new TableRows(new RecursiveArrayIterator($stmt->fetchAll())) as $k=>$v) {
        echo $v;
      }
    }
    catch(PDOException $e) {
      echo "Error: " . $e->getMessage();
    }
    $conn = null;
    echo "</table>";
    ?>
    ```

### Condición `Order by`

=== "PDO"
    ```php hl_lines="31 35"
    <?php
    echo "<table style='border: solid 1px black;'>";
    echo "<tr><th>Id</th><th>Firstname</th><th>Lastname</th></tr>";

    class TableRows extends RecursiveIteratorIterator {
      function __construct($it) {
        parent::__construct($it, self::LEAVES_ONLY);
      }

      function current() {
        return "<td style='width:150px;border:1px solid black;'>" . parent::current(). "</td>";
      }

      function beginChildren() {
        echo "<tr>";
      }

      function endChildren() {
        echo "</tr>" . "\n";
      }
    }

    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      $stmt = $conn->prepare("SELECT id, firstname, lastname FROM MyGuests ORDER BY lastname");
      $stmt->execute();

      // set the resulting array to associative
      $result = $stmt->setFetchMode(PDO::FETCH_ASSOC);
      foreach(new TableRows(new RecursiveArrayIterator($stmt->fetchAll())) as $k=>$v) {
        echo $v;
      }
    } catch(PDOException $e) {
      echo "Error: " . $e->getMessage();
    }
    $conn = null;
    echo "</table>";
    ?>
    ```
    

## Borrar datos

=== "PDO"
    ```php hl_lines="13 16"
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

      // sql to delete a record
      $sql = "DELETE FROM MyGuests WHERE id=3";

      // use exec() because no results are returned
      $conn->exec($sql);
      echo "Record deleted successfully";
    } catch(PDOException $e) {
      echo $sql . "<br>" . $e->getMessage();
    }

    $conn = null;
    ?>
    ```

## Actualizar datos

=== "PDO"
    ```php hl_lines="12 15"
    <?php
    $servername = "localhost";
    $username = "username";
    $password = "password";
    $dbname = "myDBPDO";

    try {
      $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
      // set the PDO error mode to exception
      $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

      $sql = "UPDATE MyGuests SET lastname='Doe' WHERE id=2";

      // Prepare statement
      $stmt = $conn->prepare($sql);

      // execute the query
      $stmt->execute();

      // echo a message to say the UPDATE succeeded
      echo $stmt->rowCount() . " records UPDATED successfully";
    } catch(PDOException $e) {
      echo $sql . "<br>" . $e->getMessage();
    }

    $conn = null;
    ?>
    ```

