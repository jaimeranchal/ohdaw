# Bases de Datos SQL

!!! info "Gestión de bases de datos"
    SQL es sólo un lenguaje que permite _definir_ y _manipular_ datos organizados siguiendo un **esquema relacional**. Sin embargo, hay más formas de organizar una base de datos, y herramientas para diseñar la estructura y organización de los mismos. En [este enlace](https://gestionbasesdatos.readthedocs.io/es/stable/Tema1/index.html) lo explican bastante bien.

## Sintaxis

Sigue las convenciones de otros lenguajes:

- cada _orden_ o _sentencia_ acaba en **punto y coma** (`;`).
- una sentencia larga se puede dividir en varias líneas

```sql
-- Comentarios
KEYWORD nombre|parámetro;
```

### Operadores

#### Aritméticos

| Operador | Descripción    |
|----------|----------------|
| `+`      | Suma           |
| `-`      | Resta          |
| `*`      | Multiplicación |
| `/`      | División       |
| `%`      | Módulo         |

#### Comparación

| Operador | Descripción       |
|----------|-------------------|
| `=`      | Igual que         |
| `>`      | Mayor que         |
| `<`      | Menor que         |
| `>=`     | Mayor o igual que |
| `<=`     | Menor o igual que |
| `<>`     | Diferente de      |

####  Lógicos

| Operador  | Descripción                                                       |
|-----------|-------------------------------------------------------------------|
| `ALL`     | _True_ si todos los subqueries cumplen la condición               |
| `AND`     | _True_ si todas las condiciones unidas son _true_                 |
| `ANY`     | _True_ si alguna de las subqueries es _true_                      |
| `BETWEEN` | _True_ si el valor está dentro del rango                          |
| `EXISTS`  | _True_ si la subquery devuelve algún resultado                    |
| `IN`      | _True_ si el operando es igual que uno de una lista de resultados |
| `LIKE`    | _True_ si el operando coincide con un patrón                      |
| `NOT`     | Devuelve resultados si la condición _no se cumple_                |
| `OR`      | _True_ si alguna de las subqueries es _true_                      |
| `SOME`    | _True_ si alguna subquery cumple la condición                     |

!!! info "Comodines"
    Con `LIKE` y `NOT LIKE` se pueden usar **expresiones regulares**.

### Comentarios

#### En una sóla línea
```sql
-- una query normal
SELECT * FROM usuarios;
```

#### En varias líneas
```sql
/*
Esta es mi sentencia con select.
Recoge todas las filas de datos de la tabla usuarios
*/
SELECT * FROM usuarios;
```

## Tipos de datos

### Texto

| Nombre            | Descripción                                    |
|-------------------|------------------------------------------------|
| `CHAR(size)`      | Cadena de longitud fija (0-255; 0 por defecto) |
| `VARCHAR(size)`   | Cadena de longitud variable (0-65535)          |
| `TEXT(size)`      | Cadena de longitud variable (0-65535)          |
| `ENUM(a,b,c...n)` | Array de cadenas (hasta 65535 posiciones)      |

!!! note "Ejemplo de `ENUM()`"
    ```sql
    CREATE TABLE camisetas (color ENUM('rojo', 'verde', 'azul'));
    ```

### Numéricos

| Nombre             | Descripción                                                                                         |
|--------------------|-----------------------------------------------------------------------------------------------------|
| `BIT(size)`        | Por defecto 1; rango 1-64                                                                           |
| `BOOLEAN` o `BOOL` | En la práctica, fija el valor a `TINYINT(1)`. 0 es _false_                                          |
| `INT(size)`        | Entero entre 0 y 4294967295 (sin signo); _size_ es el número de caracteres (1-255)                  |
| `FLOAT(p)`         | Número decimal; si la precisión está entre 0 y 24 se considera FLOAT, si está entre 25 y 53, DOUBLE |
| `DOUBLE(size,p)`   | _size_: número total de dígitos; _p_: número de decimales                                           |

!!! note "Nombres alternativos"
    `INTEGER` y `DECIMAL` son lo mismo que `INT` y `DOUBLE`.

### Fechas

| Nombre | Descripción|
|--------|------------|
|`DATE`|Formato AAAA-MM-DD|
|`DATETIME(fsp)`|Fecha y hora en formato AAAA-MM-DD hh:mm:ss|
|`TIMESTAMP(fsp)`|Marca de tiempo estilo Unix: número de segundos desde la época Unix (1 de enero de 1970 a las 00h)|
|`TIME(fsp)`|Hora en formato hh:mm:ss|
|`YEAR`|Año, entre 1901 y 2155|

!!! tip "Fecha y hora actual"
    Si se añade `DEFAULT` y `ON UPDATE` a la definición de una columna de tipo `DATETIME` automáticamente actualizará la fecha y hora a las actuales.

## Funciones
### Para cadenas

- `ASCII`: devuelve el valor ASCII del carácter indicado
- `CHAR_LENGTH`: longitud de caracteres de una cadena
- `CONCAT`: une dos cadenas.
- `FIELD`: devuelve el valor del índice relativo a la posición de un valor dentro de una lista de valores.
- `FIND IN SET`: Devuelve la posición de una cadena en una lista de cadenas.
- `FORMAT`: Cuando se le pasa un número, devuelve el número formateado con comas (3400 &gt; 3,400)
- `INSERT`: Permite insertar una cadena dentro de otra en una _posición_, con una _longitud determinada_.
- `INSTR` y `POSITION`: Devuelve la posición de la primera vez que aparece una cadena dentro de otra.
- `LCASE` o `LOWER`: convierte a minúsculas
- `MCASE` o `UPPER`: convierte a mayúsculas
- `LEFT` y `RIGHT`: Extrae el número de caracteres indicado hacia la izquierda/derecha y los devuelve como otra cadena.
- `SUBSTR`, `SUBSTRING` y `SUBSTRING_INDEX`: Extra una subcadena desde cualquier posición.
- `MID`: Extra una subcadena desde cualquier posición.
- `TRIM`: Elimina los espacios en blanco antes y después de la cadena.
- `LTRIM` y `RTRIM`: Elimina los espacios en blanco a la izquierda/derecha de la cadena
- `REPEAT`: Repite una cadena
- `REPLACE`: reemplaza una subcadena con otra.
- `REVERSE`: le da la vuelta a la cadena
- `SPACE`: devuelve una cadena de tantos espacios como se le pida.
- `STRCMP`: Compara dos cadenas.

### Para números

- `ABS`: Devuelve el valor absoluto (sin signo) de un número
- `CEIL` y `FLOOR`: redondean un número decimal al alza o a la baja
- `ROUND`: redondea un decimal al número indicado de cifras decimales.
- `TRUNCATE`: corta un número en el número de decimales indicados.
- `GREATEST`y `LEAST`: valor más altoo más bajo de un conjunto
- `RAND`: devuelve un número aleatorio
- `EXP`, `POW`, `POWER`: potencia de un número
- `SQRT`: raíz cuadrada de un número
- `PI`: devuelve Pi.

!!! note "Funciones de agregación"
    Son las más habituales para **manejar grupos de resultados** obtenidos mediante la clásula `GROUP BY`:

    - `COUNT`: número de resultados de un `SELECT`.
    - `MAX` y `MIN`: número más alto o más bajo de un conjunto
    - `SUM`: devuelve el valor combinado de una serie de valores.
    - `AVG`: Devuelve la media

### Para fechas

??? "Crear objetos de tipo fecha"
    - `STR_TO_DATE`: Crea una fecha a partir de una cadena y un formato.
    - `MAKEDATE`: Crea una fecha a partir del año y número de días y la devuelve 
    - `MAKETIME`: Crea un "tiempo" a partir de la hora, minuto y segundos dados y la devuelve 
    - `DATE`: Extrae la fecha a partir de una expresión de fecha y hora
    - `DATE_SUB` o `SUBDATE`: Extrae el intervalo (p.e. 10 días) hasta una fecha dada (20/01/20) y devuelve el resultado (20/01/10).
    - `FROM DAYS`: Revuelve la fecha a partir de la fecha numérica dada.
    - `SEC_TO_TIME`: Devuelve el "tiempo" a partir de los segundos indicados.


??? "Obtener datos"
    - `EXTRACT`: extrae la parte indicada de una fecha:
        - **Número**
            - `DAY`, `HOUR`, `MINUTE`, `SECOND`, `MONTH`, `QUARTER`, `YEAR` o `TIMESTAMP`
            - `DAYOFTHEWEEK`: posición del índice del día de la semana (domingo = 0).
            - `DAYOFYEAR`: día del año
            - `LAST DAY`: último día del mes de la fecha dada.
            - `WEEKDAY`: número del día de la semana para la fecha dada.
            - `WEEK` y `WEEKOFYEAR`: número de la semana en el año.
            - `YEARWEEK`: año y número del mes.
            - `TO_DAYS`: número total de días que han pasado desde `00-00-0000` hasta la fecha indicada
        - **Nombre**
            - `MONTHNAME`
            - `DAYNAME`
    - **Diferencias**:
        - `PERIOD_DIFF`: diferencia entre dos periodos determinados
        - `SUBTIME`: sustrae un intervalo de tiempo (p.e. 02:00) de una hora o fecha y hora y devuelve el resultado.
        - `TIME`: "tiempo" desde otro tiempo determinado.
        - `TIMEDIFF`: Diferencia entre dos expresiones de tiempo o tiempo y hora.
        - `DATEDIFF`: Número de días entre dos fechas
    - **Ahora**
        - `CURDATE` y `CURRENT_DATE`: fecha actual
        - `CURTIME` y `CURRENT_TIME`:
        - `CURRENT_TIMESTAMP`:
        - `LOCALTIME`, `LOCALTIMESTAMP`, `NOW` o `SYSDATE`

??? "Modificar fechas"
    - `ADDDATE` o `DATE_ADD`: añade un intervalo de tiempo a una fecha y devuelve la fecha resultante.
    - `ADDTIME`: añade un intervalo de tiempo (horas, minutos, segundos) a un "tiempo" y devuelve el resultado.
    - `PERIOD_ADD`: lo mismo pero con _periodos_.
??? "Formato"
    - `DATE_FORMAT`: aplica el formato indicado mediante patrón a una fecha.
    - `TIME_FORMAT`: lo mismo pero con el tiempo.
    - `TIME_TO_SEC`: Convierte el tiempo en segundos y lo devuelve

### Otros

- `CAST`: Convierte un tipo de dato en otro
- `COALESCE`: devuelve el primer valor no-nulo de una lista
- `CONNECTION_ID`: devuelve la ID de conexión única de la conexión actual.
- `CONV`: converte un número de un sistema a otro.
- `CONVERT`: convierte el valor dado en el tipo de dato proporcionado o en el juego de caracteres (?)
- `CURRENT_USER`, `USER`, `SYSTEM_USER` y `SESSION_USER`: devuelve el usuario y hostname que se ha usado para conectarse al servidor
- `DATABASE`: nombre de la base de datos actual
- `LAST_INSERT_ID`: id autoincrementado de la última fila añadida o actualizada.
- `IFNULL`: Si la expresión proporcionada es `NULL`, devuelve el valor.
- `ISNULL`: Si la expresión es `NULL`, devuelve 1; en caso contrario, 0.
- `NULLIF`: Compara dos expresiones. Si son iguales, devuelve `NULL`; en caso contrario devuelve la primera expresión.

## Claves

En SQL, las claves primarias y foráneas se incluyen como **restricciones**. Una tabla:

- Debe tener una clave primaria
- Puede tener una o varias claves foráneas

!!! note "Formas de declarar las claves"
    SQL es un estándar que luego se ha implementado en varios "dialectos", como pueden ser MySQL, Oracle, SQL Server, PostgreSQL...Esto lleva a que existan varias diferencias en la forma de declarar las llaves. En el capítulo dedicado al [Lenguaje de Definición de Datos](./DDL.md) se explican con más detalle.

### Primaria

La clave primaria permite identificar cada registro de forma inequívoca. Sólo puede haber _una_ por tabla (aunque puede consistir en un **conjunto de columnas**). Cada valor de una clave primaria debe ser **único**.

Cuando se crea la tabla:

```mysql
CREATE TABLE usuarios (
id int NOT NULL AUTO_INCREMENT,
nombre_propio varchar(255),
apellido varchar(255) NOT NULL,
direccion varchar(255),
email varchar(255),
PRIMARY KEY(id)
);
```

Si la queremos añadir después:

```mysql
ALTER TABLE usuarios
ADD PRIMARY KEY (first_name);
```
    
### Foránea
Una clave foránea sirve para enlazar dos tablas:

- La tabla que contiene la clave foránea se llama **tabla hijo**
- La tabla a la que hace referencia la clave foránea se llama **tabla padre**

```mysql
CREATE TABLE pedidos(
id int NOT NULL,
user_id int,
product_id int,
PRIMARY KEY (id),
FOREIGN KEY (user_id) REFERENCES usuarios(id),
FOREIGN KEY (product_id) REFERENCES productos(id)
);
```

Para alterarla después:

```mysql
ALTER TABLE pedidos
ADD FOREIGN KEY (user_id) REFERENCES usuarios(id);
```



