# Buscando datos
Para buscar información en una base de datos usamos el comando `SELECT`, que devuelve una tabla formada por los registros y columnas que cumplan las condiciones que indiquemos.

Para indicar **la tabla de donde leeremos** los datos usamos `FROM`:

```mysql
-- Devuelve todos los registros (duplicados incluidos)
SELECT * FROM tabla;

-- Devuelve todos los registros SIN duplicados
SELECT DISTINCT * FROM tabla;
```

!!! note "Contar entradas únicas"
    El operador `DISTINCT` se puede usar junto con `COUNT()` para contar el número de filas únicas en una tabla.

    === "MySQL"
        ```mysql
        SELECT COUNT(DISTINCT país) FROM clientes;
        ```
    === "MS Access"
        ```tsql
        SELECT COUNT(*) AS paisesUnicos
        FROM (SELECT DISTINCT pais FROM clientes);
        ```

## Filtrar resultados
Para filtrar los resultados según unas **condiciones** usamos el operador `WHERE`:

=== "Sintaxis"
    ```mysql
    SELECT columna1, columna2,...
    FROM tabla
    WHERE condicion;
    ```
=== "Ejemplo cadena"
    ```mysql
    SELECT * FROM clientes
    WHERE pais = 'Mexico';
    ```

    !!! important "Uso de comillas"
        Los datos de tipo _cadena_ deben ir siempre entrecomillados (simples o dobles).

=== "Ejemplo cadena"
    ```mysql
    SELECT * FROM clientes
    WHERE idCliente = 1;
    ```

!!! note "Filtrar en otros contextos"
    La cláusula `WHERE` se puede usar también al actualizar y borrar registros, con `UPDATE`, `DELETE` etc. En definitiva, en cualquier caso en el que se necesite imponer una _condición_.


### Operadores
Se admiten los siguientes operadores dentro de una cláusula `WHERE`:

| Operador  | Descripción                                                              |
|-----------|--------------------------------------------------------------------------|
| `=`       | Igual que                                                                |
| `>`       | Mayor que                                                                |
| `<`       | Menor que                                                                |
| `>=`      | Mayor o igual que                                                        |
| `<=`      | Menor o igual que                                                        |
| `<>`      | Diferente de (**NOTA**: algunas versiones permiten escribirlo como `!=`) |
| `BETWEEN` | Rango                                                                    |
| `LIKE`    | Según patrón                                                             |
| `IN`      | Dentro de un conjunto de valores indicado                                |

### Rango de valores: `BETWEEN`
Selecciona valores dentro de un rango proporcionado en la sentencia. Los valores pueden ser:

- **números**
- **texto** (siguiendo el _orden alfabético ASCII_).
- **fechas** (se pueden indicar entre comillas [`'1996-07-01'`] o entre almohadillas [`#01/07/1996#`])

El operador `BETWEEN` es **inclusivo**: los valores límite están incluidos en el resultado devuelto.

=== "Sintaxis"
    ```mysql
    SELECT nombre_columna(s)
    FROM table
    WHERE nombre_columna [NOT] BETWEEN valor1 AND valor2;
    ```

=== "Ejemplo"
    ```mysql
    SELECT * FROM productos
    WHERE precio BETWEEN 10 AND 20;
    ```
=== "Combinado con `IN`"
    ```mysql
    SELECT * FROM Products
    WHERE Price BETWEEN 10 AND 20
    AND CategoryID NOT IN (1,2,3);
    ```

### Aplicar un patrón: `LIKE`
Este operador nos permite usar una **expresión regular** para indicar un patrón de filtrado.

Existen dos _caracteres comodín_ que se usan a menudo para crear patrones con `LIKE`:

- `%`: 0, 1 o varios caracteres.
- `_`: un _único_ carácter

!!! note "Diferencias en MS Access"
    MS Access usa un asterisco (`*`) en lugar del porcentaje (`%`), y el signo de interrogación (`?`) en lugar del guión bajo (`_`).

=== "Sintaxis"
    ```mysql
    SELECT columna1, columna2, ...
    FROM tabla
    WHERE columna LIKE patrón;
    ```
=== "Ejemplo"
    ```mysql
    -- Cualquier valor que empiece por 'a'
    WHERE CustomerName LIKE 'a%';
    -- Cualquier valor que acabe en 'a'
    WHERE CustomerName LIKE '%a';
    -- Cualquier valor que tenga 'or' en cualquier posición
    WHERE CustomerName LIKE '%or%';
    -- Cualquier valor que tenga 'r' en segunda posición
    WHERE CustomerName LIKE '_r%';	
    -- Valores que empiecen por 'a' y tengan al menos 2 caracteres
    WHERE CustomerName LIKE 'a_%';	
    -- Valores que empiecen por 'a' y tengan al menos 3 caracteres
    WHERE CustomerName LIKE 'a__%';	
    -- Valores que empiecen por 'a' y acaben en 'o'
    WHERE ContactName LIKE 'a%o';	
    ```

### Lista de valores posibles: `IN`
Este operador es un atajo para introducir varias clásulas `OR`, puesto que nos permite asignar una lista de valores posibles:

=== "Sintaxis"
    ```mysql
    SELECT nombre_columna(s)
    FROM tabla
    WHERE nombre_columna IN (valor1, valor2, ...);
    -- O también
    SELECT nombre_columna(s)
    FROM tabla
    WHERE nombre_columna IN (SELECT sentencia);
    ```
=== "Ejemplo"
    ```mysql
    SELECT * FROM Customers
    WHERE Country IN ('Germany', 'France', 'UK');

    -- Si tenemos la lista de paises en otra tabla:
    SELECT * FROM Customers
    WHERE Country IN (SELECT Country FROM Suppliers);
    ```
    

### Más de una condición: operadores lógicos AND, OR, NOT

=== "AND"
    ```mysql
    SELECT columna1, columna2,...
    FROM nombre
    WHERE condicion1 AND condicion2 AND condicion3 ...;
    ```
    
=== "OR"
    ```mysql
    SELECT columna1, columna2,...
    FROM nombre
    WHERE condicion1 OR condicion2 OR condicion3 ...;
    ```
=== "NOT"
    ```mysql
    SELECT columna1, columna2,...
    FROM nombre
    WHERE NOT condicion;
    ```
=== "Ejemplos"
    ```mysql
    -- Si el pais es Alemania y la ciudad es Berlín o München
    SELECT * FROM Customers
    WHERE Country='Germany' AND (City='Berlin' OR City='München');
    -- Si el país es Alemania y no EEUU
    SELECT * FROM Customers
    WHERE NOT Country='Germany' AND NOT Country='USA';
    ```

### Filtrar resultados con funciones de agregación: `HAVING`
Si queremos imponer como condición de filtrado una operación de agregación, usando `COUNT`, `SUM`, `MAX` etc. no podemos usar `WHERE` sino que debemos añadir una cláusula extra con `HAVING`:

=== "Sintaxis"
    ```mysql
    SELECT nombre_columna(s)
    FROM tabla
    WHERE condicion
    GROUP BY nombre_columna(s)
    HAVING condicion
    ORDER BY nombre_columna(s);
    ```
=== "Ejemplo"
    ```mysql
    /*
     Número de clientes en cada país;
     Sólo recoge los de países con más de 5 clientes
    */
    SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    HAVING COUNT(CustomerID) > 5;

    -- igual que el anterior, ordenado de mayor a menor
    SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    HAVING COUNT(CustomerID) > 5
    ORDER BY COUNT(CustomerID) DESC;
    ```
=== "Con `JOIN`"
    ```mysql
    -- Empleados que han registrado más de 10 pedidos
    SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
    FROM (Orders
    INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID)
    GROUP BY LastName
    HAVING COUNT(Orders.OrderID) > 10;

    -- Muestra si los empleados "Davolio" o "Fuller" han registrado más de 25 pedidos
    SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
    FROM Orders
    INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
    WHERE LastName = 'Davolio' OR LastName = 'Fuller'
    GROUP BY LastName
    HAVING COUNT(Orders.OrderID) > 25;
    ```
    
## Ordenar resultados
Los registros recogidos por un `SELECT` se pueden ordenar con una cláusula `ORDER BY`, en sentido ascendente (`ASC`) o descendente (`DESC`):

```mysql
SELECT columna1, columna2, ...
FROM tabla
ORDER BY  columna1, columna2, ... ASC|DESC;
-- se pueden establecer sentidos diferentes en columnas diferentes
SELECT * FROM clientes
ORDER BY pais ASC, nombre DESC;
```

## Agrupar resultados
Para agrupar filas con los mismos valores usamos la cláusula `GROUP BY`. Esta suele usarse junto con **funciones de agregado** (`COUNT, MAX, MIN, SUM, AVG`):

=== "Sintaxis"
    ```mysql
    SELECT nombre_columna(s)
    FROM tabla
    WHERE condicion
    GROUP BY nombre_columna(s)
    ORDER BY nombre_columna(s);
    ```

=== "Ejemplo"
    ```mysql
    SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    ORDER BY COUNT(CustomerID) DESC;
    ```
=== "Con `JOIN`"
    ```mysql
    -- Número de pedidos enviados por cada distribuidor
    SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders 
    FROM Orders LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
    GROUP BY ShipperName;
    ```

## Combinar registros de varias tablas: JOINS
